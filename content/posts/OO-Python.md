---
title: "🚗 Veículos Orientado a Objetos (Python OOP)"
date: 2025-05-04
tags: ["Python", "POO", "OOP", "Design de Software", "Jogos", "Simulação"]
description: "Simule um mundo de veículos interativos com Python e orientação a objetos, usando conceitos como herança, interfaces, injeção de dependência e mixins."
weight: 1
draft: false
---

Vamos aplicar os pilares da **Orientação a Objetos**.

---

## 🧱 1. Classe Base: o Molde dos Veículos

> ```python
> from abc import ABC, abstractmethod
> 
> class Veiculo(ABC):
>     def __init__(self, cor, marca, modelo):
>         self.cor = cor
>         self.marca = marca
>         self.modelo = modelo
> 
>     @abstractmethod
>     def dirigir(self):
>         pass
> ```

---

## ⚡ 2. Veículos Específicos: Gasolina e Elétrico

> ```python
> class CarroGasolina(Veiculo):
>     def dirigir(self):
>         print(f"🚘 {self.marca} {self.modelo} está rugindo com gasolina!")
> 
> class CarroEletrico(Veiculo):
>     def dirigir(self):
>         print(f"🔋 {self.marca} {self.modelo} está deslizando com energia elétrica!")
> ```

---

## ⚙️ 3. Componentes Plugáveis com Injeção de Dependência

> ```python
> class Bateria:
>     def __init__(self, capacidade):
>         self.capacidade = capacidade
> 
>     def obter_capacidade(self):
>         return f"{self.capacidade} kWh"
> 
> class Tanque:
>     def __init__(self, litros):
>         self.litros = litros
> 
>     def obter_capacidade(self):
>         return f"{self.litros} litros"
> ```

> ```python
> class CarroEletrico(Veiculo):
>     def __init__(self, cor, marca, modelo, bateria: Bateria):
>         super().__init__(cor, marca, modelo)
>         self.bateria = bateria
> 
>     def dirigir(self):
>         print(f"🔋 Dirigindo {self.marca} com bateria de {self.bateria.obter_capacidade()}")
> ```

---

## 🎮 4. Interface de Ação: Dirigível e Acelerável

> ```python
> class Aceleravel(ABC):
>     @abstractmethod
>     def acelerar(self):
>         pass
> 
> class CarroCorrida(CarroGasolina, Aceleravel):
>     def acelerar(self):
>         print("🏎️ Acelerando a toda velocidade!")
> ```

---

## 🧩 5. Mixins: Funcionalidades Extras

> ```python
> class Imprimivel:
>     def imprimir_info(self):
>         print(f"🔍 STATUS: {self.__dict__}")
> 
> class CarroVIP(CarroEletrico, Imprimivel):
>     pass
> ```

---

## 🕹️ 6. Simulando uma Corrida

> ```python
> def simular_veiculo(veiculo):
>     print("==== Simulação Iniciada ====")
>     veiculo.dirigir()
> 
>     if isinstance(veiculo, Aceleravel):
>         veiculo.acelerar()
> 
>     if hasattr(veiculo, "imprimir_info"):
>         veiculo.imprimir_info()
> ```

---

## 🧪 7. Testando no Terminal

> ```python
> bateria = Bateria(100)
> tanque = Tanque(45)
> 
> tesla = CarroVIP("Azul", "Tesla", "Model 3", bateria)
> mustang = CarroCorrida("Vermelho", "Ford", "Mustang")
> 
> simular_veiculo(tesla)
> print()
> simular_veiculo(mustang)
> ```

---

## ✅ Conclusão

| Conceito               | Carro Obj                      | O que representa              |
| ---------------------- | ------------------------------ | ----------------------------- |
| `class`                | Moldes dos veículos            | Estrutura base                |
| Herança                | CarroGasolina herda de Veiculo | Reuso de comportamento        |
| Interface              | `Aceleravel`, `Dirigivel`      | Ações que certos carros fazem |
| Injc dep               | Plug baterias e tanques        | Flexibilidade e testes        |
| Mixin                  | `Imprimivel`                   | Funcionalidade plugável       |

