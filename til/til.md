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
