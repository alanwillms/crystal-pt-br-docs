# Splats e tuplas

Um método pode receber um número variável de argumentos usando um *splat* (`*`), que só pode aparecer uma única vez, em qualquer posição:

```crystal
def somar(*elementos)
  total = 0
  elementos.each do |valor|
    total += valor
  end
  total
end

somar 1, 2, 3      #=> 6
somar 1, 2, 3, 4.5 #=> 10.5
```

Os argumentos informados tornam-se em uma [Tupla](http://crystal-lang.org/api/Tuple.html) no corpo do método:

```crystal
# elementos é uma Tuple(Int32, Int32, Int32)
somar 1, 2, 3

# elementos é uma Tuple(Int32, Int32, Int32, Float64)
somar 1, 2, 3, 4.5
```
