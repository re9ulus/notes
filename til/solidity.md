# Про Solidity

Минимальный пример контракта
```
pragma solidity >=0.5.0 <0.6.0;  // версия обязательна

contract SampleContract {
}
```

Сложные типы данных
```
struct Person {
    string name;
    uint age;
}
```

Массивы
```
// Fixed length
uint[42] fixedArray;

// dynamic
uint[] dynamicArray;
Person[] people;

people.push(Person("Mr.X", 42))
```

Функции
```
// functions are public by default
// _name - by ref
function sampleFunc(string memory _name, uint _age) public {
}

function _privateFunc(uint _magicNumber) private view returns (uint){
}
```

Function modifiers
```
view - don't change state
pure - don't read state
```

Events
```
event SomethingHappens(uint a, uint b);

function do(uint _a, uint _b) public returns (uint c) {
    uint result = _a + _b;
    emit SomethingHappens(_a, _b);
    return result;
}

MyContract.SomethingHappens(function(error, result) {
    // do something
})
```

Mapping
```
mapping (address => uint) public Balance;
```
