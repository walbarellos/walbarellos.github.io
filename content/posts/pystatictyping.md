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
```
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
    ```python
# Isso vai gerar um erro em tempo de execução se descomentado
```
# resultado_errado_runtime = multiplicar("cinco", 2.0)
# print(resultado_errado_runtime)

Descomente a linha resultado_errado_runtime = multiplicar("cinco", 2.0) e execute o script usando python exercicio_modulo1.py. Você verá um erro TypeError. Comente-a novamente após o teste.

Verificação estática com mypy:

    Instale o mypy se ainda não tiver: pip install mypy

    Adicione o seguinte trecho com erro de tipo no seu script e deixe-o descomentado:
    ```python
# Exemplo de erro que mypy pegaria ANTES da execução
            outro_resultado_errado: float = multiplicar("texto", 10.0) # Mypy sinalizará um erro aqui

            Salve o arquivo e abra seu terminal na mesma pasta do arquivo.

            Execute o mypy no seu arquivo: mypy exercicio_modulo1.py

            Observe a saída do mypy. Ele deve indicar um erro na linha onde você passou uma string para a função multiplicar, mesmo sem executar o código. Isso demonstra a detecção precoce de erros!

            Comente a linha outro_resultado_errado após o teste para seguir para os próximos módulos sem erros.
```
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

## 3.3. Tipagem em Classes

Aplicar type hints em classes é crucial para projetos orientados a objetos.

    Atributos de instância e de classe:

```python
class Livro:
    titulo: str
    autor: str
    publicado: bool = False # Atributo de classe com default

    def __init__(self, titulo: str, autor: str) -> None:
        self.titulo = titulo
        self.autor = autor

    def marcar_como_publicado(self) -> None:
        self.publicado = True

livro1 = Livro("O Pequeno Príncipe", "Antoine de Saint-Exupéry")
print(f"Livro: {livro1.titulo}, Autor: {livro1.autor}, Publicado: {livro1.publicado}")
livro1.marcar_como_publicado()
print(f"Publicado após marcar: {livro1.publicado}")
```

Métodos estáticos e de classe:

```python
from typing import Type
class Calculadora:
    @staticmethod
    def dobro(x: int) -> int:
        """Retorna o dobro de um número inteiro."""
        return x * 2

    @classmethod
    def criar_calculadora_padrao(cls: Type['Calculadora']) -> 'Calculadora':
        """Cria uma instância da Calculadora."""
        return cls()

print(f"Dobro de 5: {Calculadora.dobro(5)}")
calc = Calculadora.criar_calculadora_padrao()
print(f"Instância de calculadora criada: {calc}")
```


Herança e polimorfismo com type hints:

```python
from typing import List
class Animal:
    def emitir_som(self) -> str:
        raise NotImplementedError("Este método deve ser sobrescrito pelas subclasses.")

class Cachorro(Animal):
    def emitir_som(self) -> str:
        return "Au au!"

class Gato(Animal):
    def emitir_som(self) -> str:
        return "Miau!"

def ouvir_sons(animais: List[Animal]) -> None:
    for animal in animais:
        print(f"Ouvindo: {animal.emitir_som()}")

meu_cachorro = Cachorro()
meu_gato = Gato()

animais_na_fazenda: List[Animal] = [meu_cachorro, meu_gato]
ouvir_sons(animais_na_fazenda)
```


Self (PEP 673) para retornos de métodos que retornam a própria instância (Python 3.11+).
```python
    from typing import Self

    class Builder:
        def __init__(self) -> None:
            self.data: List[str] = []

        def add_item(self, item: str) -> Self: # Retorna a própria instância
            self.data.append(item)
            return self

        def build(self) -> List[str]:
            return self.data
```

# A partir do Python 3.11
```python

    builder = Builder().add_item("primeiro").add_item("segundo").build()
    print(f"Itens construídos: {builder}")
```
## 3.4. Protocolos (Protocols - PEP 544)

Tipagem estrutural (duck typing) com type hints. Definindo interfaces implícitas.
```python
    Protocol e runtime_checkable.
    from typing import Protocol, runtime_checkable

    @runtime_checkable
    class Saudavel(Protocol):
        def saudar(self) -> str:
            ... # Este método deve ser implementado

    class Pessoa:
        def __init__(self, nome: str) -> None:
            self.nome = nome
        def saudar(self) -> str:
            return f"Olá, meu nome é {self.nome}."

    class Robo:
        def saudar(self) -> str:
            return "Saudações, unidade biológica."

    def cumprimentar(entidade: Saudavel) -> None:
        print(entidade.saudar())

    p = Pessoa("Maria")
    r = Robo()

    cumprimentar(p)
    cumprimentar(r)
```

# Verificando em tempo de execução se um objeto "implementa" o Protocolo
```python
    print(f"Pessoa é Saudavel? {isinstance(p, Saudavel)}")
    print(f"Robo é Saudavel? {isinstance(r, Saudavel)}")
```

## Exercícios do Módulo 3

    Criar type aliases para estruturas de dados complexas:
    Crie um arquivo exercicio_modulo3.py. Copie e cole o código abaixo nele:

# exercicio_modulo3.py

```python
from typing import Dict, List, TypedDict, Union
```

# Opção 1: Usando TypedDict (preferível para dicionários com chaves fixas)

```python
class Endereco(TypedDict):
    rua: str
    numero: int
    cidade: str
    cep: str
```
# Opção 2: Usando TypeAlias com Dict (para dicionários mais flexíveis ou compatibilidade anterior)
### EnderecoGenerico = Dict[str, Union[str, int]] # Mais genérico, menos específico

```python
class Pedido(TypedDict):
    id: int
    produto: str
    quantidade: int

ListaDePedidos = List[Pedido]

# Exemplo de uso
meu_endereco: Endereco = {
    "rua": "Rua das Flores",
    "numero": 123,
    "cidade": "São Paulo",
    "cep": "01000-000"
}

meus_pedidos: ListaDePedidos = [
    {"id": 1, "produto": "Teclado", "quantidade": 1},
    {"id": 2, "produto": "Mouse", "quantidade": 2}
]

print(f"Meu Endereço: {meu_endereco}")
print(f"Meus Pedidos: {meus_pedidos}")
```

Desenvolver classes com atributos e métodos tipados:
No mesmo script exercicio_modulo3.py, adicione o código abaixo:

# Adicione ao exercicio_modulo3.py

```python

class Carro:
    marca: str
    modelo: str
    ano: int
    quilometragem: float

    def __init__(self, marca: str, modelo: str, ano: int, quilometragem: float = 0.0) -> None:
        self.marca = marca
        self.modelo = modelo
        self.ano = ano
        self.quilometragem = quilometragem

    def dirigir(self, distancia: float) -> None:
        self.quilometragem += distancia
        print(f"Dirigiu {distancia} km. Quilometragem atual: {self.quilometragem} km.")

    def obter_informacoes(self) -> str:
        return (f"Marca: {self.marca}, Modelo: {self.modelo}, "
                f"Ano: {self.ano}, Quilometragem: {self.quilometragem:.2f} km")

meu_carro = Carro("Toyota", "Corolla", 2020, 15000.5)
print(meu_carro.obter_informacoes())
meu_carro.dirigir(250.3)
print(meu_carro.obter_informacoes())
```python

Implementar funções genéricas usando TypeVar:
No mesmo script exercicio_modulo3.py, adicione o código abaixo:

# Adicione ao exercicio_modulo3.py

```python
from typing import List, TypeVar

Item = TypeVar('Item')

def inverter_lista(lista: List[Item]) -> List[Item]:
    """Inverte a ordem dos elementos em uma lista, mantendo o tipo."""
    return lista[::-1]

lista_int = [1, 2, 3, 4, 5]
lista_str = ["a", "b", "c"]

lista_int_invertida = inverter_lista(lista_int)
lista_str_invertida = inverter_lista(lista_str)

print(f"Lista de inteiros original: {lista_int}, invertida: {lista_int_invertida}")
print(f"Lista de strings original: {lista_str}, invertida: {lista_str_invertida}")

Criar um Protocol e testar sua funcionalidade:
No mesmo script exercicio_modulo3.py, adicione o código abaixo:
```python
# Adicione ao exercicio_modulo3.py
    from typing import Protocol, runtime_checkable

    @runtime_checkable
    class Armazenavel(Protocol):
        id_item: str # Atributo que deve existir

        def salvar(self) -> None:
            ... # Método que deve ser implementado

    class Produto:
        def __init__(self, id_produto: str, nome: str) -> None:
            self.id_item = id_produto # Deve ter o mesmo nome do protocolo
            self.nome = nome

        def salvar(self) -> None:
            print(f"Produto '{self.nome}' com ID '{self.id_item}' salvo no banco de dados.")

    def processar_item(item: Armazenavel) -> None:
        print(f"Processando item com ID: {item.id_item}")
        item.salvar()

    meu_produto = Produto("PROD001", "Smartphone X")
```

# Verificando se Produto implementa Armazenavel
```python
    print(f"Meu produto é Armazenavel? {isinstance(meu_produto, Armazenavel)}")

    processar_item(meu_produto)
```

# Módulo 4: Ferramentas de Verificação de Tipo

## 4.1. Mypy

    A ferramenta de verificação de tipo estático mais popular para Python.

    Instalação e configuração básica:

    ```bash
pip install mypy
    ```
Executando mypy na linha de comando:

mypy seu_arquivo.py
mypy seu_modulo/

Ignorando erros específicos:
```python
# mypy: ignore-errors
```
# mypy: disable-error-code=attr-defined
minha_variavel_sem_tipo = 10 # type: ignore

Arquivo de configuração mypy.ini:
Permite configurar o comportamento do mypy para todo o projeto.
```toml
# mypy.ini
    [mypy]
    python_version = 3.9
    warn_return_any = True
    warn_unreachable = True
    no_implicit_optional = True
    check_untyped_defs = True
    disallow_untyped_defs = False

## 4.2. Integração com IDEs

    PyCharm, VS Code (com extensões como Pylance, Jedi): A maioria das IDEs modernas tem excelente suporte a type hints.

    Autocomplete e realce de erros em tempo real: As IDEs usam os type hints para oferecer sugestões mais inteligentes e para sublinhar erros de tipo enquanto você digita.

## 4.3. Outras Ferramentas (Breve Visão Geral)

    pyright (Microsoft): Outro verificador de tipo estático de alta performance, popular no ecossistema VS Code (usado pelo Pylance).

    pyre-check (Meta): Mais uma opção de verificador de tipo estático, conhecido por sua velocidade em grandes bases de código.

    Linters como flake8 com plugins de type checking: Podem ser configurados para trabalhar em conjunto com mypy ou outros verificadores.

## 4.4. Considerações sobre a Adoção

    Começando pequeno: Adicione type hints a novos códigos e a funções críticas que você já está refatorando. Não precisa tipar tudo de uma vez.

    Refatorando código existente: À medida que você trabalha em partes do código legado, aproveite para adicionar type hints.

    Equilíbrio entre cobertura de tipo e tempo de desenvolvimento: Encontre o balanço certo para o seu projeto. A tipagem estática é uma ferramenta, não um fim em si mesma.

## Exercícios do Módulo 4

    Configurar o mypy em um projeto:

        Crie uma nova pasta para este módulo (ex: modulo4_mypy).

        Dentro dela, crie um arquivo main.py. Copie e cole o código abaixo nele (com um erro intencional):
        ```python
# main.py
```
from typing import List

def saudar_todos(nomes: List[str]) -> None:
    for nome in nomes:
        print(f"Olá, {nome}!")

# Isso é um erro de tipo!
saudar_todos(["Alice", 123, "Bob"])

Crie um arquivo mypy.ini na mesma pasta com o seguinte conteúdo:
```toml
# mypy.ini
    [mypy]
    python_version = 3.9
    check_untyped_defs = True
    warn_return_any = True

    Abra o terminal nesta pasta e execute mypy main.py. Observe o erro reportado.

Corrigir erros de tipo detectados pelo mypy:

    No arquivo main.py do exercício anterior, corrija a linha que causa o erro de tipo para que mypy não aponte mais problemas.

    Execute mypy main.py novamente para confirmar que não há mais erros.

Copie e cole este código para corrigir o main.py:
```python
# main.py (corrigido)
    from typing import List

    def saudar_todos(nomes: List[str]) -> None:
        for nome in nomes:
            print(f"Olá, {nome}!")
```
# Correção: todos os elementos da lista devem ser strings
    saudar_todos(["Alice", "Maria", "Bob"])

    Experimentar a integração de type hints com sua IDE:

        Abra um dos arquivos de exercícios anteriores (ex: exercicio_modulo2.py) em sua IDE (PyCharm, VS Code, etc.).

        Tente introduzir um erro de tipo (por exemplo, passar um int para uma função que espera str).

        Observe como a IDE (via Pylance, Jedi, ou o próprio PyCharm) sublinha o erro em tempo real, sem que você precise rodar o mypy manualmente. Isso mostra o valor da integração.

        Corrija o erro após observar a detecção.

# Módulo 5: Melhores Práticas e Desafios

## 5.1. Dicas e Boas Práticas

    Ser explícito com os tipos: Quanto mais específico, melhor. Evite Any sempre que possível.

    Usar type aliases para complexidade: Simplifica a leitura de tipos aninhados ou muito longos.

    Documentar seus type hints: Explique a intenção por trás de tipos complexos ou escolhas específicas.

    Quando não usar type hints (ou ser mais flexível):

        Em scripts muito pequenos e descartáveis.

        Quando a inferência de tipo já é óbvia e adicionar hints tornaria o código mais verboso sem ganho.

        Em protótipos rápidos onde a velocidade de desenvolvimento é crítica.

## 5.2. Lidando com Bibliotecas sem Type Hints

Nem todas as bibliotecas de terceiros vêm com type hints nativos.

    Usando stub files (.pyi): São arquivos Python que contêm apenas as assinaturas de funções, classes e seus type hints, sem a implementação. mypy e outras ferramentas podem usá-los para verificar o código. Muitos pacotes populares fornecem stubs via types-xxxx (ex: pip install types-requests).

    Type[T] para classes: Já vimos seu uso para passar tipos como argumentos.

## 5.3. Desafios Comuns e Soluções

    Circular imports com type hints: Quando dois módulos se importam mutuamente e um dos imports é para um type hint.

        Solução: Use from __future__ import annotations (disponível a partir do Python 3.7) ou use strings para forward references ('MinhaClasse').
    ```python
# Exemplo de como lidar com imports circulares
```
# modulo_a.py
# from __future__ import annotations # Use isso no topo do arquivo se Python <= 3.9
from typing import TYPE_CHECKING

if TYPE_CHECKING: # Importa apenas para verificação de tipo, não em runtime
    from modulo_b import B

class A:
    def __init__(self, b_inst: 'B') -> None: # Usa string para forward reference
        self.b_inst = b_inst

# modulo_b.py
# from __future__ import annotations # Use isso no topo do arquivo se Python <= 3.9
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from modulo_a import A

class B:
    def __init__(self, a_inst: 'A') -> None:
        self.a_inst = a_inst

Tipos que dependem de si mesmos (Self-referential types): Estruturas de dados recursivas (ex: nós de árvore).

    Solução: Use strings para forward references.

```python
from typing import Optional
```
class Node:
    def __init__(self, value: int, next_node: Optional['Node'] = None) -> None:
        self.value = value
        self.next_node = next_node

Overloading de funções com typing.overload: Quando uma função tem diferentes assinaturas dependendo dos tipos de seus argumentos.
```python
    from typing import overload, Union

    @overload
    def processar_dado(data: str) -> str:
        ... # Apenas a assinatura, sem implementação

    @overload
    def processar_dado(data: int) -> int:
        ... # Apenas a assinatura

    def processar_dado(data: Union[str, int]) -> Union[str, int]:
        if isinstance(data, str):
            return data.upper()
        elif isinstance(data, int):
            return data * 2
        else:
            raise TypeError("Tipo de dado não suportado.")

    print(processar_dado("hello"))
    print(processar_dado(10))
```
# print(processar_dado(True)) # Mypy sinalizaria erro

## 5.4. Novidades e Tendências

    Python 3.9+ e os operadores | para Union e Optional: Uma sintaxe mais limpa e legível.

        list[str] em vez de List[str] (a partir de 3.9, mas o from __future__ import annotations é recomendado para isso funcionar com tipos embutidos).

        str | int em vez de Union[str, int].

        str | None em vez de Optional[str].

    Genéricos sintaxe simplificada (Python 3.12+): def func[T](arg: T) -> T:.

## Exercícios do Módulo 5

    Refatorar um trecho de código para usar as melhores práticas de tipagem:
    Crie um arquivo exercicio_modulo5.py e refatore o código abaixo adicionando type hints e, se aplicável, type aliases para melhorar a clareza.

    Código original (copie e cole no exercicio_modulo5.py):
    ```python
# exercicio_modulo5.py (Antes da refatoração)
```
def processar_config(config_data):
    if config_data.get('ativo'):
        print("Sistema ativo.")
    for item in config_data.get('lista', []):
        print(f"Item processado: {item}")
    return len(config_data.get('lista', []))

# Teste
minha_config = {'ativo': True, 'lista': ['A', 'B', 'C'], 'versao': 1.0}
resultado = processar_config(minha_config)
print(f"Total de itens na lista: {resultado}")

Sua tarefa é adicionar as dicas de tipo para config_data, para que mypy possa verificar corretamente. Pense em como criar um TypeAlias ou TypedDict para config_data.

Exemplo de como ficaria após a refatoração (NÃO COPIE AINDA, tente fazer o seu primeiro!):
```python
# exercicio_modulo5.py (Após refatoração - Exemplo)
```
from typing import List, TypedDict, Union

class AppConfig(TypedDict):
    ativo: bool
    lista: List[str]
    versao: float
# Você pode adicionar 'Optional' se uma chave pode não existir
# descricao: Optional[str]

def processar_config(config_data: AppConfig) -> int:
    if config_data.get('ativo'): # .get() ainda é válido, mas mypy conhece o tipo agora
        print("Sistema ativo.")
    for item in config_data['lista']: # Agora sabemos que 'lista' existe e é List[str]
        print(f"Item processado: {item}")
    return len(config_data['lista'])

# Teste
minha_config: AppConfig = {'ativo': True, 'lista': ['A', 'B', 'C'], 'versao': 1.0}
resultado = processar_config(minha_config)
print(f"Total de itens na lista: {resultado}")

# mypy agora poderá detectar se você tentar:
# minha_config_errada: AppConfig = {'ativo': 'sim', 'lista': [1, 2], 'versao': '1.0'}
# processar_config(minha_config_errada)

Criar um stub file para uma biblioteca de exemplo:
Imagine que você tem uma biblioteca simples sem type hints. Crie três arquivos em uma nova pasta:

    minha_lib.py:
    ```python
# minha_lib.py
```
def carregar_dados_externos(caminho):
# Simula o carregamento de dados de um arquivo
    if caminho == "valido":
        return {"chave": "valor", "numero": 123}
    return None

class Configurador:
    def __init__(self):
        self.settings = {}

    def set_setting(self, key, value):
        self.settings[key] = value

minha_lib.pyi (o stub file - crie este arquivo manualmente e copie o conteúdo):
```python
# minha_lib.pyi
```
from typing import Dict, Any, Optional

def carregar_dados_externos(caminho: str) -> Optional[Dict[str, Any]]: ...

class Configurador:
    settings: Dict[str, Any]
    def __init__(self) -> None: ...
    def set_setting(self, key: str, value: Any) -> None: ...

app.py:
```python
# app.py
        from minha_lib import carregar_dados_externos, Configurador
        from typing import Dict, Any, Optional

        dados: Optional[Dict[str, Any]] = carregar_dados_externos("valido")
        if dados:
            print(f"Dados carregados: {dados.get('chave')}")

        config_app = Configurador()
        config_app.set_setting("ambiente", "producao")
        print(f"Configurações: {config_app.settings}")
```
# mypy deve apontar um erro aqui se 'numero' não for uma chave válida para o tipo retornado
# print(dados.get('outra_chave', 0) + 5) # mypy deve sinalizar se o retorno é Optional

    Rode mypy app.py na pasta que contém os três arquivos para ver a verificação de tipo funcionando.

    Discutir em grupo casos de uso e desafios encontrados: (Este exercício é mais conceitual e para discussão)

        Em um ambiente de equipe ou com um colega, discuta:

            Quais são os maiores desafios ao adicionar tipagem estática a um projeto Python existente?

            Em quais cenários vocês acham que a tipagem estática traz mais valor?

            Vocês usariam Any? Se sim, em que situações?

            Como a tipagem estática pode impactar o processo de revisão de código em suas equipes?



## Materiais Adicionais

    Documentação Oficial Python: Módulo typing.

    PEP 484: Type Hints.

    Mypy Docs: Documentação oficial do Mypy.

    Real Python: Artigos sobre tipagem em Python.
    
