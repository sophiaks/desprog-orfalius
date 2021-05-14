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
  
Rolling Hash
---------------
* Explicação básica

Dar outro exemplo com AB e BA (strings com o mesmo hash nesse caso) e perguntar
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
