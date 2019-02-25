---
title: "Programação Funcional — Python #1"
published: true
---

# Programação Funcional — Python #1
Funções Puras


## Introdução
Antes de me aventurar por esse fantástico mundo funcional, a primeira coisa que me vinha à mente era usar somente recursão para resolver problemas, mas acabei aprendendo que não é apenas isso.

Programação Funcional ou FP(Functional Programming) é um paradigma de programação declarativo; paradigma é uma forma ou padrão de escrita de código e declarativo refere-se ao fato de usar expressões em vez de ditar um passo a passo.

Foi um dos primeiros paradigmas criados, mas devido ao alto custo de memória para a época se tornou pouco utilizado. Por conta do grande aumento da capacidade de hardware, hoje o paradigma funcional voltou em um crescente uso em produção.


## Efeitos colaterais
Um fato que tem que se ter em mente sobre FP é o objetivo de evitar ao máximo **efeitos colaterais**(_side effects_), exemplos práticos disso seria uma requisição ao **banco de dados**, uma requisição **HTTP** ou qualquer operação de **entrada/saída**. Em outras palavras, tenta-se diminuir o grau de imprevisibilidade do código. Isso nos leva ao princípio de **funções puras**.

## Funções Puras 
Funções puras retornam sempre o mesmo valor para os mesmos parâmetros e a sua execução não gera mudança no escopo fora dela. Esse comportamento é possível devido ao fato de que elas não são afetadas pelo estado externo nem interno, ou seja, tudo que a função precisa é passado para ela.

```python
def pure_increment(elements, index):
    new_elements = elements.copy()
    new_elements[index] += 1
    return new_elements


elements = [1, 2, 3]

pure_increment(elements, 0)
#Function return: [2, 2, 3]
pure_increment(elements, 0)
#Function return: [2, 2, 3]
pure_increment(elements, 0)
#Function return: [2, 2, 3]
```

No exemplo acima a função pure_increment irá incrementar o valor de um elemento em uma lista. E para torná-la pura passamos a lista e o índice da lista que será incrementado. Mas isso não é o suficiente. Vale lembrar de um fato importante: a lista no escopo da função ainda referencia a original, ou seja, qualquer mudança nela irá refletir na primeira, impossibilitando a pureza da função, recordando que uma função pura só pode operar com o que existe dentro do escopo da mesma. Por isso na linha 2 foi criado um cópia da lista usada, agora qualquer mudança nessa cópia não irá afetar a original. 
**Done!**

Vimos o que se fazer, mas é bom saber o que **não** fazer:

```python
def impure_increment(elements, index):
    elements[index] += 1
    return elements


elements = [1, 2, 3]

impure_increment(elements, 0)
#Function return: [2, 2, 3]
impure_increment(elements, 0)
#Function return: [3, 2, 3]
impure_increment(elements, 0)
#Function return: [4, 2, 3]
```

Acima não foi criada uma cópia da lista passada para a função, e como resultado temos um efeito colateral. Como a lista `elements` do escopo global é mudada dentro da função, a cada nova chamada da função passando `elements` retornará um resultado diferente. O mesmo efeito colateral poderia ocorrer com uso da palavra chave `global` para acessar diretamente `elements` no escopo global.

## Deixando seu código mais funcional 
Python tem um conjunto de funções [`built-in`](https://docs.python.org/3.5/library/functions.html) puras, como `map`, `filter`,e `sorted`, que não irão alterar a estrutura do objeto usado, elas também são **funções de alta ordem**, mas isso é assunto para outro momento. Essas funções podem muitas vezes substituir loops simples deixando o código com um estilo funcional, mas a tentativa de compactar em uma linha pode tornar o código de difícil compreensão.

```python
#Functional way
elements = {1: 'one', 2: 'two', 3: 'three', 4: 'four'}

dict(filter(lambda element: element[0] % 2 == 0, elements.items()))
#{2: 'two', 4: 'four'}

#Imperative way
for key in elements:
  if elements[key] % 2 != 0:
    del(elements[key])

#elements = {2: 'two', 4: 'four'}
```

No exemplo acima vemos uma forma de filtrar elementos pares do dicionário de uma forma simples e pura usando `filter` e um contra exemplo imperativo impuro.

Em aplicações práticas não é possível criar um código 100% puro, pois efeitos colaterais são necessários para permitir a comunicação com o ambiente externo. Então o máximo que pode ser feito é concentrar a impureza em trechos específicos da aplicação. Vale ressaltar o fato de que funções puras tornam o código bem mais fácil de testar e de fazer debug, pois os efeitos colaterais estarão em “locais” específicos do código.

---
**Be FUNctional!**

>_“Do or Do not. There is no try” — Yoda_



