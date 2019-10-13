## Про LISP

По мотивам [Practical Common Lisp](http://www.gigamonkeys.com/book/)  

Hello-world
```
(defun hello-world () (format t "Hello, world"))
```

#### Функции

Определение функции
```
(defun name (parameter*)
    "Optional docstring."
    body-from)
```

Пример
```
(defun multiply (a b)
    "Multiply two numbers"
    (format t "Multiply ~d and ~d.~%" a b)
    (* a b))
```

##### Параметры

Опциональные
```
;; По умолчанию примут значение NIL
(defun foo (a b &optional c d) (list a b c d))

;; Указание значений по умолчанию
(defun foo (a b &optional (c 42) (d 3.14) (list a b c d)))

;; Можно использовать переданные параметры
(defun rectangle-area (a &optional (b a) ) (* a b))
```

Список параметров
```
;; Можно передать любое число параметров используя Rest параметры
(defun foo (a &rest values) (list a values))

;; Пример
(defun + (&rest values) ...)
```

Keyword параметры
```
(defun foo (&key a b c) (list a b c))

;; Со значениями по умолчанию
(defun foo (&key :a 42 :b 10) (list a b c))

;; Пример вызова
(foo :a 10)
```

Разные типы параметров можно совмещать, но делать это нужно аккратно. Можно получить странные спецеффекты при одновременном использовании, например `&rest` и `&key` аргументов.

##### Возвращаемые значения

Функция возвращает последнее вычисленное выражение. Если нужно вернуться из середины функции нужен `RETURN-FROM`
```
(RETURN-FROM block-name value-to-return)
```

##### Higher Order Functions

Функциональный объект можно получить с помощью `FUNCTION` или более короткой версии `#'func-name`

```
;; Опеределение функции
(DEFUN add (a b) (+ a b))

;; Функциональный объект для последующего использования
(FUNCTION add)

;; аналогично
#'add
```

Для вызова такого объекта можно использовать `FUNCALL` либо `APPLY`.

`FUNCALL` предполагает что нам известно число аргументов.
```
(FUNCALL #'add 3 5)
```

**TODO**: Дописать про `APPLY`.

##### Анонимные функции
```
((lambda (a b) (+ a b)) 2 3)

(funcall #'(lambda (a b) (+ a b)) 2 3)
```

####  Переменные

lisp поддерживает два типа переменных `lexical` и `dynamic`.

Создание новых переменных
```
;; Функция
(defun add (a b) (+ a b))

;; Блок с переменными
(let ((x 10) (y 5) do-something))

;; Написать про let*
```
