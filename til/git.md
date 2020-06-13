# Git notes

Нагядное дерево коммитов
```
git log --all --graph --decorate
git log --all --graph --decorate --oneline
```

diff с ревизией
```
git diff <revision-hash> <./filename>
```

diff двух ревизий
```
gid diff <revision-hash-1> <revision-hash-2> <./filename>
```

diff с кешированным состоянием
```
git diff --cached
```

дополнительная информация про ветки
```
git branch -vv
```

branch + checkout
```
git checkout -b <name>
```

интерактивно выбрать, какие изменения в файле закоммитить
```
git add -p <filename>
; s - split changes
; y - yes
; n - no
; ? - help
```
