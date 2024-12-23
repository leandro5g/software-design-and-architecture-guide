 Builder Design Pattern 🏗️

O **Builder** é um padrão de projeto criacional que permite construir objetos complexos de forma **incremental** e flexível. Ele separa o processo de construção do objeto de sua representação final, permitindo criar diferentes variações de um mesmo objeto com o mesmo código de construção.

---

## Contexto Histórico 🕰️

O Builder foi descrito pelo **Gang of Four** no clássico livro _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994). Sua origem está na necessidade de construir objetos com muitos atributos opcionais ou dependentes, sem criar um construtor cheio de parâmetros ou uma infinidade de subclasses.

---

## O que é o Builder? 🤔

O Builder é um padrão que delega a criação de um objeto complexo para **uma série de etapas definidas por um construtor (builder)**. O cliente controla o processo de construção chamando métodos específicos na ordem desejada.

Imagine um pedido de pizza 🍕: você escolhe cada ingrediente (massa, molho, cobertura) passo a passo e, no final, recebe a pizza montada. O Builder funciona exatamente assim!

---

## Quando Usar o Builder? 🛠️

1. **Objetos Complexos:**  
   Quando a criação de um objeto envolve muitas etapas ou depende de configurações opcionais.

2. **Representações Diferentes:**  
   Quando o mesmo processo de construção deve gerar diferentes representações do objeto.

3. **Evolução de Objetos:**  
   Quando você precisa adicionar etapas de construção sem modificar o código existente.

---

## Benefícios do Builder 🌟

1. **Separação da Construção:**  
   O código que constrói o objeto é separado da lógica de construção.

2. **Flexibilidade:**  
   Permite construir diferentes representações do mesmo objeto sem duplicação de código.

3. **Facilidade de Leitura:**  
   O código do cliente é mais claro, já que os métodos do Builder tornam o processo de construção intuitivo.

---

## Desvantagens do Builder 🚨

1. **Complexidade Adicional:**  
   Pode ser excessivo para objetos simples.

2. **Código Verboso:**  
   Requer a criação de várias classes e métodos, o que pode aumentar a quantidade de código.

---

## Exemplo Prático: Construtor de Carros 🚗

Vamos construir um sistema que cria diferentes tipos de carros com várias configurações, como motor, cor e acessórios.

---

### Implementação

#### **Classe do Produto**

```javascript
// Car.js
class Car {
  constructor() {
    this.engine = null;
    this.color = null;
    this.features = [];
  }

  displayDetails() {
    console.log(`Carro com motor ${this.engine}, cor ${this.color}, acessórios: ${this.features.join(', ')}`);
  }
}

module.exports = Car;
```

---

#### **Interface do Builder**

```javascript
// CarBuilder.js
const Car = require('./Car');

class CarBuilder {
  constructor() {
    this.car = new Car();
  }

  setEngine(engine) {
    this.car.engine = engine;
    return this;
  }

  setColor(color) {
    this.car.color = color;
    return this;
  }

  addFeature(feature) {
    this.car.features.push(feature);
    return this;
  }

  build() {
    return this.car;
  }
}

module.exports = CarBuilder;
```

---

#### **Diretor (Opcional)**

O Diretor é opcional no padrão Builder. Ele controla o processo de construção para criar objetos pré-configurados.

```javascript
// CarDirector.js
const CarBuilder = require('./CarBuilder');

class CarDirector {
  static buildSportsCar() {
    return new CarBuilder()
      .setEngine('V8')
      .setColor('Vermelho')
      .addFeature('Aro 18 esportivo')
      .addFeature('Banco de couro')
      .build();
  }

  static buildSUV() {
    return new CarBuilder()
      .setEngine('V6')
      .setColor('Preto')
      .addFeature('Suspensão elevada')
      .addFeature('Trilha sonora Bose')
      .build();
  }
}

module.exports = CarDirector;
```

---

#### **Uso do Builder**

```javascript
const CarBuilder = require('./CarBuilder');
const CarDirector = require('./CarDirector');

// Construção manual de um carro
const builder = new CarBuilder();
const sedan = builder
  .setEngine('1.6 Flex')
  .setColor('Prata')
  .addFeature('Ar-condicionado')
  .addFeature('Central multimídia')
  .build();

sedan.displayDetails(); // Carro com motor 1.6 Flex, cor Prata, acessórios: Ar-condicionado, Central multimídia

// Construção com o Diretor
const sportsCar = CarDirector.buildSportsCar();
sportsCar.displayDetails(); // Carro com motor V8, cor Vermelho, acessórios: Aro 18 esportivo, Banco de couro

const suv = CarDirector.buildSUV();
suv.displayDetails(); // Carro com motor V6, cor Preto, acessórios: Suspensão elevada, Trilha sonora Bose
```

---

## Quando Evitar o Builder? ❌

- **Objetos Simples:**  
  Se o objeto não tem muitos atributos ou configurações, o Builder pode ser excessivo.

- **Necessidade de Construtores Simples:**  
  Se o cliente precisa instanciar objetos rapidamente, o Builder pode tornar o processo mais lento.

---

## Alternativas ao Builder 🌍

1. **Factory Method:**  
   Use quando você precisa criar objetos simples com variações.

2. **Prototype:**  
   Ideal para copiar objetos complexos sem reconstruí-los.

---

## Conclusão 🎯

O Builder é um padrão ideal para construir **objetos complexos** de forma controlada e incremental. Ele separa a lógica de construção da lógica do produto, permitindo um código mais organizado e fácil de entender.

Embora seja mais complexo que construtores simples, o Builder traz clareza e flexibilidade em projetos onde a criação de objetos precisa de personalização detalhada.

Se aplicado corretamente, o Builder pode transformar a maneira como você cria objetos complexos no seu sistema! 🚀

---

**Pronto para explorar outro padrão? Escolha o próximo e vamos "detonar"! 😉**
