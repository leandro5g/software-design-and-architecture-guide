# Abstract Factory Design Pattern 🏭🏢

O **Abstract Factory**, ou Fábrica Abstrata, é um padrão de projeto criacional que fornece uma interface para criar **famílias de objetos relacionados ou dependentes** sem especificar suas classes concretas. Ele é uma extensão do Factory Method, focado em **grupos de objetos** que precisam ser criados juntos.

---

## Contexto Histórico 🕰️

Assim como o Factory Method, o Abstract Factory foi popularizado pelo **Gang of Four** em _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994). Esse padrão nasceu da necessidade de gerenciar a criação de **famílias de objetos compatíveis** em sistemas que precisavam de maior flexibilidade.

---

## O que é o Abstract Factory? 🤔

O Abstract Factory define uma **fábrica abstrata** que encapsula a lógica de criação de objetos relacionados. Essa fábrica delega a responsabilidade de instanciar objetos concretos para subclasses específicas, mantendo o código cliente **independente de classes concretas**.

Imagine que você está construindo um sistema que precisa criar componentes visuais para **diferentes temas** (e.g., Light e Dark). Com o Abstract Factory, você pode centralizar a criação de botões, campos de texto e outros elementos, garantindo que eles pertençam ao mesmo tema.

---

## Quando Usar o Abstract Factory? 🛠️

1. **Quando você precisa criar famílias de objetos relacionados:**  
   Exemplo: Componentes visuais de um tema (botões, textos, checkboxes).

2. **Para garantir consistência entre objetos criados:**  
   Exemplo: Um sistema deve criar interfaces gráficas compatíveis entre si.

3. **Quando o código cliente não deve conhecer as classes concretas:**  
   Isso promove o princípio de aberto/fechado (OCP).

---

## Benefícios do Abstract Factory 🌟

1. **Desacoplamento:**  
   O código cliente não precisa conhecer os detalhes das classes concretas.

2. **Consistência:**  
   Garante que os objetos criados pertencem à mesma família.

3. **Flexibilidade:**  
   Facilita a adição de novas famílias de objetos sem modificar o código existente.

---

## Desvantagens do Abstract Factory 🚨

1. **Complexidade Adicional:**  
   Pode aumentar a complexidade inicial do sistema, especialmente em projetos simples.

2. **Exige Subclasses:**  
   Para cada família de objetos, você precisa criar uma nova implementação da fábrica.

---

## Exemplo Prático: Interface Gráfica Multitema 🎨

Vamos construir um sistema que cria componentes gráficos (botões e checkboxes) para dois temas: **Light** e **Dark**.

---

### Implementação

#### **Interface de Produtos**

```javascript
// button.js
class Button {
  render() {
    throw new Error("Método 'render()' deve ser implementado.");
  }
}

module.exports = Button;

// checkbox.js
class Checkbox {
  render() {
    throw new Error("Método 'render()' deve ser implementado.");
  }
}

module.exports = Checkbox;
```

---

#### **Implementações Concretas**

```javascript
// lightButton.js
const Button = require('./button');

class LightButton extends Button {
  render() {
    console.log('Renderizando botão no tema Light.');
  }
}

module.exports = LightButton;

// darkButton.js
const Button = require('./button');

class DarkButton extends Button {
  render() {
    console.log('Renderizando botão no tema Dark.');
  }
}

module.exports = DarkButton;

// lightCheckbox.js
const Checkbox = require('./checkbox');

class LightCheckbox extends Checkbox {
  render() {
    console.log('Renderizando checkbox no tema Light.');
  }
}

module.exports = LightCheckbox;

// darkCheckbox.js
const Checkbox = require('./checkbox');

class DarkCheckbox extends Checkbox {
  render() {
    console.log('Renderizando checkbox no tema Dark.');
  }
}

module.exports = DarkCheckbox;
```

---

#### **Interface da Fábrica Abstrata**

```javascript
// uiFactory.js
class UIFactory {
  createButton() {
    throw new Error("Método 'createButton()' deve ser implementado.");
  }

  createCheckbox() {
    throw new Error("Método 'createCheckbox()' deve ser implementado.");
  }
}

module.exports = UIFactory;
```

---

#### **Fábricas Concretas**

```javascript
// lightThemeFactory.js
const UIFactory = require('./uiFactory');
const LightButton = require('./lightButton');
const LightCheckbox = require('./lightCheckbox');

class LightThemeFactory extends UIFactory {
  createButton() {
    return new LightButton();
  }

  createCheckbox() {
    return new LightCheckbox();
  }
}

module.exports = LightThemeFactory;

// darkThemeFactory.js
const UIFactory = require('./uiFactory');
const DarkButton = require('./darkButton');
const DarkCheckbox = require('./darkCheckbox');

class DarkThemeFactory extends UIFactory {
  createButton() {
    return new DarkButton();
  }

  createCheckbox() {
    return new DarkCheckbox();
  }
}

module.exports = DarkThemeFactory;
```

---

#### **Uso do Abstract Factory**

```javascript
const LightThemeFactory = require('./lightThemeFactory');
const DarkThemeFactory = require('./darkThemeFactory');

function renderUI(factory) {
  const button = factory.createButton();
  const checkbox = factory.createCheckbox();

  button.render();
  checkbox.render();
}

// Renderizando tema Light
const lightFactory = new LightThemeFactory();
renderUI(lightFactory);

// Renderizando tema Dark
const darkFactory = new DarkThemeFactory();
renderUI(darkFactory);
```

---

## Quando Evitar o Abstract Factory? ❌

- **Sistemas Simples:**  
  Se você não precisa criar múltiplas famílias de objetos, o Abstract Factory pode adicionar complexidade desnecessária.

- **Baixa Probabilidade de Mudança:**  
  Se o sistema não está sujeito a mudanças frequentes de famílias de objetos, o padrão pode ser excessivo.

---

## Alternativas ao Abstract Factory 🌍

1. **Factory Method:**  
   Use o Factory Method se você precisa criar um único objeto, em vez de uma família.

2. **Protótipo:**  
   Ideal para copiar objetos existentes sem passar por um processo de criação complexo.

---

## Conclusão 🎯

O Abstract Factory é um padrão poderoso para sistemas que precisam criar **famílias de objetos relacionados ou dependentes**. Ele promove a **manutenção**, **flexibilidade** e **consistência**, mas deve ser usado com cautela em sistemas pequenos ou com poucos requisitos de expansão.

Se aplicado corretamente, o Abstract Factory pode simplificar a criação de objetos em projetos complexos e evolutivos! 🚀

---

**Quer explorar mais padrões? Escolha o próximo para "detonarmos"! 😉**
