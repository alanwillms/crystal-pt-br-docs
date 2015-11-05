# Restrições de tipo

Restrições de tipo são anotações de tipo colocadas nos argumentos de um método para restringir os tipos que são aceitos por ele.

```crystal
def adicionar(x : Number, y : Number)
  x + y
end

# Ok
adicionar 1, 2 # Ok

# Error: no overload matches 'adicionar' with types Bool, Bool
adicionar true, false
```

Perceba que se não tivessemos definido `adicionar` com restrições de tipo, também receberíamos um erro de compilação:

```crystal
def adicionar(x, y)
  x + y
end

adicionar true, false
```

O código acima dá o seguinte erro de compilação:

```
Error in foo.cr:6: instantiating 'adicionar(Bool, Bool)'

adicionar true, false
^~~

in foo.cr:2: undefined method '+' for Bool

  x + y
    ^
```

Isso ocorre porque quando você invoca `adicionar`, ele é instanciado com os tipos dos argumentos: toda invocação de método com uma combinação diferente de tipos resulta em uma instanciação diferente do método.

A única diferença é que a primeira mensagem de erro é um pouco mais clara, mas ambas as definições são seguras no sentido de que você terá um erro de um jeito ou de outro. Então, de modo geral, é preferível não especificar as restrições de tipo e praticamente só usá-las para definir sobrecarga de métodos. Isso resulta em código mais genérico e reutilizável. Por exemplo, se nós definirmos uma classe que tem um método `+`, mas não é um `Number`, podemos utilizar o método `adicionar` que não tem restrições de tipo, mas não aquele que tem restrições.

```crystal
# Uma classe que tem um método + mas não é um Number
class Seis
  def +(other)
    6 + other
  end
end

# O método adicionar sem restrições de tipo
def adicionar(x, y)
  x + y
end

# OK
adicionar Seis.new, 10

# O método adicionar com restrições de tipo
def adicionar_restrito(x : Number, y : Number)
  x + y
end

# Error: no overload matches 'adicionar_restrito' with types Seis, Int32
adicionar_restrito Seis.new, 10
```

Consulte a [gramática de tipos](type_grammar.md) para ver a notação usada nas restrições de tipo.

## Restrição a `self`

Um tipo de restrição especial é o `self`:

```crystal
class Pessoa
  def ==(outra : self)
    outra.nome == nome
  end

  def ==(outra)
    false
  end
end

joao = Pessoa.new "João"
outro_joao = Pessoa.new "João"
pedro = Pessoa.new "Pedro"

joao == outro_joao #=> true
joao == pedro #=> false (nomes diferem)
joao == 1 #=> false (porque 1 não é uma Pessoa)
```

No exemplo acima `self` é o mesmo que escrever `Pessoa`. Mas, em geral, `self` é o mesmo que escrever o tipo que finalmente terá o método, o que, quando envolve módulos, torna-se mais útil.

Além disso, uma vez que `Pessoa` herda de `Reference`, a segunda definição de `==` não é necessária, pois ela já é definida em `Reference`.

Perceba que `self` sempre representa o valor correspondente a um tipo de instância, mesmo em método de classe:

```crystal
class Pessoa
  def self.comparar(p1 : self, p2 : self)
    p1.nome == p2.nome
  end
end

joao = Pessoa.new "João"
pedro = Pessoa.new "Pedro"

Pessoa.comparar(joao, pedro) # OK
```

Você pode usar `self.class` para restringir ao tipo `Pessoa`. A próxima seção fala sobre o sufixo `.class` nas restrições de tipo.

## Classes e restrições

Usar, por exemplo, `Int32` como uma restrição de tipo faz com que o método só aceite instâncias de `Int32`:

```crystal
def foo(x : Int32)
end

foo 1     # OK
foo "olá" # Erro
```

Se você quer que um método só aceite o tipo `Int32` (e não intâncias dele), você pode usar `.class`:

```crystal
def foo(x : Int32.class)
end

foo Int32  # OK
foo String # Erro
```

Isso é útil para disponibilizar sobrecargas baseadas em tipos, não instâncias:

```crystal
def foo(x : Int32.class)
  puts "Recebi Int32"
end

def foo(x : String.class)
  puts "Recebi String"
end

foo Int32  # imprime "Recebi Int32"
foo String # imprime "Recebi String"
```

## Restrições de tipo em splats

Você pode especificar restrições de tipos em splats:

```crystal
def foo(*args : Int32)
end

def foo(*args : String)
end

foo 1, 2, 3       # OK, invoca a primeira sobrecarga
foo "a", "b", "c" # OK, invoca a segunda sobrecarga
foo 1, 2, "hello" # Erro
foo()             # Erro
```

Ao especificar um tipo, todos os elementos em uma tupla precisam ser daquele tipo. Além disso, uma tupla vazia não corresponde a nenhum dos casos acima. Se você quiser suportar uma tupla vazia, adicione outra sobrecarga:

```crystal
def foo
  # Este é o caso da tupla vazia
end
```

## Variáveis livres

Se você usar uma única letra maiúscula como uma restrição de tipo, o identificador se torna uma variável livre:

```crystal
def foo(x : T)
  T
end

foo(1)     #=> Int32
foo("olá") #=> String
```

Ou seja, `T` se torna o tipo que de fato foi usado para instanciar o método.

Uma variável livre pode ser usada para extrair o parâmetro de tipo de um tipo genérico em uma restrição de tipos:

```crystal
def foo(x : Array(T))
  T
end

foo([1, 2])   #=> Int32
foo([1, "a"]) #=> (Int32 | String)
```

Para criar um método que aceita um nome de tipo, ao invés de uma instância de um tipo, adicione `.class` à variável livre na restrição de tipo:

```crystal
def foo(x : T.class)
  Array(T)
end

foo(Int32)  #=> Array(Int32)
foo(String) #=> Array(String)
```

## Variáveis livres em construtores

Variáveis livres permitem que a indução de tipo seja usada ao criar tipos genéricos. Consulte a seção [Programação genérica](generics.md).
