# Tipos de retorno

O tipo do retorno de um método é sempre deduzido pelo compilador. No entanto, você pode querer especificá-lo por dois motivos:

1. Para assegurar que o método retorna o tipo que você quer
2. Para fazer com que o tipo apareça nos comentários da documentação

Por exemplo:

```crystal
def algum_metodo : String
  "olá"
end
```

O tipo de retorno segue a [gramática de tipos](type_grammar.md).
