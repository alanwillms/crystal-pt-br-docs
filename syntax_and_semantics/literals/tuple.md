# Tuple

Um [Tuple](http://crystal-lang.org/api/Tuple.html) geralmente é criado com um literal de tuplas:

```crystal
tuple = {1, "hello", 'x'} # Tuple(Int32, String, Char)
tuple[0]                  #=> 1       (Int32)
tuple[1]                  #=> "hello" (String)
tuple[2]                  #=> 'x'     (Char)
```

Para criar uma tupla vazia, use [Tuple.new](http://crystal-lang.org/api/Tuple.html#new%28%2Aargs%29-class-method).
