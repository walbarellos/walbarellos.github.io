---
title: "Tipagem Estática em Python"
date: 2025-04-18
tags: ["Programming", "Python", "static", "typing", "imperative", "course"]
description: "Mini curso completo sobre tipagem estática em Python: fundamentos, uso prático, ferramentas e boas práticas."
weight: 1
draft: false
---


# Tipagem Estática em Python 
A tipagem estática em Python é um recurso introduzido no Python 3.5 (com a PEP 484) que permite adicionar hints (dicas) de tipo ao seu código. Embora Python continue sendo uma linguagem de tipagem dinâmica em sua execução, essas dicas de tipo são usadas por ferramentas externas (como linters, IDEs e verificadores de tipo estáticos como mypy) para ajudar a identificar erros antes mesmo de o código ser executado, melhorar a legibilidade e facilitar a refatoração.

## Objetivo do Curso

Compreender os fundamentos da tipagem estática em Python, como utilizá-la de forma eficaz em projetos e as ferramentas disponíveis para auxiliar no processo.

## Público-Alvo

Desenvolvedores Python que desejam melhorar a qualidade do código, reduzir erros em tempo de execução e tornar seus projetos mais manuteníveis.

# Módulo 1: Introdução à Tipagem Estática em Python

## 1.1. Por que Tipagem Estática em Python?

Python é conhecido por sua tipagem dinâmica, o que oferece grande flexibilidade. No entanto, em projetos grandes e complexos, essa flexibilidade pode levar a:

Limitações em projetos grandes e complexos: Dificuldade em rastrear tipos de dados e potenciais erros em tempo de execução.

Detecção precoce de erros: Com a tipagem estática, muitos bugs são identificados antes mesmo do código ser executado, economizando tempo no desenvolvimento.

    Melhoria da legibilidade e documentação do código: Os type hints funcionam como uma forma de documentação explícita sobre os tipos esperados, tornando o código mais fácil de entender por outros desenvolvedores (e por você mesmo no futuro).

    Facilitação da refatoração: Saber os tipos de antemão torna a mudança de código mais segura, pois as ferramentas podem avisá-lo sobre quebras de tipo.

    Suporte a ferramentas de desenvolvimento (autocomplete, linting): IDEs podem oferecer sugestões mais precisas e identificar problemas de tipo em tempo real.

    Onde a tipagem estática se encaixa no ciclo de vida do desenvolvimento: Principalmente durante a codificação e revisão de código, complementando os testes unitários e de integração.

## 1.2. O que são Type Hints?

    Conceito e sintaxe básica: São anotações de tipo adicionadas a variáveis, parâmetros de função e retornos. Elas são apenas "dicas" para as ferramentas, não impondo verificações em tempo de execução.

    Importância do PEP 484: Este Python Enhancement Proposal padronizou a forma como a tipagem estática é implementada em Python.

    Como os type hints são ignorados em tempo de execução: Python continua sendo dinamicamente tipado. Isso significa que um programa com type hints não se comporta de forma diferente de um sem eles em termos de execução; as verificações são feitas por ferramentas externas.

## 1.3. Sintaxe Básica de Type Hints

A sintaxe é simples e intuitiva. Veja alguns exemplos:

```python
    Variáveis: nome: str = "João", idade: int = 30

    Parâmetros de função: def saudacao(nome: str) -> str:

    Retorno de função: def soma(a: int, b: int) -> int:

    Variáveis de instância em classes:
    ```python
    class Pessoa:
        nome: str
        idade: int

        def __init__(self, nome: str, idade: int) -> None:
            self.nome = nome
            self.idade = idade
```
## Exercícios do Módulo 1

    Adicionar type hints a funções e variáveis simples:
    Crie um novo arquivo Python chamado exercicio_modulo1.py. Copie e cole o código abaixo nele:
   
    ```python
# exercicio_modulo1.py

# 1. Variáveis com type hints

preco: float = 19.99
esta_ativo: bool = True

# 2. Função multiplicar com type hints

def multiplicar(num1: float, num2: float) -> float:
    return num1 * num2

# 3. Função formatar_mensagem com type hints

def formatar_mensagem(usuario: str, mensagem: str) -> str:
    return f"{usuario} disse: {mensagem}"

# Testes (opcional, mas bom para ver o funcionamento)

print(f"Preço: {preco}")
print(f"Ativo: {esta_ativo}")
print(f"Multiplicação: {multiplicar(5.5, 2.0)}")
print(f"Mensagem formatada: {formatar_mensagem('Alice', 'Olá, mundo!')}")

Entender a diferença entre tipagem dinâmica e estática em Python:
No mesmo arquivo exercicio_modulo1.py:

    Erro em tempo de execução (Tipagem Dinâmica): Adicione as seguintes linhas (comentadas) ao seu script.

    
# Isso vai gerar um erro em tempo de execução se descomentado

# resultado_errado_runtime = multiplicar("cinco", 2.0)
# print(resultado_errado_runtime)

Descomente a linha resultado_errado_runtime = multiplicar("cinco", 2.0) e execute o script usando python exercicio_modulo1.py. Você verá um erro TypeError. Comente-a novamente após o teste.

Verificação estática com mypy:

    Instale o mypy se ainda não tiver: pip install mypy

    Adicione o seguinte trecho com erro de tipo no seu script e deixe-o descomentado:

# Exemplo de erro que mypy pegaria ANTES da execução
            outro_resultado_errado: float = multiplicar("texto", 10.0) # Mypy sinalizará um erro aqui

            Salve o arquivo e abra seu terminal na mesma pasta do arquivo.

            Execute o mypy no seu arquivo: mypy exercicio_modulo1.py

            Observe a saída do mypy. Ele deve indicar um erro na linha onde você passou uma string para a função multiplicar, mesmo sem executar o código. Isso demonstra a detecção precoce de erros!

            Comente a linha outro_resultado_errado após o teste para seguir para os próximos módulos sem erros.

# Módulo 2: Tipos Comuns e Coleções

## 2.1. Tipos Primitivos

    str, int, float, bool, bytes: São os tipos de dados fundamentais em Python.

    None: Representa a ausência de valor.

    Optional[T]: Do módulo typing. Significa que um valor pode ser do tipo T OU None. É equivalente a Union[T, None].
    ```python
    from typing import Optional

    def obter_nome_meio(nome_completo: str) -> Optional[str]:
```
# Lógica para extrair nome do meio, pode não existir
        if " " in nome_completo:
            return "Silva" # Exemplo simples
        return None

    nome_meio_presente = obter_nome_meio("João Silva Santos")
    nome_meio_ausente = obter_nome_meio("Ana")

    print(f"Nome do meio presente: {nome_meio_presente}")
    print(f"Nome do meio ausente: {nome_meio_ausente}")

## 2.2. Coleções (do módulo typing)

Para tipar coleções, precisamos importá-las do módulo typing.

    List[T]: Uma lista onde todos os elementos são do tipo T.
    ```python
from typing import List
```
numeros: List[int] = [1, 2, 3]
nomes: List[str] = ["Ana", "Bruno"]

Tuple[T, ...] ou Tuple[T1, T2]:

    Tuple[T, ...]: Uma tupla de tamanho variável, onde todos os elementos são do tipo T.

    Tuple[T1, T2]: Uma tupla de tamanho fixo e tipos específicos.

```python
from typing import Tuple
```
coordenadas: Tuple[float, float] = (10.5, 20.3)
qualquer_tupla_de_int: Tuple[int, ...] = (1, 2, 3, 4)

Dict[K, V]: Um dicionário com chaves do tipo K e valores do tipo V.
```python
from typing import Dict
```
idades: Dict[str, int] = {"Alice": 25, "Bob": 30}
config: Dict[str, List[float]] = {"valores": [1.1, 2.2]}

Set[T]: Um conjunto de elementos do tipo T.
```python
    from typing import Set

    frutas: Set[str] = {"maçã", "banana"}

    FrozenSet[T]: Um conjunto imutável de elementos do tipo T.
```
## 2.3. Outros Tipos Úteis

    Union[T1, T2, ...]: Indica que uma variável pode ser de um tipo ou de outro. No Python 3.10+, o operador | é a sintaxe preferida (str | int).
    ```python
from typing import Union
```
def imprimir_id(identificador: Union[str, int]) -> None:
    print(f"ID: {identificador}")

# No Python 3.10+ você pode usar:
def imprimir_id_novo(identificador: str | int) -> None:
    print(f"ID (novo): {identificador}")

imprimir_id("abc123")
imprimir_id(456)
imprimir_id_novo("xyz789")
imprimir_id_novo(1011)

Any: Usado quando o tipo é desconhecido ou pode ser qualquer coisa. Use com moderação, pois anula os benefícios da tipagem estática.
```python
from typing import Any
```
dados_aleatorios: Any = "qualquer coisa"
dados_aleatorios = 123
dados_aleatorios = [1, 2, 3]

print(f"Dados aleatórios: {dados_aleatorios}")

Callable[[Arg1, Arg2, ...], ReturnType]: Para tipar funções que são passadas como argumentos.
```python
from typing import Callable
```
def aplicar_operacao(a: int, b: int, operacao: Callable[[int, int], int]) -> int:
    return operacao(a, b)

def subtrair(x: int, y: int) -> int:
    return x - y

resultado_subtracao = aplicar_operacao(10, 5, subtrair) # resultado é 5
print(f"Resultado da subtração: {resultado_subtracao}")

# Exemplo com lambda
resultado_soma_lambda = aplicar_operacao(20, 30, lambda x, y: x + y)
print(f"Resultado da soma (lambda): {resultado_soma_lambda}")

Literal[value1, value2, ...]: Permite especificar que uma variável só pode ter um conjunto fixo de valores.
```python
from typing import Literal
```
def processar_status(status: Literal["pendente", "concluido", "erro"]) -> None:
    print(f"Status processado: {status}")

processar_status("pendente")
# processar_status("invalido") # mypy sinalizaria um erro aqui

Type[T]: Usado quando você espera que um argumento seja um tipo (uma classe) em vez de uma instância.
```python
    from typing import Type

    class Animal:
        def __init__(self, nome: str = "Animal") -> None:
            self.nome = nome
        def falar(self) -> str:
            return f"{self.nome} faz um som."

    class Cachorro(Animal):
        def __init__(self, nome: str = "Cachorro") -> None:
            super().__init__(nome)
        def falar(self) -> str:
            return f"{self.nome} late: Au au!"

    def criar_instancia(classe_animal: Type[Animal]) -> Animal:
        return classe_animal()

    minha_cachorra = criar_instancia(Cachorro)
    print(minha_cachorra.falar())

    minha_animal_generico = criar_instancia(Animal)
    print(minha_animal_generico.falar())
```
## Exercícios do Módulo 2

    Usar type hints com diferentes tipos de coleções:
    Crie um novo arquivo chamado exercicio_modulo2.py. Copie e cole o código abaixo nele:
    ```python
# exercicio_modulo2.py
```
from typing import List, Dict, Tuple, Set

# Sua implementação aqui
lista_compras: List[str] = ["Arroz", "Feijão", "Macarrão"]
estoque_produtos: Dict[str, int] = {"Pão": 10, "Leite": 5, "Ovos": 12}
item_preco: Tuple[str, float] = ("Café", 8.50)
status_flags: Set[bool] = {True, False}

print(f"Lista de Compras: {lista_compras}")
print(f"Estoque de Produtos: {estoque_produtos}")
print(f"Item e Preço: {item_preco}")
print(f"Status Flags: {status_flags}")

Implementar funções que aceitem múltiplos tipos usando Union:
No mesmo script exercicio_modulo2.py, adicione o código abaixo:
```python
# Adicione ao exercicio_modulo2.py
```
from typing import Union

def imprimir_dado(dado: Union[str, int]) -> None:
    if isinstance(dado, str):
        print(f"Recebido string: {dado}")
    elif isinstance(dado, int):
        print(f"Recebido inteiro: {dado}")

# Para Python 3.10+
def imprimir_dado_novo(dado: str | int) -> None:
    if isinstance(dado, str):
        print(f"Recebido string (novo): {dado}")
    elif isinstance(dado, int):
        print(f"Recebido inteiro (novo): {dado}")

imprimir_dado("Hello World")
imprimir_dado(123)
imprimir_dado_novo("Python Rules")
imprimir_dado_novo(456789)

Criar exemplos onde Optional é essencial:
Ainda no mesmo script exercicio_modulo2.py, adicione o código abaixo:
```python
# Adicione ao exercicio_modulo2.py
    from typing import Optional

    def buscar_usuario(id: int) -> Optional[str]:
        if id % 2 == 0:
            return f"usuario_{id}"
        return None

    usuario1 = buscar_usuario(10) # ID par
    usuario2 = buscar_usuario(7)  # ID ímpar

    if usuario1:
        print(f"Usuário encontrado: {usuario1}")
    else:
        print("Usuário 1 não encontrado.")

    if usuario2:
        print(f"Usuário encontrado: {usuario2}")
    else:
        print("Usuário 2 não encontrado.")
```
# Módulo 3: Tipagem Avançada e Classes

## 3.1. Type Aliases

Criar alias para tipos complexos melhora a legibilidade do código, especialmente para estruturas de dados aninhadas.
```python
from typing import Dict, List, Union, TypedDict
```
# Definindo um alias para um dicionário de configuração
# Pode ser Dict[str, Union[str, int, bool, List[str]]] ou str | int | bool | List[str] no 3.10+
ConfigDict = Dict[str, Union[str, int, bool, List[str]]]

# Usando TypedDict para maior clareza em dicionários com chaves fixas
class PedidoDetalhe(TypedDict):
    id: int
    produto: str
    quantidade: int

ListaDePedidos = List[PedidoDetalhe]

def carregar_configuracao() -> ConfigDict:
    return {
        "nome_app": "MeuApp",
        "versao": 1,
        "debug": True,
        "recursos": ["login", "dashboard", "relatorios"]
    }

UserId = str # Um alias simples para um tipo primitivo
def obter_usuario_por_id(id: UserId) -> str:
    return f"Usuário com ID: {id}"

config = carregar_configuracao()
print(f"Configuração carregada: {config}")
print(obter_usuario_por_id("user123"))

meus_pedidos: ListaDePedidos = [
    {"id": 1, "produto": "Teclado", "quantidade": 1},
    {"id": 2, "produto": "Mouse", "quantidade": 2}
]
print(f"Meus pedidos: {meus_pedidos}")

## 3.2. Genéricos (Generics)

Permitem escrever funções e classes que operam sobre vários tipos, mantendo a segurança de tipo.

    Usando TypeVar: A maneira tradicional de criar genéricos.

```python
from typing import TypeVar, List
T = TypeVar('T') # 'T' pode ser qualquer tipo

def obter_primeiro_item(data: List[T]) -> T:
    """Retorna o primeiro item de uma lista, mantendo o tipo."""
    if not data:
        raise ValueError("A lista não pode estar vazia.")
    return data[0]

primeiro_int = obter_primeiro_item([1, 2, 3])
print(f"Primeiro int: {primeiro_int}") # tipo int

primeira_str = obter_primeiro_item(["a", "b", "c"])
print(f"Primeira str: {primeira_str}") # tipo str
```

Nova sintaxe de Genéricos (Python 3.12+): Mais concisa.

 A partir do Python 3.12

```python

# def obter_primeiro_item_novo[T](data: list[T]) -> T: # Note 'list' minúsculo
#     """Retorna o primeiro item de uma lista, mantendo o tipo (sintaxe 3.12+)."""
#     if not data:
#         raise ValueError("A lista não pode estar vazia.")
#     return data[0]

# if __name__ == '__main__':
#     # Apenas execute isso se estiver em Python 3.12+
#     # primeiro_int_novo = obter_primeiro_item_novo([10, 20, 30])
#     # print(f"Primeiro int (novo): {primeiro_int_novo}")
#     pass

```
