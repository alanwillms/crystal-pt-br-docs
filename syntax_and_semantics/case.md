# case

Um `case` é uma expressão de controle que permite realizar uma forma de _pattern matching_. Ele permite escrever uma corrente de if-else-if com uma pequena mudança na semântica e alguns construtores mais poderosos.

Em sua forma mai básica, ele permite comparar um valor com outros:

```crystal
case exp
when valor1, valor2
  faz_alguma_coisa
when valor3
  faz_outra_coisa
else
  faz_mais_uma_coisa
end

# O código acima é o mesmo que:
tmp = exp
if valor1 === tmp || valor2 === tmp
  faz_alguma_coisa
elsif valor3 === tmp
  faz_outra_coisa
else
  faz_mais_uma_coisa
end
```

Perceba que é usado o `===` para comparar uma expressão no valor do `case`.

Se a expressão do `when` for um tipo, é usado o `is_a?`. Além disso, se a expressão do `case` for uma variável ou a atribuição de um variável, o tipo dela será restringido:

```crystal
case var
when String
  # var :: String
  faz_alguma_coisa
when Int32
  # var :: Int32
  faz_outra_coisa
else
  # aqui "var" não é nem String e nem Int32
  faz_mais_uma_coisa
end

# O código acima é o mesmo que:
if var.is_a?(String)
  faz_alguma_coisa
elsif var.is_a?(Int32)
  faz_outra_coisa
else
  faz_mais_uma_coisa
end
```

Você pode invocar um método na expressão do `case` em um `when`, utilizando a sintaxe de objeto implícito:

```crystal
case num
when .even?
  faz_alguma_coisa
when .odd?
  faz_outra_coisa
end

# O código acima é o mesmo que:
tmp = num
if tmp.even?
  faz_alguma_coisa
elsif tmp.odd?
  faz_outra_coisa
end
```

Por fim, você pode omitir o valor do `case`:

```crystal
case
when cond1, cond2
  faz_alguma_coisa
when cond3
  faz_outra_coisa
end

# O código acima é o mesmo que:
if cond1 || cond2
  faz_alguma_coisa
elsif cond3
  faz_outra_coisa
end
```

Isso as vezes leva a um código que é mais natural de ler.
