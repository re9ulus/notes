### Assm

### x86

- Годный слайд с названиями регистров

### Move

```
;eax = 3
MOV EAX, 3
```

```
; eax = ebx
MOV EAX, EBX
```

```
; eax += 5
ADD EAX, 5

; eax += ebx
ADD EAX, EBX
```

Intel: ADD Destination

```
SUB
```

```
Shift Left
; eax <<= 2
; eax *= (1 << 2)
SHL EAX, 2
SAL EAX, 2
; eax <<= cl
; eax *= (1 << cl)
SHL EAX, CL
SAL EAX, CL
```

Shift artithmetic right
```
int32_t eax
eax >>= 2
SAR EAX, 2
```

Shift arithmetic - сдвиг распространяет знак [sign shift]
Logical arithmetic right

Rotate Over Left
Rotate Over Right
```
ROR EAX, 2
```

Unsigned multiplication
[edx:eax] = eax * rm  ; rm - register or memory
MUL rm

Signed multiplication
IMUL RM

Unsigned division  # TODO: Посмотреть на слайде
eax 

Отрицание neg

Logcal AND
eax &= 

Or
...


# ---
goto label

JMP label

Conditional Jump
if (eax) goto label
TEST EAX, EAX
JNZ label


# Флаги
ZF - zero flag (result is zero)
SF - signed flag (result sign)
CF - carry flag (unsigned overflow)
OF - overflow flag (signed overflow)
# Хранятся в специальном регистре FLAGS


### CF
CF равен 1 при переполнении беззнакового сложения или вычитания
Если переполнения не было, флаг равен 0

### OF
OF равен 1 при переполнении знакового сложения или вычитания

### Оператор сравнения
if (eax?ebx)
где ? означает сразу ==,!=,>,<,>=,<=
CMP eax, ebx ; как вычитание, только без записи перхода


je | if equal(ZF=1)
js | if sign(SF=1)
...  # TODO: Посмотреть другие примеры со слайда

ja | if above (CF = 0 and ZF = 0)
...

### Логическое сравнение
if (eax&0xff)
TEST EAX, 0xff  ; как and без присваивания результата

### Условное присваивание
eax = cond?1:0
SETcc EAX

### Условное перемещение
eax = cond ? ebx : eax
...

### Инструкции использующие CF

# Обращение к памяти
Многие инструкции работают не только с регистрами, но и с памятью
В инструкции нужно указать адрес
Адрес для x86:
addr = base + index * scale + disp

base, index - регистры; scale - 1,2,4,8; константа

Пример:
a[i]
EBX = a
ECX = i
Addl(%ebx, %eax, 4)

Вычисление адреса
```
;eax = base + index * scale + disp
LEA EAX, [base + index * scale + disp]# TODO: Посмотреть про LEA в слайдах
```

## Стек
Отдельная область памяти
На верхушку указывает ESP
Стек на x86 растет вниз

Запиь всех регистров на стек
- Записать все регистры на стек (включая ESP): PUSHA
- Восстановить значение всех регистров со стека (игнорируя ESP): POPA

Регистры x86_32

Регистры x86_64


### Чтение ассемблера
AT&T vs Intel
movl $0, %eax => mov eax, 0


В Linux используется синтаксис AT&T.
А в документации Intel: Intel (внезапно!)

AT&T 
Команды оканчиваются суффиксом:
b byte (8bit)
w word (16bit)
l long word (32)
...


### Программа на ассемблере


### Вызов функции и возврат из нее
- Вызов делается через CALL
- Возврат RET
...

### Фреймы

### Соглашения о вызовах
System V Application Binary Interface   

### интерфейс системных вызовов
syscall


### Компилятор
gcc -S

Дизасм
objdump
-drwC

транслятор GAS (Gnu Assebmler)


### Ассемблерные вставки
asm


### GDB
bt - стек вызовов
info registers
info locals
info frame
frame idx -- перейти в кадр стека idx
p -- print
p n -- print n

b -- breakpoint
b main.c:9
b walk  -- walk - имя функции
info breakpoints
del 1 -- брейкпоинты можно удалять по номеру

s -- step, заходит внутрь функций
n -- next, не заходит внутрь

si -- step instruction, следующая ассм инструкция с заходом внутрь функции
ni -- next instruction, следующая ассм инструкция без захода внутрь функции

record 
rstep
rnext
