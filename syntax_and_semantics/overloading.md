# Sobrecarga

Podemos definir um método `envelhecer` que aceita um número indicando os anos a acrescentar:

```crystal
class Pessoa
  def envelhecer
    @idade += 1
  end

  def envelhecer(anos)
    @idade += anos
  end
end

joao = Pessoa.new "João"
joao.idade #=> 0

joao.envelhecer
joao.idade #=> 1

joao.envelhecer 5
joao.idade #=> 6
```

Ou seja, podemos ter métodos diferentes com o mesmo nome e números diferentes de argumentos, e eles serão considerados como métodos separados. Isso é chamado de *sobrecarga de métodos*.

Métodos são sobrecarregados por diversos critérios:

* O número de argumentos
* As restrições de tipos aplicadas aos argumentos
* Se o método aceita ou não um [bloco](blocks_and_procs.md)

Por exemplo, podemos definir quatro métodos `envelhecer` diferentes:

```crystal
class Pessoa
  # Aumenta a idade em um
  def envelhecer
    @idade += 1
  end

  # Aumenta a idade pelo número de anos informado
  def envelhecer(anos : Int32)
    @idade += anos
  end

  # Aumenta a idade pelo número de anos informado como uma String
  def envelhecer(anos : String)
    @idade += anos.to_i
  end

  # Faz o yield da idade desta pessoa e aumenta sua idade pelo valor retornado
  # pelo bloco
  def envelhecer
    @idade += yield @idade
  end
end

pessoa = Pessoa.new "João"

pessoa.envelhecer
pessoa.idade #=> 1

pessoa.envelhecer 5
pessoa.idade #=> 6

pessoa.envelhecer "12"
pessoa.idade #=> 18

pessoa.envelhecer do |idade_atual|
  idade_atual < 20 ? 10 : 30
end
pessoa.idade #=> 28
```

Perceba que no caso do método com yield, o compilador descobriu que usa blocos por causa de uma expressão `yield`. Para tornar isso mais explícito, você pode adicionar um argumento opcional `&bloco` no final:

```crystal
class Pessoa
  def envelhecer(&bloco)
    @idade += yield @idade
  end
end
```

Na documentação gerada, o método com o `&bloco` sempre irá aparecer, independente de você escrevê-lo ou não.

Dado o mesmo número de argumentos, o compilador tentará organizá-los deixando os menos restritivos para o final:

```crystal
class Pessoa
  # Primeiro define este método
  def envelhecer(idade)
    @idade += idade
  end

  # Já que "String" é mais restritivo do que não ter nenhuma restrição, o
  # compilador coloca este método antes do anterior ao considerar qual
  # sobrecarga corresponde à chamada.
  def envelhecer(idade : String)
    @idade += idade.to_i
  end
end

pessoa = Pessoa.new "João"

# Invoca a primeira definição
pessoa.envelhecer 20

# Invoca a segunda definição
pessoa.envelhecer "12"
```

No entanto, nem sempre o compilador descrobrirá a ordem porque nem sempre há uma ordem, então é melhor sempre colocar os métodos menos restritivos no final.
