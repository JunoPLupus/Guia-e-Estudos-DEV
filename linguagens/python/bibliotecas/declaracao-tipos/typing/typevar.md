---
title: TypeVar
aliases:
  - TypeVar
tipo: ferramenta
categoria:
  - declaracao-tipos
linguagem: python
versao_linguagem: 3.5+
tags:
  - ferramenta
status: pronto
atualizado: 2026/07/16 18:44
criado: 2026-07-15 14:34
links:
  - https://docs.python.org/pt-br/3.12/library/typing.html#typing.TypeVar
  - https://www.youtube.com/watch?v=nUOFiJ19rQk&list=PLbIBj8vQhvm04EuddtleOAoEmfU9vwQlN&index=8
---
# TypeVar

> Este recurso da biblioteca é compatível com versões _Python_ 3.5 ou superiores, mas nos exemplos vou utilizar a sintaxe nova definida pela PEP 695 (_Python_ >= 3.12).

## O que é

**Type variable (TypeVar)** é um parâmetro de tipo que atua como um símbolo para um tipo ainda não conhecido. Seu valor será substituído por um tipo concreto durante a verificação estática ou inferência de tipos.

## Pra que serve / quando usar

É muito útil quando você precisa criar [funções genéricas](https://docs.python.org/pt-br/3.12/reference/compound_stmts.html#generic-functions), [classes genéricas](https://docs.python.org/pt-br/3.12/reference/compound_stmts.html#generic-classes) ou [apelidos de tipo genérico](https://docs.python.org/pt-br/3.12/reference/compound_stmts.html#generic-type-aliases):

```python
class Sequence[T]:  # T é um TypeVar
    ...
```

## Uso básico

Podemos criar tipos variáveis delimitados e tipos variáveis restritos:

```python
class StrSequence[S: str]:  # S é a TypeVar com como limite superior em `str`;
    ...                     # podemos dizer que S é "delimitada por `str`"


class StrOrBytesSequence[A: (str, bytes)]:  # A é uma TypeVar restrita a str ou bytes
    ...
```

> ENTENDER MELHOR E MELHORAR ESSA EXPLICAÇÃO AQUI

O modo antigo (que ainda pode ser usado) era definido com o seguinte:

```python
from typing import TypeVar
```

---
### 1. TypeVar livre (_Unconstrained type variable_)

Pode ser substituído por qualquer tipo:

```python
_T = TypeVar('_T') # modo antigo

[T] # modo novo
```

---
### 2. TypeVar com _upper bound_ (_Bounded type variable_)

Neste exemplo, só pode ser substituído por `str` ou subtipos de `str`:

```python
_U = TypeVar('_U', bound=str) # modo antigo

[U: str] # modo novo
```

---
### 3. TypeVar covariante (_Covariant type variable_)

Usado em contextos de "produção" de valores (`output`), permite que subtipos sejam aceitos onde se espera o tipo base:

```python
_V_co = TypeVar('_V_co', covariant=True) # PEP 695: variância inferida no uso
```

Na nova sintaxe, a **TypeVar** é definida como **covariante** caso esteja sendo usada apenas para produzir valores, como neste exemplo retornando `T`:

```python
class A[T]:
	def pegar(self) -> T:... # só produz -> covariante
```

Para entender melhor a **covariante**, vamos usar o seguinte exemplo:

```python
class Animal: ...

class Felino(Animal): ...

class Gato(Felino): ...

class Abrigo[V]:
	def __init__(self, ocupante: V) -> None:
		...
	
	def pegar(self) -> V: # só produz V, covariante
		...
```

> [!TIP] O _typechecker_ ignora o `__init__`, então este **TypeVar** `[V]` ainda pode ser considerado **covariante**, já que o único método comum dele (`pegar() -> V`) apenas produz, logo, **covariante**.

Já que não especificamos na classe o "teto" no código acima, podemos criar funções que usam `Abrigo` com qualquer tipo igual ou abaixo de `object`, mas cada função define seu próprio teto. Vamos criar uma função que define a classe `Felino` como teto:

```python
def ler_animal(a: Abrigo[Felino]) -> None: ...

ler_animal(Abrigo[Felino](Felino())) # ok, Felino é o teto
ler_animal(Abrigo[Gato](Gato())) # ok, Gato é subtipo do teto (Felino)
ler_animal(Abrigo[Animal](Animal())) # erro: Animal está acima do teto (Felino)
```

>[!NOTE] Note que:
>Neste exemplo estamos criando as instâncias de `Abrigo` já dentro dos parâmetros apenas para encurtar o exemplo, mas poderíamos fazer algo tipo:
>```python
>felino = Abrigo[Felino](Felino()) # cria o objeto Abrigo (com um gato dentro)
>ler_animal(felino) # passa esse objeto pra função
>```

Poderíamos também ter definido o teto na classe de duas formas:

```python
class Abrigo[V: Felino]: ... # cria classe com teto fixo

class AbrigoDeGato(Abrigo[Gato]): ... # ou cria subclasse que possui um teto fixo
```

Assim qualquer função que use `Abrigo` como parâmetro, só poderia ter `Felino` ou subtipos dele, como por exemplo `Gato`:

```python
def funcao_certa(a: Abrigo[Gato])

def funcao_errada(b: Abrigo[Animal])
```

---
### 4. TypeVar Contravariante (_Contravariant type variable_)

Usado em contextos de "consumo" de valores (`input`), permite que supertipos sejam aceitos onde se espera o subtipo.

```python
_W_contra = TypeVar('_W_contra', contravariant=True) # PEP 695: variância inferida por tipo
```

A **Contravariante** é basicamente o oposto da [_covariante_](#3.%20TypeVar%20covariante%20(Covariant%20type%20variable)), ela define o "chão", então se por exemplo eu definir o chão como `Felino`, ele vai aceitar `Felino`(chão), `Animal` e `object` (supertipos), mas não vai aceitar `Gato` (subtipo). 

Para definir uma **TypeVar** como **contravariante**, todos os métodos dela (com exceção do `__init__`) devem APENAS consumir a **TypeVar** e não produzir/retornar ela. Um exemplo:

```python
class Abrigo[V: Felino]:
	def __init__(self, ocupante: V) -> None:
		...
	
	def pegar(self, V) -> None: # só consome V, contravariante
		...
```

Assim a **TypeVar** `[V]` é definida como **contravariante** e qualquer método que use `Abrigo` como parâmetro, só pode usar o "chão" (`Abrigo[Felino]`) e os supertipos dele (`Abrigo[Animal]`, `Abrigo[object]`).

---
### 5. União de tipos como bound (_Bounded type variable_)

Neste exemplo, aceita apenas `str` e/ou `bytes` (ou subtipos de cada um).

```python
_X = TypeVar('_X', bound=str | bytes) # modo antigo

[X: str | bytes] # modo novo
```

O _Bounded_ é mais flexível, ele define o "teto" (tipo `str | types`). Se você passar um `str` e um `bytes`, ele não precisa escolher uma única opção; ele sobre até o ancestral comum que ainda respeite o teto:

```python
def juntar[T: str | bytes](a: T, b: T) -> T:...

juntar("oi", b"tchau") # ok, T = str | bytes
```

---
### 6. TypeVar com restrições explícitas (_Constrained type variable_)

Neste exemplo, aceita apenas `int` ou `str` (tipos exatos ou subtipos deles).

```python
_Y = TypeVar('_Y', int, str) # modo antigo

[Y: (int, str)] # modo novo
```

O _Constrained_ é mais restritivo, o tipo que você usar, seja `int` ou `str` deve ser o mesmo tipo que você vai usar em todos os parâmetros `Y`. Vamos a um exemplo:

```python
def juntar[T: (str, bytes)](a: T, b: T) -> T:...

juntar("oi", "tchau")    # ok, T = str
juntar(b"oi", b"tchau")  # ok, T = bytes
juntar("oi", b"tchau")   # erro: não dá pra ser str e bytes ao mesmo tempo
```


---
### 7. TypeVar com tipo padrão (_Default Type_)

Se um tipo não for informado inicialmente, temos um tipo padrão `str` neste exemplo:

```python
_Z = TypeVar('_Z', default=str) # modo antigo

[Z = str] # modo novo
```

---
## Dicas

>[!DANGER] Nunca misture a versão nova da sintaxe com a versão antiga nos seus tipos. 
>Por exemplo:
>```python
>T = TypeVar('T')
>def funcao[U](x: T, y: U): ... # mistura proibida: antiga (T) e nova (U) na MESMA função
>```
>Você pode usar tanto a sintaxe antiga quanto a nova, contanto que elas não sejam misturadas na MESMA função ou classe.

## Notas sobre versões

A sintaxe antiga permite criar uma **TypeVar** de maneira mais verbosa, acoplada em uma classe, função ou método, OU reutilizável em alguma variável externa:

```python
T = TypeVar('T')
```

Já a a sintaxe nova apenas permite criar uma **TypeVar** de maneira mais simples, acoplada em uma classe, função ou método, nunca fora:

```python
class Abrigo[V: Felino]: ...

def ler_animal(Abrigo[Gato](Gato())): ...
```

Portanto você pode usar uma sintaxe ou outra no decorrer do seu projeto conforme sua necessidade, apenas **evite misturar as formas de declaração dentro de uma mesma classe, função ou método**.
