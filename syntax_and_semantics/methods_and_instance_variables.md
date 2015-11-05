# Métodos e variáveis de instância

Podemos simplificar nosso construtor usando uma sintaxe mais curta para atribuir o argumento de um método a uma variável de instância:

```crystal
class Pessoa
  def initialize(@nome)
    @idade = 0
  end
end
```

No momento não podemos fazer muita coisa com uma pessoa: podemos criá-la com um nome, e pedir por seu nome e sua idade (que sempre será zero). Então vamos adicionar um método que envelhece uma pessoa:

```crystal
class Pessoa
  def envelhecer
    @idade += 1
  end
end

joao = Pessoa.new "João"
pedro = Pessoa.new "Pedro"

joao.idade #=> 0

joao.envelhecer
joao.idade #=> 1

pedro.idade #=> 0
```

Os nomes de métodos começam com uma letra minúscula e, como convenção, só usam letras minúsculas, _underscores_ e números.

Além disso, podemos declarar `envelhecer` dentro da definição original da `Pessoa`, ou em uma definição separada: Crystal combina todas as definições em uma única classe. O código abaixo funciona normalmente:

```crystal
class Pessoa
  def initialize(@nome)
    @idade = 0
  end
end

class Pessoa
  def envelhecer
    @idade += 1
  end
end
```

## Sobrescrevendo métodos e `previous_def`

Se você sobrescreve um método, a última definição toma precedência.

```crystal
class Pessoa
  def envelhecer
    @idade += 1
  end
end

class Pessoa
  def envelhecer
    @idade += 2
  end
end

person = Pessoa.new "João"
person.envelhecer
person.idade #=> 2
```

Você pode invocar o método sobrescrito com `previous_def`:

```crystal
class Pessoa
  def envelhecer
    @idade += 1
  end
end

class Pessoa
  def envelhecer
    previous_def
    @idade += 2
  end
end

person = Pessoa.new "João"
person.envelhecer
person.idade #=> 3
```

Sem argumentos ou parênteses, `previous_def` recebe os mesmos argumentos que o método onde é invocado. Caso contrário, ele recebe os argumentos que você passa para ele.
