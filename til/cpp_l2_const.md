
По мотивам второй лекции по C++ в ШАД

### Const

- keyword
- type qualifier
- Можно дописывать к типам объявляемых переменных
- Компилятор не даст изменить значение переменной объявленной `const`

```
const int y = 0;
y = 1;  // error
```

### Reference

- Способ создать псевдоним для переменной
- Для обозначения используется амперсанд (&)

```
int x = 0;
int& y = x;

y = 1;
std::cout << x;  // 1
```

std::unordered_map<std::string, int> aVeryLongMap;
std::string aVaryLongKey = "some_thing";
int& value = aVeryLongMap[aVeryLongKey];
value = 1;

Свойтва ссылок:
- Ссылка должна быть инициализирована при создании
- Ссылка может быть связана с переменной только один раз


#### Константные ссылки
```
int x = 0;
const int& ref = x;
ref = 1;  // Error
```

### Оператор взятия адреса (address-of)
```
int x = 42;
std::cout << &x << std::endl;
```

### Значения амперсанда
```
// Побитовое И
auto y = x & 0xFFFFFFFF;

// Объявление ссылки
int& y = x;

// Вычисление адреса
auto addr = &x;
```

### Указатели
```
int x = 100500;
int* px = &x;

std::cout << x << std::endl;
std::cout << y << std::endl;
```

#### Оператор разыменования
```
int x = 100500;
int* px = &x;
int y = *px;

std::cout << x << std::endl;
std::cout << y << std::endl;
```

#### Значения звездочки

#### Указатели и ссылки на константы

#### Константный указатель
