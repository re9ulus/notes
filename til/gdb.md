# Как готовить gdb


Запустить
```
gdb ./a.out
run
```

Ключевые команды
```
bt              ; backtrace стек вызовов
info registers
info locals
info frame
frame idx       ; перейти в кадр стека idx
p n             ; print n

b               ; breakpoint
b main.c:9      ; breakpoint on the line
b func_name     ; breakpoint on function name
info breakpoints
del 1           ; удалить брейкпоинт с номером 1

s               ; step, шаг с заходом внутрь функции
n               ; next, шаг без захода в функцию

si              ; step instruction, следующая инструкция с заходом в функции
ni              ; next instruction, без захода
```

Time-travel debugging
```
record
rstep
rnext
```

Полезное
```
Включить дополнительный интерфейс: ctrl+x a
```

TODO: Написать более подробно про time-travel debugging


Полезные плагины:

Python Exploit Assistance for GDB [git](https://github.com/longld/peda)

Установка:
```
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit
echo "set disassembly-flavor att" >> ~/.gdbinit
echo "DONE! debug your program with gdb and enjoy"
```

Ссылки:

- Великолепное видео: [Give me 15 minutes & I'll change your view of GDB](https://www.youtube.com/watch?v=PorfLSr3DDI&t)
