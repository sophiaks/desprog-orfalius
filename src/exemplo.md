Algoritmo de Rabin-Karp
=============

Introdução
----------
* O que é o algoritmo
* Como ele funciona em linhas gerais
* Exemplo e Pergunta
* Encontramos um vídeo de uma explicação nada intuitiva: <https://bit.ly/3uHwsdE>


??? Exercício

Vamos ver se você entendeu: se nossa string de entrada é `INSPERCOMP`, e nosso padrão é COM, qual índice nosso algoritmo devolve?

::: Gabarito
ERROU BABACA
:::

???
  
Contextualização Básica
---------------

Vamos primeiro imaginar como nós faríamos a procura de uma string de um jeito mais primitivo usando o exemplo do `INSPERCOMP`. Supondo que, dentro dessa string, nós quiséssemos achar a substring `COM`, o jeito mais óbvio seria ir comparando caractere por caractere até acharmos a sequência inteira:

;primitivo

E isso pode até parecer completamente OK e que não tem nada de errado. Mas agora imagine que nós quiséssemos achar a substring `AAAAAAB` na string `AAAAAAAAAAAAAAAAAAAAAAAB`. Complicado, o algoritmo iria fazer tantas comparações que ou ele iria demorar demais ou ia crashar, e nenhum desses casos parece ser interessante. 

Vamos mudar o método então. Em vez de comparar caractere por caractere entre as strings, vamos comparar os valores hash(i) das strings.

Mas primeiro, pros que não sabem, vamos só definir o que é o valor hash. Cada caractere tem um valor decimal pré-definido (mas não necessariamente universal, existem diversos modos de calcular o hash de uma string), e a soma dos valores de cada caractere da string é o valor hash da string. Por exemplo, podemos usar os valores definidos na tabela ASCII:

![](ASCII.png)

Para calcular o valor hash de `COM`, por exemplo, calcularíamos o valor de cada caractere de acordo com os seus valores decimais na tabela ASCII e somaríamos:

C = 67

O = 79

M = 77

hash(COM) = 67 + 79 + 77 = 223

hash(COM) = 223

A ideia de comparar hashs é justamente comparar o valor do hash do nosso padrão, ou seja, a substring `COM`, com o hash cada substring no texto `INSPERCOMP`, até acharmos o mesmo valor. 

(Hashi n fica puto não eu vou fazer uma animação pra isso mas não deu tempo ainda)

Mas espera aí, ficar fazendo o cálculo do hash de cada substring caractere por caractere não é igualmente trabalhoso a comparar cada caractere primitivamente? Sim!

Rolling Hash
---------------

E é aí que entra o rolling hash. A ideia do rolling hash é a seguinte: em vez de você calcular o valor hash de cada substring do texto caractere por caractere, você faz o primeiro cálculo do hash, e a medida que você vai passando pro lado, em vez de refazer o cálculo inteiro, você vai apenas subtrair o valor do caractere que saiu e adicionar o valor do caractere que entrou. Desse modo, você só faz 3 cálculos por tentativa! Você provavelmente não entendeu, mas vamos tentar explicar melhor isso através de uma animaçãozinha show:

;inspercomp

Através do algoritmo, a gente recebe que o padrão `COM` pode ser encontrado no índice `6` do texto `INSPERCOMP`.

enfia um exercício normal aqui pra eles verem se entenderam

Vamos tentar com outro exemplo agora. Digamos que queremos saber em que índice da string `REVIVER` podemos encontrar o padrão `IVE`. Qual índice o rolling hash vai devolver?

Eita muleque, algo deu errado. Era pra ele ter devolvido o índice `3`, mas ele acabou devolvendo `1`.

??? Exercício
O que tem de errado com essa implementação? - INTRODUZIR Las Vegas vs. Monte Carlo

::: Gabarito
Strings diferentes - mesmo hash
:::

???

FORMULA DO HASH LOUQUINHO

??? Exercício
``` py
# calculo do Hash
for i in range(len(padrao)):
    p += (ord(padrao[i]))
    t += (ord(txt[i]))
``` 
Assustador o bichin né? Qual deles deveríamos usar?

::: Gabarito
EXPLICAR Las Vegas vs. Monte Carlo
:::

???


* Tabela ASCII
* Las Vegas vs. Monte Carlo (comparar os dois)

|   CHAR   |   ASCII  |
|----------|----------|
| A        | 231      |

Funcionamento
--------------
- Animaçãozinha

;bubble

Complexidade
--------------
Nos casos em que o padrão não se repete em todas as partes
* Dicas - pensando que cada comparação é O(1), com N caracteres temos qual complexidade papapa

Desafio
--------------
* IMPLEMENTA (vamos dar um pouquinho do código) - PYTHON pfpfpf



Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo

!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!
