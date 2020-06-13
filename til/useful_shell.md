Какая ос установлены
```
hostnamectl
lsb_release -a
cat /etc/* -releases
```

Сколько CPU и какие
```
cat /proc/cpuinfo
```

Сколько памяти с свопа
```
free -h
```

Сколько дисков и места
```
df -h
```

`lsof` - показать открытые файлы
```
; Найти процессы использующие файл
lsof path/to/file

; Найти процессы слашающие tcp на 80 порту
lsof -i tcp:80
```
