Algoritmo de Rabin-Karp
=============

Introdução
----------
O algoritmo de Rabin-Karp é um algoritmo de procura em strings, que utiliza a técnica de _rolling hash_ para encontrar um padrão em uma string. Ou seja, a ideia é que as entradas sejam o padrão que queremos encontrar e a string que queremos varrer, e a saída é o índice no qual esse padrão aparece na string.

```py
def RabinKarp(string, padrão):
    ...
    return index
```

??? Exercício

Bom, dito isso vamos ver se deu para entender. Dada uma string
```
Mecsol
```
e um padrão 
```
cs
```
qual é a saída esperada do algoritmo?
::: Gabarito
A saída seria o índice no qual o padrão 'cs' aparece na string 'Mecsol', ou seja, 2.
:::
???

Não entendeu? Encontramos um vídeo de uma explicação nada intuitiva: <https://bit.ly/3uHwsdE>

  
Contextualização Básica
---------------

Vamos primeiro imaginar como nós faríamos a procura de uma string de um jeito mais primitivo usando o exemplo do `md INSPERCOMP`. Supondo que, dentro dessa string, nós quiséssemos achar a substring `md COM`, o jeito mais óbvio seria ir comparando caractere por caractere até acharmos a sequência inteira:

;primitivo

E isso pode até parecer completamente OK e que não tem nada de errado. Mas agora imagine que nós quiséssemos achar a substring `md AAAB` na string `md AAAAAAB`. Olha como isso ficaria:

;AAAAB

 Complicado, o algoritmo iria muitas comparações redunantes e repetidas, e isso não parece ser interessante. 

Vamos mudar o método então. Em vez de comparar caractere por caractere entre as strings, vamos comparar os valores hash(i) das strings.

Mas primeiro vamos só definir o que é o valor hash. Cada caractere tem um valor decimal pré-definido (mas não necessariamente universal, existem diversos modos de calcular o hash de uma string), ou seja, vamos resumir a string a um valor, e a soma dos valores de cada caractere da string é o valor hash da string. Por exemplo, podemos usar os valores definidos na tabela ASCII:

![](ASCII.png)

Para calcular o valor hash de `md COM`, por exemplo, calcularíamos o valor de cada caractere de acordo com os seus valores decimais na tabela ASCII e somaríamos:

C = 67

O = 79

M = 77

hash(COM) = 67 + 79 + 77 = 223

hash(COM) = 223

A ideia de comparar hashs é justamente comparar o valor do hash do nosso padrão, ou seja, a substring `md COM`, com o hash cada substring no texto `md INSPERCOMP`, até acharmos o mesmo valor.

Mas espera aí, ficar fazendo o cálculo do hash de cada substring caractere por caractere não é igualmente trabalhoso a comparar cada caractere primitivamente?

INSERIR ANIMATION

??? Exercício

Baseado na animação acima, pense em uma estratégia para que o algoritmo seja mais eficiente.

Dica: a palavra chave é redundância.

::: Gabarito
A ideia central é, ao invés de calcular e recalcular os hashs em todas as iterações, como estamos resumindo a string a um número, podemos apenas substrair o primeiro termo, e somar o último. Enfim, o rolling hash.
:::
???



Rolling Hash
---------------

E é aí que entra o rolling hash. A ideia do rolling hash é a seguinte: em vez de você calcular o valor hash de cada substring do texto caractere por caractere, você faz o primeiro cálculo do hash, e a medida que você vai passando pro lado, em vez de refazer o cálculo inteiro, você vai apenas subtrair o valor do caractere que saiu e adicionar o valor do caractere que entrou. Desse modo, você só faz 3 cálculos por tentativa! Você provavelmente não entendeu, mas vamos tentar explicar melhor isso através de uma animaçãozinha show:

;inspercomp

Através do algoritmo, a gente recebe que o padrão `md COM` pode ser encontrado no índice `md 6` do texto `md INSPERCOMP`.

??? Exercício
Vamos tentar com outro exemplo agora. Digamos que queremos saber em que índice da string `md REVIVER` podemos encontrar o padrão `md IVE`. Qual índice o rolling hash vai devolver?
Dica: calcule os hashs do começo dessa string.

::: Gabarito
Eita, parece que algo deu errado. Era pra ele ter devolvido o índice 3, mas ele acabou devolvendo 1. Porque isso aconteceu?
:::
???

Então você provavelmente percebeu que 'md EVI' e 'md IVE' têm o mesmo hash, quando calculamos só com a tabela ASCII.

Então vamos consertar isso. Pense nessa próxima implementação, com uma hash function diferente.
```md
hash("COM") =  [ ( [ ( [  (67 × 256) % 101 + 79 ] % 101 ) × 256 ] % 101 ) + 77 ] % 101 = 4
```

Complicadinha, né? Não vamos entrar muito em detalhe, mas esse é um dos cálculos possíveis do hash que minimiza os erros!

E é aí que aparece a mágica do algoritmo de Rabin-Karp!


Podemos enxergar o problema de **duas perspectivas**:
* O hash é simples e rápido, mas temos chance de erro.
* O hash pode ser extremamente complexo, mas não temos chance de erro.

Essas duas abordagens são chamadas de algoritmo de Monte Carlo, e algoritmo de Las Vegas respectivamente.

Monte Carlo: <https://en.wikipedia.org/wiki/Monte_Carlo_algorithm>;

Las Vegas: <https://en.wikipedia.org/wiki/Las_Vegas_algorithm>


??? Exercício
Agora para a complexidade: dê uma pensada, caso tenhamos que varrer a string uma vez apenas, e cada função hash tem complexidade O(1), qual a complexidade do algoritmo nesse caso?

Dica: utilize a animação para guiar seu raciocínio.

::: Gabarito
A complexidade vai ser O(N). Esse é o melhor caso!
:::
???

E o pior caso? O pior caso é 

??? Desafio
``` py
txt = "akjh bvhqregrqyuguiedgfvuyeqrgyfucgeyrgfyregfuygrf8geoygfoaeygfuqgf8ur34grfgeuabhfuGEFP8Haship9fehufhyreeqfygefyegfyewfijhes8gfhtrbgvreqfhubqeyfugeqyfgq8rgf8oqrfyuq7hg9q87uhvg78ugfv7uhgqu87gqyhg7rq0hyg7qyhqg0h70rqgvq0fhvgqfv0vg"
padrao = "Hashi"
for i in range(len(padrao)):
    p += (ord(padrao[i]))
    t += (ord(txt[i]))
``` 

::: Gabarito
!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!
:::

???



