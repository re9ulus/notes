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
