# Variáveis de instância e inferência de tipo

Você percebeu que em todos os exemplos anteriores nós nunca dissemos os tipos do `@nome` e da `@idade` da `Pessoa`? Isso porque o compilador deduziu os tipos para nós.

Quando escrevemos:

```crystal
class Pessoa
  getter nome

  def initialize(@nome)
    @idade = 0
  end
end

joao = Pessoa.new "João"
joao.nome #=> "João"
joao.nome.size #=> 4
```

Já que invocamos `Pessoa.new` com um argumento do tipo `String`, o compilador também torna `@nome` em uma `String`.

Se tivéssemos invocado `Pessoa.new` com outro tipo, `@nome` teria recebido esse tipo diferente:

```crystal
um = Pessoa.new 1
um.nome #=> 1
um.nome + 2 #=> 3
```

Se você compilar os programas acima com o comando `tool hierarchy`, o compilador mostraria um grafo da hierarquia com os tipos que ele deduziu. No primeiro caso:

```
- class Object
  |
  +- class Reference
     |
     +- class Pessoa
            @nome : String
            @idade  : Int32
```

No segundo caso:

```
- class Object
  |
  +- class Reference
     |
     +- class Pessoa
            @nome : Int32
            @idade  : Int32
```

O que acontece se criarmos duas pessoas diferentes, uma com uma `String` e outra com um `Int32`? Vamos tentar fazer isso:

```crystal
joao = Pessoa.new "João"
um = Pessoa.new 1
```

Ao invocar o compilador com o comando `tool hierarchy`, nós obtemos:

```
- class Object
  |
  +- class Reference
     |
     +- class Pessoa
            @nome : (String | Int32)
            @idade  : Int32
```

Podemos ver que agora `@nome` tem um tipo `(String | Int32)`, que é lido como uma **união** de `String` e `Int32`. O compilador fez com que `@nome` tivesse todos os tipos atribuídos a ele.

Neste caso, o compilador considerará qualquer uso de `@nome` como sempre sendo ou uma `String` ou um `Int32`, e retornará um erro em tempo de compilação se um método não for encontrado em *ambos* os tipos:

```crystal
joao = Pessoa.new "João"
um = Pessoa.new 1

# Error: undefined method 'size' for Int32
joao.nome.size

# Error: no overload matches 'String#+' with types Int32
joao.nome + 3
```

O compilador também exibirá um erro se você usar uma variável primeiro assumindo um tipo e depois mudando esse tipo:

```crystal
joao = Pessoa.new "João"
joao.nome.size
um = Pessoa.new 1
```

Dá o erro em tempo de compilação:

```
Error in foo.cr:14: instantiating 'Pessoa:Class#new(Int32)'

um = Pessoa.new 1
             ^~~

instantiating 'Pessoa#initialize(Int32)'

in foo.cr:12: undefined method 'size' for Int32

joao.nome.size
          ^~~~~~
```

Ou seja, o compilador faz uma inferência global de tipos e te avisa quando quer que você cometa o erro no uso de uma classe ou método. Você pode optar por colocar uma restrição de tipo como `def initialize(@nome : String)`, mas isso torna o código um pouquinho mais verboso e também menos genérico: tudo funcionará normalmente se você criar uma instância de `Pessoa` com tipos que têm a mesma *interface* que uma `String`, desde que você use o nome da `Pessoa` como se fosse uma `String`.

Se você quiser ter tipos diferentes de `Pessoa`, um onde o `@nome` é um `Int32` e um onde o `@nome` é uma `String`, você precisa utilizar [programação genérica](generics.html).

## Variáveis de instância que podem ser Nil

Se uma variável de instância não for atribuída em todos os `initialize` definidos em uma classe, considera-se que ela também tem o tipo `Nil`:

```crystal
class Pessoa
  getter nome

  def initialize(@nome)
    @idade = 0
  end

  def endereco
    @endereco
  end

  def endereco=(@endereco)
  end
end

joao = Pessoa.new "João"
joao.endereco = "Argentina"
```

Agora o grafo da hierarquia nos mostra:

```
- class Object
  |
  +- class Reference
     |
     +- class Pessoa
            @nome : String
            @idade : Int32
            @endereco : String?
```

Você pode ver que `@endereco` é do tipo `String?`, que é uma notação curta para `String | Nil`. Isso significa que o código a seguir lança um erro em tempo de compilação:

```crystal
# Error: undefined method 'size' for Nil
joao.endereco.size
```

Para lidar `Nil`, e geralmente com tipos união, vocêm tem diversas opções: usar [if var](if_var.html), [if var.is_a?](if_varis_a.html), [case](case.html) e [is_a?](is_a.html).

## Inicialização geral

Variáveis de instância também podem ser inicializadas fora de métodos `initialize`:

```crystal
class Pessoa
  @idade = 0

  def initialize(@nome)
  end
end
```

Isso inicializará `@idade` com zero em todos os construtores. Isso é útil para evitar duplicação e para evitar o tipo `Nil` ao reabrir uma classe e adicionar variáveis de instância a ela.

## Especificando os tipos de variáveis de instância

Em determinados casos você pode querer dizer ao compilador para fixar o tipo de uma variável de instância. Você pode fazer isso com `::`:

```crystal
class Pessoa
  @idade :: Int32

  def initialize(@nome)
    @idade = 0
  end
end
```

Neste caso, se atribuirmos alguma coisa que não é um `Int32` a `@idade`, um erro de tempo de compilação será lançado no ponto da atribuição.

Perceba que você ainda precisa inicializar as variáveis de instância, seja com um inicializador geral ou dentro de um método `initialize`: não existem valores "padrão" para os tipos.
