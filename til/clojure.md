Start repl
```console
lein repl
```

Create new project
```console
lein new app <project-name>
```

Run project
```console
lein run
```

Build jar file
```console
lein uberjar
; and run it
java - jar target/uberjar/<name>.jar
```

Every valid expression is `form`. Clojure `evaluates` every form to produce a value. Sample forms:
```clojure
42
"random string"
["array" "of" "random" "strings"]
```

`operations` do thigs, they take the form
```clojure
(operator operand1 operand2 ... operandn)
```

`if`
```clojure
(if condition
    then
    else)
```

`do` - wrap up multiple forms and run each of them
```clojure
(do (form1) (form2) (formN))
```

`when` - combine `if` and `do`, but without `else` brunch
```clojure
(when true (form1) (form2) (formN))
```

`nil?` - check if value is `nil`
```clojure
(nil? nil)
```

`def` - bind value to a variable
```clojure
(def answer 42)
```

`let` - binds names to values
```clojure
(let [prime 7]
  prime)
(let [x 2 y 4]
  (+ x y))
(let [[val & other] [1 2 3 4]
  (other))  ; [2 3 4]
```

data-types
```clojure
;; Numbers
42
3.14
1/2

;; Strings
"Primo Victoria!"

;; Maps
{:first-name "Mono" :second-name "Inc"}
(hash-map :key1 1 :key2 2)
;; use `get` to get value
(get {a: 0 b: 1) a)
;; use `get-in` for nested maps
;; use `assoc` to insert key
(assoc {} :answer 42)

;; Keywords
:like-this
;; Use to get values
(:a {:a 1 :b 2})  ; 1

;; Vector
[1 2 3]
(get [1 2 3] 0)  ; 1
(vector "some" "crazy" "values")
(conj [1 2 3] 4)  ; [1 2 3 4]

;; Lists
'(1 2 3)
(nth '(:a :b :c) 2)
(list "some" "values" "of" "the" "list")
(conj '(1 2 3) 4)  ; (4 1 2 3)

;; Sets
#{"wow" 42 :iceberg}
(hash-set 1 2 1 2)
(conj #{1 2} 3)
(set [1 2 3 3 3 3])
(contains #{1 2} 2)  ; true
(:a #{:a :b})  ; :a
(:a #{:b :c})  ; nil
(get #{:a :b 3.14} 42)  ; nil
```

### Functions
`defn` - define function
```clojure
(defn
  "Docstring"
  function-name [arg1 arg2] (function body))
```

`doc` get docstring
(doc map)

`map`
```clojure
(map inc [1 2 3 4])
```

Arity overload
```clojure
(defn multi-arity
  ;; 2-arity
  ([first-arg second-arg]
    (do-something first-arg second-arg))
  ;; 1-arity
  ([first-arg]
    (multi-arity first-arg "default-second-arg")))
```

Rest parameters
```clojure
(defn do-something
  [first-arg second-arg & rest] (do something))
```

Destructuring
```clojure
(defn take-things
  [[first-item second-item & other-items]]
  (println first-item)
  (println second-item)
  (println other-items))
(take-things [1 2 3 4 5])  ; 1\n2\n3 4 5

(defn location
  [{lat :lat lon :lon}]
  (println (str "Lat is: " lat))
  (println (str "Lon is: " lon)))
```

Anonymous functions
```clojure
(fn [params]
  body)
((fn [x] (* 2 x)) 3)  ; 6

#(do-something %)
(#(* % 2) 3)  ; 6
(map #(str "Hi, " %) ["World", "Harry"])
(#(str %1 %2) "Super " "Hot")
```

Loop
```clojure
(loop [idx 0]
  (println (str "Idx " idx))
  (if (> idx 5)
      (println "Done!")
      (recur (inc idx))))
```

Regular Expression
```clojure
(re-find #"^sample" "sample test!")  ; sample
(clojure.string/replace "Nice cat!" #"cat" "dog")  ; Nice dog!
```

Reduce
```clojure
(reduce + [1 2 3 4])  ; 10
```

### seq abstraction

map
```
; one collection
(map inc [1 2 3])

; multiple collections
(map str ['a' 'b' 'c'] ['A' 'B' 'C'])

; use function as args
(map #(% [1 2 3 4]) (func1 func2 func3))

; use to get items from dict
(map :code [{:name "russia" :code "ru"} {:name "england" :code "uk"}])
```

### namespaces
`in-ns` - create namespace and switch to it
```clojure
(in-ns 'hello.world)
```

Access name from other namespace
```clojure
sample.namespace-name/name-i-was-looking-for
```

Import all names from other namespace with `refer`
```clojure
(in-ns 'my.data)  ; supports :only, :exclude, :rename
(def book "Zen and the Art of Motorcycle Maintenance")
(in-ns 'my.workspace)
(clojure.core/refer 'my.data)
book
```

Short namespace names with `alias`
```clojure
(in-ns 'my.data)
(def book "Zen")
(in-ns 'my.workspace)
(alias 'd 'my.data)
d/book
```
