
---
title: Programação Assíncrona e Metaprogramação em Python vs Ruby
date: 2025-04-18
description: Comparativo prático entre recursos de programação assíncrona e metaprogramação em Python e Ruby, com exemplos reais.
tags:
  - Python
  - Ruby
  - Asyncio
  - Fibers
  - Metaprogramação
  - Desenvolvimento
  - Programação Avançada
---

# Python vs Ruby — Assíncrono e Metaprogramação

Este documento compara as abordagens de **Python** e **Ruby** para dois temas poderosos: **programação assíncrona** e **metaprogramação**.

---

## ⚙️ Programação Assíncrona

### Python com `asyncio`

```python
import asyncio

async def tarefa():
    print("Início")
    await asyncio.sleep(1)
    print("Fim")

asyncio.run(tarefa())
```

- Nativo desde o Python 3.5 (`async`, `await`)
- Cancelamento com `.cancel()`
- Timeout com `asyncio.wait_for()`
- Execução paralela com `asyncio.gather()`
- Suporte a `async generators`

### Ruby com `async` (gem)

```ruby
require 'async'

Async do
  task1 = Async { sleep 2; puts "Task 1 done" }
  task2 = Async { sleep 1; puts "Task 2 done" }
end
```

- Utiliza Fibers (não threads)
- Controle via escopos de tarefa
- Cancelamento com `task.stop`
- Timeout com `Async::Timeout`
- Leve, elegante, mas exige gems externas

### Cancelamento de Tarefa em Ruby

```ruby
require 'async'

Async do |task|
  child = task.async { sleep 5; puts "não verá isso" }
  sleep 1
  child.stop  # Cancela
end
```

### Timeout em Ruby

```ruby
require 'async'
require 'async/timeout'

Async do
  Async::Timeout.timeout(2) do
    sleep 5
  end
rescue Async::TimeoutError
  puts "Tempo excedido!"
end
```

---

## 🧠 Metaprogramação

### Python

```python
def criar_classe():
    return type('NovaClasse', (), {'diga_oi': lambda self: print("Oi")})

cls = criar_classe()
obj = cls()
obj.diga_oi()
```

- Criação de classes com `type()`
- Metaclasses via `__new__`
- Descriptors com `__get__`, `__set__`
- Manipulação de AST (`ast`, `inspect`)
- Monkey patching e introspecção

### Ruby

```ruby
class Pessoa
  define_method(:diga_oi) { puts "Oi!" }
end

Pessoa.new.diga_oi
```

- Tudo é objeto (incluindo classes e métodos)
- Métodos dinâmicos com `define_method`
- `method_missing` para interceptação
- Avaliação dinâmica com `eval`, `instance_eval`
- `Module#prepend`, `alias_method` para alteração de comportamento

### Ruby: `method_missing`

```ruby
class Logger
  def method_missing(metodo, *args)
    puts "Chamou #{metodo} com #{args.inspect}"
  end
end

Logger.new.salvar("teste")
```

### Ruby: introspecção e reflexão

```ruby
class Produto
  attr_accessor :nome
end

p = Produto.new
p.methods.include?(:nome)  # true
```

---

## 🕐 Cancelamento de Tarefas

Permite interromper uma tarefa que não precisa mais ser executada. Útil em sistemas reativos ou que precisam otimizar recursos.

```python
import asyncio

async def tarefa_longa():
    print("A iniciar tarefa longa")
    try:
        await asyncio.sleep(10)
    except asyncio.CancelledError:
        print("Tarefa longa cancelada")
        raise
    print("Tarefa longa concluída")
    return "Resultado da tarefa longa"

async def main():
    tarefa = asyncio.create_task(tarefa_longa())
    await asyncio.sleep(2)
    print("A cancelar tarefa")
    tarefa.cancel()
    try:
        await tarefa
    except asyncio.CancelledError:
        print("Tarefa principal: tarefa foi cancelada")

if __name__ == "__main__":
    asyncio.run(main())
```

**Explicação**:
- `create_task()` agenda uma coroutine para rodar em segundo plano.
- `cancel()` emite um sinal de cancelamento.
- A exceção `CancelledError` permite capturar esse cancelamento e reagir apropriadamente.

---

## ⏱️ Timeouts com `asyncio.wait_for`

Controla o tempo de espera de uma coroutine. Se ela demorar demais, é cancelada automaticamente.

```python
async def tarefa_demorada():
    await asyncio.sleep(5)
    return "Resultado"

async def main():
    try:
        resultado = await asyncio.wait_for(tarefa_demorada(), timeout=2)
        print(f"Resultado: {resultado}")
    except asyncio.TimeoutError:
        print("Tempo de espera excedido!")
```

**Explicação**:
- Útil para prevenir travamentos ou lentidão em chamadas de rede ou I/O.
- `TimeoutError` pode ser tratado como fallback ou retry.

---

## ⚡ Tratamento de Exceções em Corrotinas

Erros acontecem — e precisam ser capturados para não comprometer o loop de eventos.

```python
async def tarefa_com_erro():
    raise ValueError("Ocorreu um erro na tarefa")

async def main():
    try:
        await tarefa_com_erro()
    except ValueError as e:
        print(f"Erro capturado: {e}")
```

**Explicação**:
- Sempre trate exceções ao usar `await` para manter o controle da aplicação.

---

## ✨ Corrotinas dentro de Corrotinas

Permite criar fluxos assíncronos mais complexos e encadeados.

```python
async def tarefa1():
    print("Tarefa 1 iniciada")
    await asyncio.sleep(1)
    print("Tarefa 1 concluída")
    return "Resultado 1"

async def tarefa2():
    print("Tarefa 2 iniciada")
    await asyncio.sleep(2)
    print("Tarefa 2 concluída")
    return "Resultado 2"

async def main():
    resultado1 = await tarefa1()
    resultado2 = await tarefa2()
    print(f"Resultados: {resultado1}, {resultado2}")
```

**Explicação**:
- Cada coroutine pode aguardar outras, permitindo controle total da ordem de execução.

---

## 🌐 Async Generators

Geração assíncrona de valores — ideal para grandes volumes de dados ou streams.

```python
async def gerar_numeros(maximo):
    numero = 0
    while numero < maximo:
        yield numero
        numero += 1
        await asyncio.sleep(0.5)

async def main():
    async for numero in gerar_numeros(5):
        print(f"Número: {numero}")
```

**Explicação**:
- `yield` em conjunto com `await` permite produzir dados em tempo real sem bloquear a aplicação.

---

## 🔮 Dicas e Truques com `asyncio`

- **`gather()`**: executa várias tarefas ao mesmo tempo.
- **`to_thread()`**: roda funções bloqueantes em uma thread separada.
- **`get_event_loop()`**: acesso manual ao loop para controle fino.
- **Use bibliotecas assíncronas**: como `aiohttp`, `aiomysql`, `aiosqlite`, etc.

---

## ⚖️ Comparativo Final

| Conceito                    | Python                          | Ruby                             |
|----------------------------|----------------------------------|----------------------------------|
| Assíncrono nativo          | ✅ asyncio                       | ⚠️ via gem (async, EventMachine) |
| Cancelamento de tarefas    | ✅ cancel(), try/except          | ⚠️ task.stop, escopos manuais    |
| Timeout                    | ✅ wait_for()                    | ⚠️ Async::Timeout                |
| `async generators`         | ✅ sim                          | ❌ não diretamente                |
| Criação dinâmica de classe | ✅ `type()`                      | ✅ `define_method`, `class_eval` |
| Metaclasses                | ✅ via `type`, `__new__`         | ✅ via `Class.new`, `Module`     |
| AST e introspecção         | ✅ `ast`, `inspect`              | ⚠️ introspecção limitada         |
| Monkey patching            | ✅ sim                          | ✅ muito comum                   |

---



## 🧩 Conclusão

A combinação de programação assíncrona e metaprogramação representa o ápice da expressividade e controle sobre o fluxo e a estrutura de um programa — e tanto Python quanto Ruby oferecem abordagens poderosas, ainda que com focos diferentes.

🐍 **Python:**

* O `asyncio` provê uma estrutura robusta, eficiente e escalável para lidar com tarefas simultâneas, I/O não bloqueante e fluxos de controle altamente performáticos.
* A metaprogramação com `type()`, metaclasses, descriptors e AST torna o Python uma linguagem introspectiva e dinâmica, excelente para automações, frameworks e DSLs controladas.

💎 **Ruby:**

* O Ruby brilha pela elegância e concisão. Com `define_method`, `method_missing`, `eval` e `Module#prepend`, a metaprogramação se torna fluida e poderosa.
* Para tarefas assíncronas, gems como `async`, `eventmachine` e `celluloid` trazem capacidades equivalentes, especialmente úteis para servidores e aplicações reativas.

🧠 **Em resumo:**

* Se seu foco é concorrência com controle preciso do loop de eventos, Python entrega isso de forma mais direta.
* Se você deseja criar DSLs, estruturas dinâmicas, ou modificar comportamento de código com leveza, Ruby é imbatível na metaprogramação.

Ambas linguagens são ferramentas de alto nível — e saber usá-las com consciência pode transformar a forma como você escreve e entende software.


