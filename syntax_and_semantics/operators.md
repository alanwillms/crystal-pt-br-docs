# Operadores

Operadores como `+` e `-` são chamadas regulares de métodos. Por exemplo:

```crystal
a + b
```

é o mesmo que:

```crystal
a.+(b)
```

Você pode definir um operador para um tipo da seguinte maneira:

```crystal
struct Vetor2
  getter x, y

  def initialize(@x, @y)
  end

  def +(outro)
    Vetor2.new(x + outro.x, y + outro.y)
  end
end

v1 = Vetor2.new(1, 2)
v2 = Vetor2.new(3, 4)
v1 + v2               #=> Vetor2(@x=4, @y=6)
```

Veja a seguir uma lista completa de operadores com seu significado usual.

## Operadores unários

```crystal
+   # positivo
-   # negativo
!   # não
~   # complemento bit a bit
```

Estes são definidos sem argumentos. Por exemplo

```crystal
struct Vetor2
  def -
    Vetor2.new(-x, -y)
  end
end

v1 = Vetor2.new(1, 2)
-v1                   #=> Vetor2(@x=-1, @y=-2)
```

## Operadores binários

```crystal
+   # adição
-   # subtração
*   # multiplicação
/   # divisão
%   # módulo
!   # negação
&   # and bit a bit
|   # or bit a bit
^   # xor bit a bit
**  # exponenciação
<<  # deslocar para a esquerda, anexar
>>  # deslocar para a direita
==  # igual
!=  # desigual
<   # menor
<=  # menor ou igual
>   # maior
>=  # maior ou igual
<=> # comparação
=== # igualdade de case
```

## Indexação

```crystal
[]  # índice do array (lança exceção se estiver fora do limite)
[]? # índice do array (nil se estiver fora do limite)
[]= # atribuição ao índice do array
```

Por exemplo:

```crystal
class MeuArray
  def [](indice)
    # ...
  end

  def [](indice1, indice2, indice3)
    # ...
  end

  def []=(indice, valor)
    # ...
  end
end

array = MeuArray.new

array[1]       # invoca o primeiro método
array[1, 2, 3] # invoca o segundo método
array[1] = 2   # invoca o terceiro método

array.[](1)       # invoca o primeiro método
array.[](1, 2, 3) # invoca o segundo método
array.[]=(1, 2)   # invoca o terceiro método
```

## Significado

Pode-se atribuir qualquer significado aos operadores, mas a convenção é seguir
os significados acima para evitar código críptico ou código que se comporta de
maneira inesperada.
