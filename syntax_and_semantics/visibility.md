# Visibilidade

Os métodos são públicos por padrão: o compilador sempre permitirá que você os
invoque. Já que o padrão é público, não existe uma palavra-chave `public`.

Os métodos podem ser marcados como `private` ou `protected`.

Um método `private` (privado) só pode ser invocado sem um receptor, ou seja,
sem algo antes do ponto:

```crystal
class Pessoa
  private def dizer(mensagem)
    puts mensagem
  end

  def dizer_ola
    dizer "olá"      # OK, não tem receptor
    self.dizer "olá" # Erro, self é um receptor

    outro = Pessoa.new "Outro"
    outro.dizer "olá" # Erro, outro é um receptor
  end
end
```

Perceba que os métodos `private` estão visíveis para suas subclasses:

```crystal
class Empregado < Pessoa
  def dizer_tchau
    dizer "tchau" # OK
  end
end
```

Um método `protected` (protegido) só pode ser invocado em instâncias do mesmo
tipo que o atual:

```crystal
class Pessoa
  protected def dizer(mensagem)
    puts mensagem
  end

  def dizer_ola
    dizer "olá"      # OK, o self implícito é uma Pessoa
    self.dizer "olá" # OK, self é uma Pessoa

    outro = Pessoa.new "Outro"
    outro.dizer "olá" # OK, outro é uma Pessoa
  end
end

class Animal
  def fazer_uma_pessoa_falar
    pessoa = Pessoa.new
    pessoa.dizer "olá" # Erro, pessoa é uma Pessoa,
                       # mas o tipo atual é um Animal
  end
end

mais_uma = Pessoa.new "Mais uma"
mais_uma.dizer "olá" # Erro, mais_uma é uma Pessoa
                     # mas o tipo atual é Program
```

Um método de classe protegido (`protected`) pode ser invocado de um método de
instância e vice versa:

```crystal
class Pessoa
  protected def self.dizer(mensagem)
    puts mensagem
  end

  def dizer_ola
    Pessoa.dizer "olá" # OK
  end
end
```

## Métodos privados top-level

Um método top-level privado (`private`) só é visível no arquivo atual.

```crystal
# No arquivo um.cr
private def cumprimentar
  puts "Olá"
end

cumprimentar #=> "Olá"

# No arquivo dois.cr
require "./um"

cumprimentar # undefined local variable or method 'cumprimentar'
```

Isso permite que você defina métodos auxiliares em um arquivo que só serão
conhecidos por ele próprio.
