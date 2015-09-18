# Variáveis Globais

Variáveis globais começam com um sinal de dólar (`$`). Elas são declaradas na primeira vez em que você atribui um valor a elas.

```crystal
$year = 2014
```

Seu tipo é o tipo combinado de todas as expressões que foram atribuídas a elas. Além disso, se o seu programa lê uma variável global antes que ela receba qualquer valor, ela também terá o tipo `Nil`.
