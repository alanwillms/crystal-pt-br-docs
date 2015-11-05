# O programa

O programa é um objeto global no qual você pode definir tipos, métodos e variáveis locais no escopo do arquivo.

```crystal
# Define um método no programa
def add(x, y)
  x + y
end

# Invoca o método add no programa
add(1, 2) #=> 3
```

O valor de um método é o valor de sua última expressão, não há necessidade de usar expressões `return` explicitamente. No entanto, elas são possíveis:

```crystal
def even?(num)
  if num % 2 == 0
    return true
  end

  return false
end
```

Ao invocar um método sem um receptor, como `add(1, 2)`, ele procurará no programa se não encontrá-lo no tipo atual ou em seus ancestrais.

```crystal
def add(x, y)
  x + y
end

class Foo
  def bar
    # invoca o método add do programa
    add(1, 2)

    # invoca o método baz de Foo
    baz(1, 2)
  end

  def baz(x, y)
    x * y
  end
end
```

Se você quiser invocar o método do programa, mesmo que o tipo atual defina um método com o mesmo nome, você pode prefixá-lo com `::`:

```crystal
def baz(x, y)
  x + y
end

class Foo
  def bar
    baz(4, 2) #=> 2
    ::baz(4, 2) #=> 6
  end

  def baz(x, y)
    x - y
  end
end
```

As variáveis declaradas em um programa não estão visíveis dentro de métodos:

```crystal
x = 1

def add(y)
  x + y # error: undefined local variable or method 'x'
end

add(2)
```

Os parênteses são opcionais nas invocações de métodos:

```crystal
add 1, 2 # é o mesmo que add(1, 2)
```
