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
```

Abaixo está o programa Go que parseia esse código-fonte, gera a AST e imprime as informações da função declarada:

```go
package main

import (
	"fmt"
	"go/parser"
	"go/token"
	"go/ast"
)

func main() {
	src := `
		package main
		import "fmt"
		func Hello(name string) {
			fmt.Println("Hello", name)
		}
	`

	// Cria um novo conjunto de tokens
	fset := token.NewFileSet()

	// Faz o parse do código
	node, err := parser.ParseFile(fset, "example.go", src, parser.AllErrors)
	if err != nil {
		panic(err)
	}

	// Percorre a árvore de sintaxe
	ast.Inspect(node, func(n ast.Node) bool {
		// Encontra declarações de função
		if fn, ok := n.(*ast.FuncDecl); ok {
			fmt.Println("Função encontrada:", fn.Name.Name)
			for _, param := range fn.Type.Params.List {
				for _, name := range param.Names {
					fmt.Printf("- Parâmetro: %s do tipo %T\n", name.Name, param.Type)
				}
			}
		}
		return true
	})
}
```

### 🧠 O que esse código faz?

1. Define um trecho de código Go como string.
2. Usa `parser.ParseFile` para gerar uma árvore sintática.
3. Percorre a árvore com `ast.Inspect`.
4. Encontra funções (`ast.FuncDecl`) e extrai os parâmetros e nomes.

---

## 💡 Dicas de uso prático

Esse conhecimento é a base para criar ferramentas como:

- **Linters personalizados** (ex: proibir funções globais).
- **Geradores de código** com base em padrões.
- **Analisadores semânticos** para refatoração automática.
- **Mini interpretadores ou DSLs** dentro de Go.

---

## 🚀 Quer avançar ainda mais?

Se quiser ir além, você pode:

- Modificar a AST e **reescrever o código-fonte**.
- Integrar com `go/types` para fazer análise de tipos.
- Criar plugins para o `go vet` ou para sua própria IDE.

---

## 🧭 Referências oficiais

- [Pacote `go/ast`](https://pkg.go.dev/go/ast)
- [Pacote `go/parser`](https://pkg.go.dev/go/parser)
- [Pacote `go/token`](https://pkg.go.dev/go/token)

---

## 🧵 Conclusão

Com `go/ast`, você não está só escrevendo Go — você está **escrevendo ferramentas que entendem Go**. Isso coloca você em um novo patamar como desenvolvedor: um que pode construir suas próprias ferramentas de análise, refatoração ou até compilação!

---

Quer ver mais posts como esse? Comenta aí ou me chama que posso explorar mais temas avançados como geração de código com `go/types`, concorrência low-level com `unsafe`, ou construção de plugins com `go plugin`!
