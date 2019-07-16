---
title: "Programação Funcional — Python #2 [PT-BR]"
published: true
---

## Introdução
Em Python, como outras linguagens multiparadigma (JavaScript, Ruby, etc), funções são também um tipo primitivo (`function`), permitindo serem usadas como argumento ou retorno de outras funções, as quais podemos chamar de **Funções de Alta Ordem** ou **HOF**s (_High Order Functions_).

## Generalização de funções
Outro bom exemplo de uso de HOFs é a generalização de funções. Usarei como exemplo o método `split`: 

```python
def split_by(character):
    return lambda content: content.split(character)

split_by_comma = split_by(', ')
split_by_comma('This, Is, A, Content, Example')
#result:
#['This', 'Is', 'A', 'Content', 'Example']
```

> Se você não está familiarizado com a palavra-chave `lambda`, ela é a forma como podemos declarar funções anônimas(funções simples que não possuem um nome) em Python.

Esse foi um exemplo com apenas uma sentença, mas imagine o poder de generalização que temos com a função split_by() , eu posso definir qualquer caractere que eu queira separar uma expressão e reusar essa lógica em qualquer parte do meu código ou em qualquer outra coleção de sentenças:

```python
>>> sentences = ['This, is a content example', 'This is, a content example', ...]
>>> list(map(split_by_comma, sentences))
# result:
# [['This', 'is a content example'], ['This is', 'a content example']]
```

Acima o código está expressando que vai ocorrer o mapeamento( map) de cada sentença em sentencespara um novo tipo de dado através da função split_by_comma .

Python possui um módulo chamado operator que evita a escrita de funções lambdas simples: chamar um método, acessar uma propriedade de todos os objetos dentro de uma estrutura(lista, dicionário, tupla, etc).

Podemos usar o a função methodcaller(),do módulo operator, em que eu passo o nome de outra função que quero chamar e os próximos parâmetros serão passados para a mesma.

```python
>>> from operator import methodcaller
>>> sentences = ['This, is a content example', 'This is, a content example', ...]
>>> map(methodcaller('split', ', '), sentences)
```

## Decorators
Python possui uma representação para funções de alta ordem, conhecida como **decorator**(`@`), e são amplamente usados em _frameworks_, como **Flask** e **Django**. Com decorators “capturamos” uma função e aplicamos alguma operação ao retorno da mesma:

```python
def split_by_comma(fn):
    def wrapper(sentence):
       return fn(sentence).split(',')
    return wrapper


@split_by_comma
def transform_content(content):
    return content.swapcase()

transform_content('some, Example')
#returns:
#['SOME', 'eXAMPLE']
```

Acima transform_content recebe uma string e retorna ela com o _case_ invertido(`swapcase`). Quando colocamos `@split_by_comma` na linha imediatamente acima da definição de `transform_content`, implicitamente, estamos capturando `transform_content`.

Na definição de `split_by_comma()` o parâmetro `fn` é a função que será capturada, definimos uma função (`wrapper`) com a mesma assinatura de `fn`, que irá ser chamada quando chamarmos `fn`, que nesse exemplo é `transform_content`. Quando chamamos `transform_content('some, content')` na verdade estamos fazendo:

```python
transform_content('some, content').split(',')
```

O uso do decorator é equivalente à:

```python
split_by_comma(transform_content)('some, Example')
```

O uso de decorators só é viável quando precisamos aplicar uma mesma lógica em vários outros métodos ou funções, aumentando o reuso e a manutenibilidade.

---
**Be FUNctional!**

>_“Do or Do not. There is no try” — Yoda_



