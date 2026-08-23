# Factory Method Design Pattern 🏭

O **Factory Method**, ou Método Fábrica, é um padrão de projeto criacional que define uma interface para criar objetos, mas permite que as subclasses decidam qual classe será instanciada. Ele é uma solução elegante para evitar **acoplamento direto** entre o código cliente e as classes concretas.

---

## Contexto Histórico 🕰️

O Factory Method foi descrito no livro _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994), criado pelo famoso **Gang of Four**. Durante os anos 90, o padrão foi amplamente adotado para resolver problemas de **flexibilidade na criação de objetos** em sistemas orientados a objetos.

---

## O que é o Factory Method? 🤔

O Factory Method permite delegar a responsabilidade de criar objetos para subclasses ou classes específicas, promovendo a **flexibilidade** e o **princípio de aberto/fechado (OCP)**. 

Em vez de instanciar objetos diretamente usando `new`, o código cliente chama um método fábrica que retorna uma instância do objeto.

---

## Quando Usar o Factory Method? 🛠️

1. **Quando você precisa de flexibilidade na criação de objetos:**  
   O código cliente não precisa saber detalhes sobre as classes concretas.

2. **Quando o processo de criação é complexo:**  
   Você pode encapsular a lógica de criação em um método ou classe fábrica.

3. **Quando diferentes subclasses ou configurações requerem diferentes instâncias:**  
   O Factory Method permite que subclasses definam o tipo de objeto a ser criado.

4. **Para reduzir o acoplamento entre código cliente e classes concretas:**  
   Isso facilita a manutenção e a evolução do sistema.

---

## Benefícios do Factory Method 🌟

1. **Desacoplamento:**  
   O cliente não precisa saber qual classe concreta será usada.

2. **Flexibilidade:**  
   Permite adicionar novos tipos de objetos sem alterar o código existente.

3. **Reutilização de código:**  
   A lógica de criação é centralizada, evitando duplicação.

---

## Desvantagens do Factory Method 🚨

1. **Complexidade Adicional:**  
   Adicionar fábricas pode tornar o código mais complicado em sistemas simples.

2. **Subclasses Necessárias:**  
   O padrão frequentemente exige a criação de subclasses para definir objetos específicos.

---

## Exemplo Prático: Sistema de Transporte 🎓

Imagine um sistema que precisa criar diferentes tipos de transporte, como **carros** e **barcos**. Em vez de instanciar as classes concretas diretamente, usaremos o Factory Method.

---

### Implementação

#### **Interface de Transporte**

```javascript
// transport.js
class Transport {
  deliver() {
    throw new Error("Método 'deliver()' deve ser implementado.");
  }
}

module.exports = Transport;
```

---

#### **Classes Concretas**

```javascript
// truck.js
const Transport = require('./transport');

class Truck extends Transport {
  deliver() {
    console.log("Entregando por estrada em um caminhão.");
  }
}

module.exports = Truck;

// ship.js
const Transport = require('./transport');

class Ship extends Transport {
  deliver() {
    console.log("Entregando por mar em um navio.");
  }
}

module.exports = Ship;
```

---

#### **Classe Base com Factory Method**

```javascript
// logistics.js
class Logistics {
  createTransport() {
    throw new Error("Método 'createTransport()' deve ser implementado.");
  }

  planDelivery() {
    const transport = this.createTransport();
    transport.deliver();
  }
}

module.exports = Logistics;
```

---

#### **Subclasses da Fábrica**

```javascript
// roadLogistics.js
const Logistics = require('./logistics');
const Truck = require('./truck');

class RoadLogistics extends Logistics {
  createTransport() {
    return new Truck();
  }
}

module.exports = RoadLogistics;

// seaLogistics.js
const Logistics = require('./logistics');
const Ship = require('./ship');

class SeaLogistics extends Logistics {
  createTransport() {
    return new Ship();
  }
}

module.exports = SeaLogistics;
```

---

#### **Uso do Factory Method**

```javascript
const RoadLogistics = require('./roadLogistics');
const SeaLogistics = require('./seaLogistics');

// Planejamento de entrega terrestre
const roadLogistics = new RoadLogistics();
roadLogistics.planDelivery(); // Saída: "Entregando por estrada em um caminhão."

// Planejamento de entrega marítima
const seaLogistics = new SeaLogistics();
seaLogistics.planDelivery(); // Saída: "Entregando por mar em um navio."
```

---

## Quando Evitar o Factory Method? ❌

- **Em sistemas simples:**  
  Se o processo de criação não for complexo, o Factory Method pode adicionar complexidade desnecessária.

- **Se as subclasses não forem necessárias:**  
  Em cenários onde apenas um tipo de objeto será criado, o padrão Factory Method não é necessário.

---

## Alternativas ao Factory Method 🌍

1. **Abstract Factory:**  
   Quando você precisa criar famílias inteiras de objetos relacionados.

2. **Builder:**  
   Para criar objetos complexos com muitas etapas de configuração.

---

## Conclusão 🎯

O Factory Method é um padrão poderoso para **desacoplar a lógica de criação de objetos** do código cliente, promovendo flexibilidade e manutenção. Embora introduza complexidade adicional, ele é indispensável em sistemas que precisam gerenciar **múltiplas variantes de objetos** ou abstrair o processo de criação.

Se usado com sabedoria, o Factory Method pode transformar a forma como você organiza e expande seu sistema! 🚀

---

**Pronto para explorar outro padrão? Diga-me qual você quer "detonar" a seguir! 😉**
