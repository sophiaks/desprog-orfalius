Algoritmo de Rabin-Karp
=============

Introdução
----------
O algoritmo de Rabin-Karp é um algoritmo de procura de padrões em strings. Ou seja, a ideia é que as entradas sejam o padrão que queremos encontrar e a string que queremos varrer, e a saída é o índice no qual esse padrão aparece na string.

```py
def RabinKarp(string, padrão):
    ...
    return index
```

??? Exercício

Bom, dito isso vamos ver se deu para entender. Dada uma string

![](mecs.png)

e um padrão 

![](cs.png)

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

E isso pode até parecer completamente OK e que não tem nada de errado. Mas agora imagine que nós quiséssemos achar a substring `md AAAB` na string `md AAAAAAB`. Olha como que isso seria:

;AAAAB

??? Exercício
Então com isso podemos chegar no pseudocódigo seguinte:
```
n = tamanho do padrão
m = tamanho do texto

para cada substring de n caracteres do texto
    para cada caractere no padrão
        compara com o caractere na substring
```
Agora tente implementar esse pseudocódigo!
::: Gabarito
```py
for i in len(m):
    pat = txt[i:i+n]
    for j in len(n):
        if pat[j] != txt[j]:
            return 0
return 1
``` 
:::
???

Complicado, o algoritmo iria fazer tantas comparações que ele iria demorar demais da conta, e isso com o fator de que ambas as strings são pequenas. Se o computador tivesse que fazer esse processo com uma string de dezenas de caracteres num texto com centenas, nem pense.

Vamos mudar o método então. Em vez de comparar caractere por caractere entre as strings, vamos comparar os valores hash(i) das strings.

Mas primeiro vamos só definir o que é o valor hash. Cada caractere tem um valor decimal pré-definido (mas não necessariamente universal, existem diversos modos de calcular o hash de uma string), ou seja, vamos resumir a string a um valor, e a soma dos valores de cada caractere da string é o valor hash da string. Por exemplo, podemos usar os valores definidos na tabela ASCII:

![](ASCII.png)

Para calcular o valor hash de `md COM`, por exemplo, calcularíamos o valor de cada caractere de acordo com os seus valores decimais na tabela ASCII e somaríamos:

C = 67

O = 79

M = 77



hash(COM) = 67 + 79 + 77 = 223

hash(COM) = 223

A ideia de comparar hashs é justamente comparar o valor do hash do nosso padrão, ou seja, a substring `md COM`, com o hash de cada substring no texto `md INSPERCOMP`, até acharmos o mesmo valor. 

;hash

??? Exercício
Mas espera, algo me parece estranho nisso aí. Você consegue ver qual é a falha desse sistema?

Dica: como são calculados os hashs das substrings?

::: Gabarito
Puts, mas nesse caso, o computador vai a cada substring fazer a soma, caractere por caractere, dos valores hashs de cada letra. Ficar fazendo essas equações matemáticas é igualmente trabalhoso a comparar cada caractere primitivamente.
:::
???

??? Exercício

Baseado na animação acima, pense em uma estratégia para que o algoritmo seja mais eficiente.

Dica: a palavra chave é redundância.

Dica 2: 

![](rolling.jpg)



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

Então você provavelmente percebeu que `md EVI` e `md IVE` têm o mesmo hash, quando calculamos só com a tabela ASCII.

Então vamos consertar isso. Pense nessa próxima implementação, com uma hash function diferente.
```md
hash("COM") =  [([([(67 × 256) % 101 + 79] % 101) × 256] % 101) + 77] % 101 = 4
```

Complicadinha, né? Não vamos entrar muito em detalhe, mas esse é um dos cálculos possíveis do hash que minimiza os erros!



Complexidade
---------------

A complexidade é um fator decisivo para a escolha do algoritmo *Rabin-Karp*. Como visto anteriormente, ele se difere de um algoritmo "bruto" justamente por usar técnicas de "*Rolling Hash*", evitando calcular o "Hash" para cada *string* analisada e logo não precisando comparar caractere por caractere (*char* por *char*).
??? Exercício
Vamos "descobrir" a complexidade, considerando os parâmetros do exemplo:
``` py
texto = "INSPERCOMP"
padrao = "COM"
M = len(texto)  #  M = 10
N = len(padrao) #  N = 3
``` 

Se baseie em M e N para calcular a complexidade nessa situação.

:::Dica
Pense que, no mínimo, precisamos percorrer o texto uma vez.
Utilize a animação para guiar seu raciocínio.

;primitivo

:::
::: Observação
Não é necessário chegar a um resultado exato, mas pelo menos a ordem de complexidade do algoritmo em seu melhor/médio caso.
:::

::: Gabarito
Nesse caso, o algoritmo irá iterar algumas *X* vezes até o *Hash* da substring se igualar ao do padrão, depois, será iterado *Y* vezes para fazer o "check" *char* a *char*, do primeiro até o primeiro *char* diferente, se não houver nenhum *char* destoante, ele realizará esse loop N vezes. Após essa checagem, ele deve continuar iterando *Z* vezes até a string principal acabar.
A complexidade é linear *$O(13)$*, *$O(N+M)$*, de ordem *$O(n)$*.
:::

Agora, pensando no pior caso, temos que lembrar que isso acontece quando o padrão se repete todas vezes durante o texto. Por exemplo:
``` py
texto = "AAAAAAAAAA"
padrao = "AAAA"
M = len(texto)  #  M = 10
N = len(padrao) #  N = 4
``` 
Esse é o pior caso, porém o cálculo de sua complexidade é bem mais trivial.
::: Gabarito
Uma vez que vão ter 4 iterações para todas *substrings* do texto, conclui-se que a complexidade é exponencial *$O(N*M)$*, de ordem *$O(NM)$*. 

::: Tabela de Complexidades
|                   | melhor caso | caso médio | pior caso  |
|===================|:-----------:|:----------:|:----------:|
   **Rabin-Karp**   |   *$O(n)$*  |  *$O(n)$*  |  *$O(n²)$* |
:::
???

Beleza, agora para a cereja em cima do bolo!

??? Exercício
O que acontece em termos de complexidade e acurácia do algoritmo quando confiamos no hash? Ou seja, não vamos nos importar se ele dá um resultado correto, e a função não precisa ser complicada.

::: Gabarito
Podemos dizer que a complexidade não será o pior caso, já que não temos que iterar novamente sobre a string, mas por outro lado a acurácia também diminui, já que não temos a garantia de rever os caracteres e confirmar se realmente são iguais.
:::
???

??? Exercício
O que acontece em termos de complexidade e acurácia do algoritmo quando não confiamos no hash e adicionamos iterações para que o algoritmo retorne o valor correto sempre?

::: Gabarito
Como temos que iterar mais vezes sobre a string, a complexidade aumenta e a acurária aumenta também!
:::
???

E é aí que aparece a mágica do algoritmo de Rabin-Karp!

Podemos enxergar o problema de **duas perspectivas**:
* O hash é simples e rápido, mas temos chance de erro.
* O hash pode ser extremamente complexo, mas temos pouquíssima chance de erro.

Essas duas abordagens são chamadas de algoritmo de Monte Carlo, e algoritmo de Las Vegas respectivamente.

Monte Carlo: <https://en.wikipedia.org/wiki/Monte_Carlo_algorithm>;

Las Vegas: <https://en.wikipedia.org/wiki/Las_Vegas_algorithm>

A escolha entre um tipo de algoritmo ou outro depende mesmo da aplicação. Se você prioriza mais tempo do que acurácia, o Monte Carlo parece melhor, mas se você prefere maior acurácia com a chance de ser mais lento, entra aí o Las Vegas.


Desafio
---------------
Nessa seção final, é o desafio... Vamos implementar o Algoritmo de Rabin-Karp! Mas não se preocupe, apenas vamos 
programar uma versão mais "easy".

Iniciando, é necessário criar uma função rabin_karp(), qual vai receber o texto em que vai ser procurado nosso padrão, o próprio padrão e nos retornar o índice em que essas colisões acontecem. Para seguir com as nossas instruções será necessário rodar esse código em **Python**, se quiser programar em outra linguagem não tem problema, apenas não garantimos o resultado final.



??? Desafio

***Parte 1***

A ***def*** deve seguir esse esqueleto:
```
def RabinKarp(pat, txt):
    M = len(pat)
    N = len(txt)
    p = 0    
    t = 0    
```
Onde "p" e "t" são variáveis que devem acumular o valor do hash do padrão a ser procurado e texto, respectivamente.
O primeiro exercício consta em fazer o cálculo de hash! Sugerimos que faça um loop e que use a função ***ord()*** para saber o hash de cada caractere.

::: Gabarito

O loop deve estar de acordo, não precisa ser igual, com o código abaixo:

``` py
for i in range(M):
    p += (ord(pat[i]))
    t += (ord(txt[i]))

``` 
:::

***Parte 2***

Agora vamos fazer o "coração do código", usar o hash calculado para comparar e caso achar, "printar" o índice deste achado!
Para isso, basta criar um loop que deve rodar para todos os caracteres do texto, logo que uma vez o primeiro hash calculado, agora é só adicionar e retirar o hash do caractere novo e anterior, respectivamente.
Nessa parte, foque em criar o loop e o mecanismo de identificação de padrões, isto é, quando houver uma colisão (hash do padrão for igual ao do texto).

::: Dica
    
Lembre-se que como o primeiro hash já foi calculado, não precisamos iterar as primeiras M (comprimento do padrão) vezes

::: Dica do range do for
faça um *for in range(***N-M+1***)*
***N-M*** porque não precisamos dessas primeiras ***M*** vezes e ***+ 1*** porque o código tem que ser capaz de pegar o hash do próximo caractere, caso não tenha esse ***+1*** não vamos conseguir calcular o último hash.
:::

:::

::: Gabarito parte 2
``` py
for i in range(N-M+1):

        if p==t:

            for j in range(M):
                if txt[i+j] != pat[j]:
                    break
                else: j+=1
 
            if j==M:
                print("Achei! Está no índice " + str(i))
```
:::

***Parte 3***

Se não achar nada, deve fazer o rolling hash do próximo e segue o jogo!
Para isso, no loop da parte 2, inclua o mecanismo que calcula o rolling hash, que resumidamente é subtrair o hash do caractere mais antigo e adicionar o hash do caractere mais novo!

::: Gabarito parte 3
``` py
if i < N-M:
   t = (t-ord(txt[i])) + ord(txt[i+M])
```
:::


::: Gabarito geral
``` py
# calculo do Hash
def RabinKarp(pat, txt):
    M = len(pat)
    N = len(txt)
    p = 0    
    t = 0    
 
    for i in range(M):
        p += (ord(pat[i]))
        t += (ord(txt[i]))

    for i in range(N-M+1):

        if p==t:

            for j in range(M):
                if txt[i+j] != pat[j]:
                    break
                else: j+=1
 
            if j==M:
                print("Achei! Está no índice " + str(i))
 
        if i < N-M:
            t = (t-ord(txt[i])) + ord(txt[i+M])
 
```
:::

???


