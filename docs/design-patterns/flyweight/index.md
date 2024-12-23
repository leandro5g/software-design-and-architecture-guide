# Flyweight Design Pattern 🪶

O **Flyweight**, ou Padrão de Peso-Mosca, é um padrão de projeto estrutural que foca na **otimização do uso de memória**. Ele faz isso compartilhando o máximo possível de dados entre objetos semelhantes, reduzindo o uso de memória em sistemas que criam grandes quantidades de objetos.

---

## Contexto Histórico 🕰️

O Flyweight foi descrito pelo **Gang of Four** no livro _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994). Ele nasceu da necessidade de otimizar sistemas que criam **milhares ou milhões de objetos**, como editores gráficos ou sistemas de renderização.

---

## O que é o Flyweight? 🤔

O Flyweight minimiza o consumo de memória compartilhando objetos semelhantes. Ele separa os dados em **intrínsecos** (compartilhados entre objetos) e **extrínsecos** (específicos para cada instância).

Imagine que você está desenhando um mapa de uma cidade com milhares de árvores. Em vez de criar uma instância para cada árvore, o Flyweight permite que você reutilize objetos com as mesmas propriedades, como espécie e cor, enquanto armazena apenas a localização individualmente.

---

## Quando Usar o Flyweight? 🛠️

1. **Muitos Objetos Similares:**  
   Quando o sistema precisa criar muitas instâncias de objetos semelhantes.

2. **Memória Limitada:**  
   Quando o uso de memória precisa ser otimizado.

3. **Comportamento Compartilhado:**  
   Quando os objetos compartilham a maior parte de seus dados.

---

## Benefícios do Flyweight 🌟

1. **Redução no Uso de Memória:**  
   Menos objetos são criados, economizando recursos.

2. **Performance Melhorada:**  
   A redução no número de objetos pode melhorar a velocidade do sistema.

3. **Estrutura Simplificada:**  
   Centraliza os dados compartilhados, tornando o sistema mais organizado.

---

## Desvantagens do Flyweight 🚨

1. **Complexidade Adicional:**  
   Separar dados intrínsecos e extrínsecos pode complicar a implementação.

2. **Dependência de Contexto:**  
   Os dados extrínsecos precisam ser gerenciados externamente.

---

## Exemplo Prático: Floresta 🌲

Vamos criar um exemplo onde modelamos uma floresta com milhares de árvores usando o Flyweight para otimizar o uso de memória.

---

### Implementação

#### **Classe Flyweight**

```javascript
// TreeType.js
class TreeType {
  constructor(name, color, texture) {
    this.name = name;
    this.color = color;
    this.texture = texture;
  }

  display(x, y) {
    console.log(
      `Árvore: ${this.name}, Cor: ${this.color}, Textura: ${this.texture}, Posição: (${x}, ${y})`
    );
  }
}

module.exports = TreeType;
```

---

#### **Fábrica de Flyweights**

```javascript
// TreeTypeFactory.js
const TreeType = require('./TreeType');

class TreeTypeFactory {
  constructor() {
    this.treeTypes = {};
  }

  getTreeType(name, color, texture) {
    const key = `${name}-${color}-${texture}`;
    if (!this.treeTypes[key]) {
      this.treeTypes[key] = new TreeType(name, color, texture);
    }
    return this.treeTypes[key];
  }
}

module.exports = new TreeTypeFactory();
```

---

#### **Classe Contexto**

```javascript
// Tree.js
class Tree {
  constructor(x, y, treeType) {
    this.x = x;
    this.y = y;
    this.treeType = treeType;
  }

  display() {
    this.treeType.display(this.x, this.y);
  }
}

module.exports = Tree;
```

---

#### **Classe Floresta**

```javascript
// Forest.js
const Tree = require('./Tree');
const TreeTypeFactory = require('./TreeTypeFactory');

class Forest {
  constructor() {
    this.trees = [];
  }

  plantTree(x, y, name, color, texture) {
    const treeType = TreeTypeFactory.getTreeType(name, color, texture);
    const tree = new Tree(x, y, treeType);
    this.trees.push(tree);
  }

  displayTrees() {
    for (const tree of this.trees) {
      tree.display();
    }
  }
}

module.exports = Forest;
```

---

#### **Uso do Flyweight**

```javascript
const Forest = require('./Forest');

const forest = new Forest();

// Plantando árvores
forest.plantTree(1, 2, 'Carvalho', 'Verde', 'Lisa');
forest.plantTree(2, 3, 'Carvalho', 'Verde', 'Lisa');
forest.plantTree(3, 4, 'Pinheiro', 'Verde Escuro', 'Rugosa');
forest.plantTree(4, 5, 'Carvalho', 'Verde', 'Lisa');

// Exibindo árvores
forest.displayTrees();
```

---

#### **Saída Esperada**

```
Árvore: Carvalho, Cor: Verde, Textura: Lisa, Posição: (1, 2)
Árvore: Carvalho, Cor: Verde, Textura: Lisa, Posição: (2, 3)
Árvore: Pinheiro, Cor: Verde Escuro, Textura: Rugosa, Posição: (3, 4)
Árvore: Carvalho, Cor: Verde, Textura: Lisa, Posição: (4, 5)
```

---

## Quando Evitar o Flyweight? ❌

- **Objetos Únicos:**  
  Se cada objeto é único e não compartilha propriedades, o Flyweight é desnecessário.

- **Baixo Volume de Objetos:**  
  Se o número de objetos é pequeno, a otimização de memória pode ser irrelevante.

---

## Alternativas ao Flyweight 🌍

1. **Prototype:**  
   Use quando precisar copiar objetos existentes de forma eficiente.

2. **Caching:**  
   Armazene instâncias reutilizáveis em cache sem implementar um Flyweight formal.

---

## Conclusão 🎯

O Flyweight é um padrão essencial para sistemas que precisam lidar com **grande volume de objetos semelhantes**, promovendo eficiência no uso de memória e performance. Embora introduza complexidade adicional, ele é indispensável em sistemas como renderizadores gráficos, jogos e editores de texto.

Se usado corretamente, o Flyweight pode transformar sistemas intensivos em memória em soluções elegantes e escaláveis! 🚀

---

**Quer explorar outro padrão? Diga-me qual você quer "detonar" a seguir! 😉**
