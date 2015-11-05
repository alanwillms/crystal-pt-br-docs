# Valores padrão e argumentos nomeados

Um método pode especificar valores padrão para os últimos argumentos:

```crystal
class Pessoa
  def envelhecer(anos = 1)
    @idade += anos
  end
end

joao = Pessoa.new "João"
joao.idade #=> 0

joao.envelhecer
joao.idade #=> 1

joao.envelhecer 2
joao.idade #=> 3
```

Para especificar os valores dos argumentos que tem um valor padrão, você também pode usar seus nomes na invocação:

```crystal
joao.envelhecer anos: 5
```

Quando o método possui vários argumentos com um valor padrão, a ordem dos nomes na invocação não importa, e alguns nomes podem ser omitidos:

```crystal
def algum_metodo(x, y = 1, z = 2, w = 3)
  # faz alguma coisa...
end

algum_metodo 10 # x = 10, y = 1, z = 2, w = 3
algum_metodo 10, z: 10 # x = 10, y = 1, z = 10, w = 3
algum_metodo 10, w: 1, y: 2, z: 3 # x = 10, y = 2, z = 3, w = 1
```

Perceba que no exemplo acima você não pode usar o nome do `x`, já que ele não possui um valor padrão.

Desta forma, argumentos com valor padrão e argumentos nomeados estão relacionados uns com os outros: quando você especifica argumentos com um valor padrão você também está permitindo que quem chama use seus nomes. Seja sábio e escolha bons nomes.
