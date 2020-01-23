Случайные заметки на разные темы о том что я узнал или вспомнил из давно забытого и такого, что не хочется потерять.


### Использование iota в C++

Периодически бывает нужен векто чисел заполненный по порядку. Вместо того, чтобы делать это в цикле можно использовать

```
#include <numeric>
std::vector<int> data(n);
std::iota(data.begin(), data.end(), 0);
```

Пояилось в C++11 [link](https://en.cppreference.com/w/cpp/algorithm/iota).


### Копирование части вектора

Чтобы скопировать последовательный кусок вектора не обязательно использовать цикл

```
std::vector<int> a(10);
//
std::vector<int> b(a.begin() + 1, a.begin() + 5); // [1, 5)
```


### Компораторы с помощью tie

Иногда бывает нужно сделать компаратор по нескольким полям. Если нужно просто сравнить поля, можно воспользоваться `std::tie`.

```
bool CustomCMP(const MyType& lhs, const MyType& rhs) {
    return std::tie(lhs.field2, lhs.field1, lhs.field3) <
           std::tie(rhs.field2, rhs.field1, rhs.field3);
}
```
[link](https://en.cppreference.com/w/cpp/utility/tuple/tie)

### Семантика указателей

`unique_ptr` - единоличное владение
`shared_ptr` - использование разными объектами
`week_ptr` - наблюдение

`week_ptr` не может бысть создан на сыром указателе, для создания нужно использовать `shared_ptr`. Обладает двумя полезными методами

- `expired` - проверить жив ли объект, на который указывает указатель.
- `lock` - продлить жизнь объекта (вернуть `shared_ptr`).

### Компиляция C++ проекта

На самом высоком уровне: Каждый `*.cpp` файл (`Translation unit`) компилируется отдельно, получается `*.o` (object file). Все объектные файлы собираются вместе и получается исполняемый файл, который можно запустить `./app`.

- `*.cpp -> *.obj` - компиляция
- собрать `*.obj` - линковка

`*.h` файлы _никогда_ не подаются на вход компилятору, они используются `*.cpp` файлами.

```
; -c сгенерировать объектный файл
g++ file.cpp -c  ; -> file.o
; линковщику можно просто передать объектные файлы
g++ file.o file2.o fileN.o -o run -> исполняемый ./run
```

### Pointer implementation example
Позволяет ускорить сборку. Tradeoff - кадый вызов функции превращается в два вызова. С шаблонами все это не работает, т.к. реализация должна быть в `.h`.
```
// item.h
class Item {
public:
    Item(std::string);
    DoThing();
    ~Item();  // Need to specify destructor because of incomplete type
private:
    class ItemImpl;
    std::unique_ptr<ItemImpl> pimpl_;
};

// item.cpp
Item::Item(std::string str) {
    pimpl_ = std::make_unique<Item::ItemImpl>(std::move(str));
}

Item::DoThing() {
    pimpl_->DoThing();
}

class Item::ItemImpl {};

Item::~Item() {}
```
