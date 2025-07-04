
---
title: "🧬 Tipagem Estática em Python"
date: 2025-07-03
tags: ["Programming", "Python", "static", "typing", "imperative", "course"]
description: "Mini curso completo sobre tipagem estática em Python: fundamentos, uso prático, ferramentas e boas práticas."
weight: 1
draft: false
---

# Tipagem Estática em Python

A tipagem estática em Python é um recurso introduzido no Python 3.5 (com a PEP 484) que permite adicionar *hints* (dicas) de tipo ao seu código. Embora Python continue sendo uma linguagem de tipagem dinâmica em sua execução, essas dicas de tipo são usadas por ferramentas externas (como linters, IDEs e verificadores de tipo estáticos como `mypy`) para:

- Ajudar a identificar erros antes mesmo de o código ser executado.
- Melhorar a legibilidade.
- Facilitar a refatoração.

## Objetivo do Curso

Compreender os fundamentos da tipagem estática em Python, como utilizá-la de forma eficaz em projetos e as ferramentas disponíveis para auxiliar no processo.

## Público-Alvo

Desenvolvedores Python que desejam melhorar a qualidade do código, reduzir erros em tempo de execução e tornar seus projetos mais manuteníveis.

# Módulo 1: Introdução à Tipagem Estática em Python

## 1.1. Por que Tipagem Estática em Python?

Python é conhecido por sua tipagem dinâmica, o que oferece grande flexibilidade. No entanto, em projetos grandes e complexos, essa flexibilidade pode levar a:

- **Limitações em projetos grandes**: Dificuldade em rastrear tipos de dados e potenciais erros em tempo de execução.
- **Detecção precoce de erros**: Muitos bugs são identificados antes da execução do código.
- **Melhoria da legibilidade**: `type hints` funcionam como documentação explícita sobre os tipos esperados.
- **Facilitação da refatoração**: Saber os tipos permite mudanças mais seguras no código.
- **Suporte a ferramentas de desenvolvimento**: Melhor autocomplete, linting e análise estática.
- **Ciclo de vida do desenvolvimento**: A tipagem estática atua principalmente durante a codificação e revisão, complementando os testes.

## 1.2. O que são Type Hints?

- **Conceito e sintaxe básica**: Anotações de tipo para variáveis, parâmetros e retornos.
- **PEP 484**: Define o padrão para type hints em Python.
- **Ignorados em tempo de execução**: Python continua dinâmico — as verificações são feitas por ferramentas externas.

## 1.3. Sintaxe Básica de Type Hints

A sintaxe é simples e intuitiva. Veja alguns exemplos:

```python
# Variáveis
nome: str = "João"
idade: int = 30

# Parâmetros de função
def saudacao(nome: str) -> str:
    return f"Olá, {nome}"

# Retorno de função
def soma(a: int, b: int) -> int:
    return a + b
```

### Variáveis de instância em classes

```python
class Pessoa:
    nome: str
    idade: int

    def __init__(self, nome: str, idade: int) -> None:
        self.nome = nome
        self.idade = idade
```

## Exercícios do Módulo 1

Adicione type hints a funções e variáveis simples.

Crie um novo arquivo Python chamado `exercicio_modulo1.py`. Copie e cole o código abaixo:

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

# Testes (opcional)
print(f"Preço: {preco}")
print(f"Ativo: {esta_ativo}")
print(f"Multiplicação: {multiplicar(5.5, 2.0)}")
print(f"Mensagem formatada: {formatar_mensagem('Alice', 'Olá, mundo!')}")
```

## Materiais Adicionais

- [Documentação Oficial Python: Módulo `typing`](https://docs.python.org/3/library/typing.html)
- [PEP 484: Type Hints](https://peps.python.org/pep-0484/)
- [Mypy: Documentação Oficial](https://mypy.readthedocs.io/)
- [Real Python: Artigos sobre Type Hints](https://realpython.com/python-type-checking/)
