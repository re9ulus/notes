# Шаблоны

```
template<typename T>
void Swap(T& x, T& y) {
    T t = x;
    x = y;
    y = t;
```

Каждое инстанцирование пораждает отдельную функцию. В шаблонную часть нужно выносить только части, которым это действительно необходимо.

Компилятор умеет выводить типы самостоятельно.

Специализация шаблонов
```
// Говорим компилятору, что для типа std::string нужно сгенерировать другую функцию
template<>
void Swap<std::string>(std::string& x, std::string& y) {
    std::string t = std::move(x);
    x = std::move(y);
    y = std::move(x);
}
```

Порядок поиска по имени:
- обычные функции
- специализированные шаблонные функции
- шаблонные функции

Если шаблон предполагается куда-либо импортировать, его определение должно полностью содержаться в .h файле.

#### Шаблонные классы
```
template <typename T1, typename T2>
class Pair {
public:
    Pair(T1 a, T2 b) { ... }
private:
    T1 a;
    T2 b;
}
```

### STL

#### Контейнеры

- Последовательные
  - std::vector
  - std::array - в отиличие от вектора хранит данные на стеке
  - std::deque - push\_front, emplace\_front, pop\_front
  - std::list
  - std::forward\_list
- Ассоциативные
  - std::set, std::map - rb-tree под капотом
  - std::multiset, std::multimap
  - std::unordered\_set, std::unordered\_map
  - std::unordered\_multiset, std::unordered\_multimap
- Адаптеры
  - std::stack
  - std::queue
- std::priority\_queue
