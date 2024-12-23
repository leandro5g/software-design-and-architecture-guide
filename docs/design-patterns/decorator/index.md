# Decorator Design Pattern 🎨

O **Decorator**, ou Decorador, é um padrão de projeto estrutural que permite adicionar **comportamento ou responsabilidades adicionais a objetos de forma dinâmica**, sem alterar sua estrutura original. Ele é uma alternativa flexível à herança, permitindo modificar o comportamento de um objeto em tempo de execução.

---

## Contexto Histórico 🕰️

O Decorator foi descrito pelo **Gang of Four** no livro _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994). Ele foi projetado para resolver o problema de adicionar funcionalidades a objetos de forma flexível, sem criar subclasses para cada combinação de comportamentos.

---

## O que é o Decorator? 🤔

O Decorator envolve um objeto original em outro objeto (o decorador), que **intercepta as chamadas** e pode adicionar ou modificar funcionalidades. Isso permite empilhar vários decoradores, criando um fluxo de comportamento dinâmico.

Imagine um sistema de pedidos onde você pode adicionar complementos a um café (e.g., leite, caramelo, chantilly). Cada complemento pode ser adicionado dinamicamente sem alterar a classe original.

---

## Quando Usar o Decorator? 🛠️

1. **Adição Dinâmica de Funcionalidades:**  
   Quando você precisa adicionar comportamentos a objetos em tempo de execução.

2. **Evitar Herança Excessiva:**  
   Quando criar subclasses para cada combinação de funcionalidades seria inviável.

3. **Modificação Transparente:**  
   Quando os clientes não devem perceber que um objeto foi decorado.

---

## Benefícios do Decorator 🌟

1. **Flexibilidade:**  
   Permite adicionar ou remover funcionalidades de objetos de forma dinâmica.

2. **Reutilização de Código:**  
   Comportamentos podem ser encapsulados em decoradores reutilizáveis.

3. **Simplicidade Estrutural:**  
   Evita a explosão de subclasses para cada combinação de funcionalidades.

---

## Desvantagens do Decorator 🚨

1. **Complexidade Adicional:**  
   Pode gerar pilhas de decoradores difíceis de rastrear.

2. **Dependência Implícita:**  
   Comportamentos empilhados podem se tornar confusos se não documentados adequadamente.

---

## Exemplo Prático: Sistema de Cafeteria ☕

Vamos implementar um exemplo onde decoramos bebidas com complementos como leite e caramelo.

---

### Implementação

#### **Interface Base**

```javascript
// Beverage.js
class Beverage {
  getDescription() {
    throw new Error("Método 'getDescription()' deve ser implementado.");
  }

  cost() {
    throw new Error("Método 'cost()' deve ser implementado.");
  }
}

module.exports = Beverage;
```

---

#### **Classe Concreta**

```javascript
// Coffee.js
const Beverage = require('./Beverage');

class Coffee extends Beverage {
  getDescription() {
    return 'Café';
  }

  cost() {
    return 5.0; // Custo base do café
  }
}

module.exports = Coffee;
```

---

#### **Decorador Base**

```javascript
// BeverageDecorator.js
const Beverage = require('./Beverage');

class BeverageDecorator extends Beverage {
  constructor(beverage) {
    super();
    this.beverage = beverage;
  }

  getDescription() {
    return this.beverage.getDescription();
  }

  cost() {
    return this.beverage.cost();
  }
}

module.exports = BeverageDecorator;
```

---

#### **Decoradores Concretos**

```javascript
// MilkDecorator.js
const BeverageDecorator = require('./BeverageDecorator');

class MilkDecorator extends BeverageDecorator {
  getDescription() {
    return `${this.beverage.getDescription()} com leite`;
  }

  cost() {
    return this.beverage.cost() + 1.5; // Custo adicional do leite
  }
}

module.exports = MilkDecorator;

// CaramelDecorator.js
const BeverageDecorator = require('./BeverageDecorator');

class CaramelDecorator extends BeverageDecorator {
  getDescription() {
    return `${this.beverage.getDescription()} com caramelo`;
  }

  cost() {
    return this.beverage.cost() + 2.0; // Custo adicional do caramelo
  }
}

module.exports = CaramelDecorator;
```

---

#### **Uso do Decorator**

```javascript
const Coffee = require('./Coffee');
const MilkDecorator = require('./MilkDecorator');
const CaramelDecorator = require('./CaramelDecorator');

// Café simples
let beverage = new Coffee();
console.log(beverage.getDescription()); // Saída: "Café"
console.log(beverage.cost()); // Saída: 5.0

// Café com leite
beverage = new MilkDecorator(beverage);
console.log(beverage.getDescription()); // Saída: "Café com leite"
console.log(beverage.cost()); // Saída: 6.5

// Café com leite e caramelo
beverage = new CaramelDecorator(beverage);
console.log(beverage.getDescription()); // Saída: "Café com leite com caramelo"
console.log(beverage.cost()); // Saída: 8.5
```

---

## Quando Evitar o Decorator? ❌

- **Sistemas Simples:**  
  Se você não precisa de comportamentos dinâmicos, o Decorator pode ser desnecessário.

- **Empilhamento Excessivo:**  
  Quando muitas camadas de decoradores podem dificultar a depuração e manutenção.

---

## Alternativas ao Decorator 🌍

1. **Herança:**  
   Útil em sistemas pequenos onde combinações de comportamento são limitadas.

2. **Strategy:**  
   Permite alternar entre comportamentos dinamicamente sem empilhar decoradores.

---

## Conclusão 🎯

O Decorator é um padrão poderoso para adicionar **comportamentos dinâmicos** a objetos de forma elegante e reutilizável. Ele promove flexibilidade e reutilização de código, especialmente em sistemas onde a personalização de objetos é essencial.

Embora exija cuidado para evitar pilhas complexas, o Decorator pode transformar a maneira como você projeta funcionalidades dinâmicas no seu sistema! 🚀

---

**Pronto para explorar outro padrão? Diga-me qual você quer "detonar" a seguir! 😉**
