На данный момент заметки по мотивам книжки `Concurrency programming in c++ in action`.

### Hello world

```
#include <iostream>
#include <thread>

void hello() {
    std::cout << "hellow world\n" << std::endl;
}

int main() {
    auto t = std::thread(hello);
    t.join();
    return 0;
}
```

### Управление потоками

Ключевая библиотека: `std::thread`. Запуск потока сводится к конструированию объекта `std::thred`. Работает с любым `callable` типом.

Функция
```
void some_function();
std::threadr{some_function};
```

Класс
```
class Task {
public:
    void operator() () const {
        DoSomething();
    }
};
std::thread{Task()};
```

Лямбда-выражение
```
std::thread{[](){ std::cout << "Hello\n"; };
```

Переданный объект _копируется_ в память нового потока.

После создание потока можно отсоединить его
```
my_thread.detach();
```

Или ожидать завершения
```
my_thread.join();  // можно вызвать только 1 раз. Проверить можно через joinable(); 
```

### Использование RAII

С момента запуска потока могут произойти непредвиденные ситуации. Например исключение или ошибка, из-за которой ресурс используемый в дочернем потоке станет недоступен или подвиснет. Одним из способов решения проблемы является `RAII`.
```
class Guard P
    std::thread t;
public:
    explicit Guard(std::thread t_) : t(std::move(t_)) {}
    ~Guard() {
        if (t.joinable()) {
            t.join();
        }
    }
    Guard(const ThreadGuard&) = delete;
    Guard& operator=(const ThreadGuard&) = delete;
};

{
    int local_state = 42;
    Guard(std::thread(functor(local_state)));  // Гарантированно будет вызван дестркутор
    // some other code
}
```

### Передача аргументов

Аргументы передаются в качестве дополнительных аргументов в `std::thread`. По умолчанию они _копируются_ в память нового потока, *даже в том случае, когда функция ожидает ссылку*. Нужно быть осторожным, есть вероятность получить неожиданные спецэффекты.
```
void SomeFunction(std::string& str);

char buffer[256];
// Передали указатель char*, который будет приведен к строке
// Есть вероятность что буфер будет уничтожен до преобразования в std::string и все взорвется
std::thread t(SomeFunction, buffer);
// Правильно
std::thread t(SomeFunction, std::string(buffer));
```

Если хотим получить ссылку нужно использовать `std::ref`.
```
Data data;
// Будет передана ссылка на data
std::thread(Func, std::ref(data));
```

Можно передать функцию-член, нужно первым объектом передать указатель на объект класса
```
class X {
public:
    void DoWork();
};

X x_obj;
std::thread t(&X::DoWork, &x_obj);
```

Отдельную осторожность необходимо проявлять с перемещающими конструкторами и операторами присваивания.

#### Передача владения потоком

Потоки являются перемещаемыми, но некопируемыми.
```
std::thread t1(SomeFunction);
std::thread t2 = std::move(t1);
t1 = std::thead(SomeOtherFunction);
t1 = std::move(t2);  // Ошибка, т.к. с t1 уже связан другой поток
```

Потоки можно передавать/возвращать из функций и хранить в коллекциях поддерживающих перемещение.
```
std::thread GoThread() {
    return std::thread(SomeFunc);
}

void AcceptThread(std::thread t) {}  // ! По значению
std::thread t(SomeFunc);
AcceptThread(std::move(t));
```

#### Мьютексы

Лучше использовать с `RAII`
```
#include <mutex>
std::mutex my_mutex;

void SomeFunc() {
    std::lock_guard<std::mutex> guard(my_mutex);  // mutex будет освобожден в деструкторе
}
```

Цитата:
```
Не передавайте указатели и ссылки на защищенные данные за пределы области видимости блокировки никаким способом, будь то возврат из функции, сохранене в видимой извне памати или передача в виде аргумента пользовательской функции.
```

#### Захват нескольких мьютексов

Здравый смысл: захватывать мьютексы в одном и том же порядке.

Стандартная функция `std::lock`.

```
std::lock(m1, m2);  // Захватили мьютексы
// Обеспечили освобождение. adopt_lock говорит что мьютекс уже захвачен и guard должен только принять владение мьютексом.
std::lock_guard<std::mutex> guard_1(m1, std::adopt_lock);
std::lock_guard<std::mutex> guard_2(m2, std::adopt_lock);
```

Попытка захватить уже захваченный мьютекс ведет к UB. Для этого есть специальный `recursive_mutex`.

#### Как избегать блокировок

- Не ждать поток, если есть малейшая вероятность что он ждет тебя
- Избегать вложенных блокировок - на захватывать новый мьютекс, если уже завладел другим.
- Не вызывать пользовательский код под мьютексом
- Захватывать мьютексы в фиксированном порядке
- Использовать иерархию блокировок

#### Защита редко обновляемых данных

Начиная с 17 стандарта в языке есть `std::shared_mutex`. Для него можно брыть `shared_lock` обеспечивая одновременное чтение из разных потоков. И `unique_lock` обеспечивающий одиноличное владение мьютексом, для внесения изменений. Объект блокирующий для изменения ждет пока все читающие потоки закончат свою работу. До 17 стандарта был `boost:shared_mutex`.

#### Разное

Узнать число потоков которые могут честно бежать параллельно
```
std::thread::hardware_concurrencty();
```

Получить идентификатор текущего потока
```
std::this_thread::get_id();
```

### Синхронизация операций

#### Condvar

Для ожидания есть `std::conditional_variable` и `std::conditional_variable_any`. Первый класс работает только с `std::mutex`. Второй - с любым `mutex`-подобным классом.

Пример
```
std::mutex mt;
std::conditional_variable cond;

// thread 1
std::lock_guard<std::mutex> lock(mt);
queue.push(data);
cond.notify_one();

// thread 2
std::unique_lock<std::mutex> lock(mt);
// Нужно передать lambda с ожидаемым условием
cond.wait(
    lock, [], {return !queue.empty();});
```

#### Future

`future` бывает двух видов, неразделяемое `std::future<>` и разделяемое `std::shared_future<>`. Если ассоциированных с объектом данных нет, можно использовать `std::future<void>`.

Пример
```
std::future<int> answer = std::await(find_answer);
...
answer.get();
```

#### Ассоциирование задачи с будущим результатом

Можно использовать `std::packaged_task<>` и `std::promise`.
