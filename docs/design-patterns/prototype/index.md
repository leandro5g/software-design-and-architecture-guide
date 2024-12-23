# Prototype Design Pattern 🧬

O **Prototype** é um padrão de projeto criacional que permite criar novos objetos copiando instâncias existentes, ao invés de construir um objeto do zero. Ele é especialmente útil quando a criação de um objeto envolve um custo elevado de tempo ou recursos.

---

## Contexto Histórico 🕰️

O Prototype foi introduzido pelo **Gang of Four** no livro _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994). Ele foi inspirado na necessidade de criar objetos complexos ou custosos de forma eficiente e reutilizável.

Imagine que você está modelando um editor gráfico e precisa duplicar formas com configurações específicas. Criar novos objetos a partir de protótipos existentes é muito mais eficiente do que configurá-los novamente do zero.

---

## O que é o Prototype? 🤔

O Prototype permite **copiar objetos existentes** sem depender de suas classes concretas. Isso é feito usando um método `clone`, que cria uma nova instância baseada nos valores do objeto original.

Ele é amplamente usado em sistemas onde os objetos têm muitas configurações ou são criados frequentemente com pequenas variações.

---

## Quando Usar o Prototype? 🛠️

1. **Objetos Complexos:**  
   Quando criar um objeto do zero é um processo caro ou demorado.

2. **Variações de Objetos:**  
   Quando você precisa criar várias cópias de objetos com pequenas modificações.

3. **Evitar Dependência de Classes Concretas:**  
   O Prototype abstrai o processo de cópia, reduzindo o acoplamento.

---

## Benefícios do Prototype 🌟

1. **Eficiência:**  
   Copiar objetos é mais rápido e econômico do que criá-los do zero.

2. **Flexibilidade:**  
   Permite criar novas instâncias sem depender de classes concretas.

3. **Consistência:**  
   As cópias herdam o estado do objeto original, garantindo que configurações complexas sejam preservadas.

---

## Desvantagens do Prototype 🚨

1. **Clonagem Profunda:**  
   Implementar clonagem profunda pode ser complicado em objetos com referências aninhadas.

2. **Dependência de Estado:**  
   As cópias herdam o estado do objeto original, o que pode causar efeitos colaterais se não forem isoladas adequadamente.

---

## Exemplo Prático: Formas Geométricas 🖼️

Vamos criar um sistema para copiar formas geométricas (círculos, retângulos), mantendo as propriedades e permitindo modificações.

---

### Implementação

#### **Classe Base**

```javascript
// Shape.js
class Shape {
  constructor(color) {
    this.color = color;
  }

  clone() {
    throw new Error("Método 'clone()' deve ser implementado.");
  }

  displayDetails() {
    console.log(`Forma com cor: ${this.color}`);
  }
}

module.exports = Shape;
```

---

#### **Classes Concretas**

```javascript
// Circle.js
const Shape = require('./Shape');

class Circle extends Shape {
  constructor(color, radius) {
    super(color);
    this.radius = radius;
  }

  clone() {
    return new Circle(this.color, this.radius);
  }

  displayDetails() {
    console.log(`Círculo com cor: ${this.color}, raio: ${this.radius}`);
  }
}

module.exports = Circle;

// Rectangle.js
const Shape = require('./Shape');

class Rectangle extends Shape {
  constructor(color, width, height) {
    super(color);
    this.width = width;
    this.height = height;
  }

  clone() {
    return new Rectangle(this.color, this.width, this.height);
  }

  displayDetails() {
    console.log(`Retângulo com cor: ${this.color}, largura: ${this.width}, altura: ${this.height}`);
  }
}

module.exports = Rectangle;
```

---

#### **Uso do Prototype**

```javascript
const Circle = require('./Circle');
const Rectangle = require('./Rectangle');

// Criação de formas originais
const originalCircle = new Circle('Vermelho', 10);
const originalRectangle = new Rectangle('Azul', 20, 15);

// Clonando formas
const clonedCircle = originalCircle.clone();
const clonedRectangle = originalRectangle.clone();

// Modificando clones
clonedCircle.color = 'Verde';
clonedRectangle.height = 25;

// Exibindo detalhes
originalCircle.displayDetails(); // Círculo com cor: Vermelho, raio: 10
clonedCircle.displayDetails(); // Círculo com cor: Verde, raio: 10

originalRectangle.displayDetails(); // Retângulo com cor: Azul, largura: 20, altura: 15
clonedRectangle.displayDetails(); // Retângulo com cor: Azul, largura: 20, altura: 25
```

---

## Quando Evitar o Prototype? ❌

- **Objetos Simples:**  
  Se a criação de objetos é rápida e direta, o Prototype pode ser excessivo.

- **Dependência de Estado:**  
  Se os objetos clonados precisam de isolamento completo, o Prototype pode exigir cuidado extra.

---

## Alternativas ao Prototype 🌍

1. **Builder:**  
   Ideal para criar objetos complexos passo a passo.

2. **Factory Method:**  
   Use quando você precisa criar objetos sem a necessidade de clonagem.

---

## Conclusão 🎯

O Prototype é um padrão poderoso para criar objetos complexos de forma eficiente e reutilizável. Ele reduz a necessidade de inicialização repetitiva e promove a flexibilidade, especialmente em sistemas que exigem muitas cópias ou variações de objetos.

Embora sua implementação exija atenção ao estado e à clonagem profunda, o Prototype é indispensável em sistemas que precisam balancear eficiência e flexibilidade! 🚀

---

**Quer aprender mais sobre padrões? Escolha o próximo para "detonarmos"! 😉**
