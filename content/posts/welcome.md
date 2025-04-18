---
title: "Explorando as Entranhas do Go: Manipulando Código com ASTs como um Compilador Ninja"
date: 2025-04-18
tags: ["Go", "Golang", "AST", "Compiladores", "Parser", "go/ast"]
description: "Aprenda como usar os pacotes internos da linguagem Go para analisar, interpretar e navegar por código-fonte Go usando árvores de sintaxe abstrata."
weight: 1
draft: false
---

Você sabia que dá pra analisar código Go em tempo real usando ferramentas da própria linguagem? Com os pacotes `go/ast`, `go/parser` e `go/token`, podemos **parsear código-fonte, navegar na AST e até transformar código** — tudo isso dentro de um programa Go!

Neste post, vamos explorar o núcleo da linguagem e construir um mini interpretador estático que **encontra funções e parâmetros declarados** em um código Go qualquer. Isso é o tipo de coisa que linters, IDEs e ferramentas como `go vet` fazem nos bastidores.

## 🔍 O que é uma AST?

A AST (*Abstract Syntax Tree*) é uma estrutura de dados que representa o código-fonte de maneira hierárquica e estruturada. Cada pedaço do código (função, variável, chamada de função) vira um nó dentro de uma árvore.

Com ela, podemos **inspecionar e manipular o código como dados**. É o coração de qualquer compilador ou analisador semântico.

---

## 🧪 Exemplo na prática: parseando código Go

Vamos analisar o seguinte código:

```go
package main

import "fmt"

func Hello(name string) {
	fmt.Println("Hello", name)
}
