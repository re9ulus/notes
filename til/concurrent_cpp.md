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
