# new, initialize e allocate

Você pode criar uma instância de uma classe invocando `new` nela:

```
pessoa = Pessoa.new
```

Aqui `pessoa` é uma instância de `Pessoa`.

Nós não podemos fazer muita coisa com `pessoa`, então vamos adicionar alguns conceitos a ela. Uma `Pessoa` tem um nome e uma idade. Na seção "Tudo é um objeto", nós dissemos que um objeto tem um tipo e responde a alguns métodos, que são a única maneira de interagir com objetos, então vamos precisar dos métodos `nome` e `idade`. Vamos armazenar essas informações em variáveis de instância, que são prefixadas com um caractere de *arroba* (`@`). Nós também queremos que uma Pessoa venha a existir com um nome de nossa escolha e uma idade de zero. Nós programamos a parte de "vir a existir" com um método especial chamado `initialize`, que normalmente é conhecido como *construtor*:

```crystal
class Pessoa
  def initialize(nome)
    @nome = nome
    @idade = 0
  end

  def nome
    @nome
  end

  def idade
    @idade
  end
end
```

Agora podemos criar pessoas da seguinte forma:

```crystal
joao = Pessoa.new "João"
pedro = Pessoa.new "Pedro"

joao.nome #=> "João"
joao.idade #=> 0

pedro.nome #=> "Pedro"
```

Perceba que criamos uma `Pessoa` com `new`, mas nós definimos a inicialização em um método chamado `initialize`, não em um método `new`. Por que isso?

A reposta é que quando definimos um método `initialize`, o Crystal definiu um método `new` para nós, da seguinte forma:

```crystal
class Pessoa
  def self.new(nome)
    instancia = Pessoa.allocate
    instancia.initialize(nome)
    instancia
  end
 end
```

Primeiramente, perceba a notação `self.new`. Isso significa que o método pertence à **classe** `Pessoa`, e não qualquer instância em particular dessa classe. É por isso que conseguimos chamar `Pessoa.new`.

Em segundo lugar, `allocate` é um método de classe de baixo nível que cria um objeto não-inicializado com o tipo informado. Ele basicamente aloca a memória necessária para isso. Então o método `initialize` é invocado nele e então você obtém uma instância. Geralmente você nunca invoca o método `allocate`, já que ele é [inseguro](unsafe.md), mas esse é o motivo pelo qual `new` e `initialize` estão relacionados.

