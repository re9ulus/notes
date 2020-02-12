Start repl
```
lein repl
```

Create new project
```
lein new app <project-name>
```

Run project
```
lein run
```

Build jar file
```
lein uberjar
; and run it
java - jar target/uberjar/<name>.jar
```

Every valid expression is `form`. Clojure `evaluates` every form to produce a value. Sample forms:
```
42
"random string"
["array" "of" "random" "strings"]
```

`operations` do thigs, they take the form
```
(operator operand1 operand2 ... operandn)
```
