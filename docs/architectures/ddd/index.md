# Domain-Driven Design (DDD) 🏗️

O **Domain-Driven Design (DDD)**, ou Design Orientado ao Domínio, é um padrão arquitetural e de modelagem proposto por Eric Evans em seu livro _Domain-Driven Design: Tackling Complexity in the Heart of Software_. O foco do DDD é alinhar o desenvolvimento de software ao **domínio de negócio** e suas regras, garantindo que o código represente fielmente a realidade do negócio.

---

## O que é Domain-Driven Design? 🌐

No DDD, o desenvolvimento é guiado pelo **domínio do problema**, ou seja, pelo conjunto de regras, processos e conceitos do negócio. A ideia central é usar a linguagem ubíqua (**Ubiquitous Language**) para que tanto os desenvolvedores quanto os especialistas de domínio (stakeholders) compartilhem o mesmo entendimento.

### Componentes Essenciais do DDD 🧩

1. **Entidades (Entities):** Objetos com identidade única no domínio (e.g., Usuário, Pedido).
2. **Objetos de Valor (Value Objects):** Objetos que representam valores imutáveis sem identidade única (e.g., Dinheiro, Endereço).
3. **Agregados (Aggregates):** Conjuntos de entidades e objetos de valor que formam uma unidade lógica.
4. **Repositórios (Repositories):** Responsáveis por gerenciar o ciclo de vida das entidades e agregados.
5. **Serviços de Domínio (Domain Services):** Regras de negócio que não pertencem a uma única entidade ou agregado.
6. **Contextos Delimitados (Bounded Contexts):** Divisões claras dentro do domínio, onde cada contexto tem seu modelo.
7. **Eventos de Domínio (Domain Events):** Notificações sobre algo importante que aconteceu no domínio.

---

## Vantagens e Desvantagens do DDD

### Vantagens 🌟

1. **Alinhamento com o Negócio:**
   - O software reflete o domínio, tornando o código mais compreensível para os stakeholders.

2. **Gestão de Complexidade:**
   - A separação em contextos delimitados ajuda a organizar sistemas complexos.

3. **Evolução Sustentável:**
   - Mudanças no domínio podem ser refletidas no código de forma natural.

4. **Cooperação Entre Equipes:**
   - A linguagem ubíqua reduz mal-entendidos entre desenvolvedores e especialistas de domínio.

### Desvantagens ❌

1. **Complexidade Inicial:**
   - Exige um entendimento profundo do domínio e um esforço considerável no início do projeto.

2. **Sobrecarga para Sistemas Simples:**
   - Em aplicações triviais, o DDD pode ser excessivamente robusto.

3. **Curva de Aprendizado:**
   - Implementar todos os conceitos do DDD pode ser desafiador para equipes sem experiência.

---

## Exemplo Prático com Node.js 🌐

### Diagrama de Comunicação

```mermaid
graph TD
    CONTROLLER[Controller]
    SERVICE[Serviço de Domínio]
    AGGREGATE[Agregado (Pedido)]
    REPOSITORY[Repositório]
    VALUE_OBJECT[Objeto de Valor (Endereço)]
    EVENT[Evento de Domínio]
    DB[Banco de Dados]

    CONTROLLER --> SERVICE
    SERVICE --> AGGREGATE
    AGGREGATE --> VALUE_OBJECT
    SERVICE --> EVENT
    SERVICE --> REPOSITORY
    REPOSITORY --> DB
```

### Código Exemplo

#### Entidades e Objetos de Valor

```javascript
// address.js (Value Object)
class Address {
  constructor(street, city, state, zip) {
    this.street = street;
    this.city = city;
    this.state = state;
    this.zip = zip;
  }
}

module.exports = Address;

// order.js (Entity)
class Order {
  constructor(id, customerId, address, items) {
    this.id = id;
    this.customerId = customerId;
    this.address = address; // Address é um Value Object
    this.items = items; // Lista de itens do pedido
  }

  addItem(item) {
    this.items.push(item);
  }

  calculateTotal() {
    return this.items.reduce((total, item) => total + item.price * item.quantity, 0);
  }
}

module.exports = Order;
```

#### Serviço de Domínio

```javascript
// orderService.js
const Order = require('./order');

class OrderService {
  constructor(orderRepository) {
    this.orderRepository = orderRepository;
  }

  async createOrder(customerId, address, items) {
    const order = new Order(null, customerId, address, items);
    const total = order.calculateTotal();

    if (total <= 0) {
      throw new Error('O total do pedido deve ser maior que zero.');
    }

    return await this.orderRepository.save(order);
  }
}

module.exports = OrderService;
```

#### Repositório

```javascript
// orderRepository.js
class OrderRepository {
  constructor(database) {
    this.database = database;
  }

  async save(order) {
    const result = await this.database.query('INSERT INTO orders SET ?', {
      customerId: order.customerId,
      address: JSON.stringify(order.address),
      items: JSON.stringify(order.items),
    });
    return { id: result.insertId, ...order };
  }
}

module.exports = OrderRepository;
```

#### Evento de Domínio

```javascript
// domainEvent.js
class DomainEvent {
  constructor(eventName, payload) {
    this.eventName = eventName;
    this.payload = payload;
    this.timestamp = new Date();
  }
}

module.exports = DomainEvent;
```

#### Controller

```javascript
// orderController.js
class OrderController {
  constructor(orderService) {
    this.orderService = orderService;
  }

  async handleCreateOrder(req, res) {
    try {
      const { customerId, address, items } = req.body;
      const order = await this.orderService.createOrder(customerId, address, items);
      res.status(201).json(order);
    } catch (error) {
      res.status(400).json({ error: error.message });
    }
  }
}

module.exports = OrderController;
```

#### Inicialização do Sistema

```javascript
// app.js
const express = require('express');
const bodyParser = require('body-parser');
const database = require('./database'); // Simulação de conexão com DB
const OrderRepository = require('./orderRepository');
const OrderService = require('./orderService');
const OrderController = require('./orderController');

const app = express();
app.use(bodyParser.json());

// Inicializando os componentes
const orderRepository = new OrderRepository(database);
const orderService = new OrderService(orderRepository);
const orderController = new OrderController(orderService);

// Rotas
app.post('/orders', (req, res) => orderController.handleCreateOrder(req, res));

// Iniciando o servidor
app.listen(3000, () => {
  console.log('Servidor rodando na porta 3000');
});
```

---

## Boas Práticas e Cuidados a Tomar 🛠️

### Boas Práticas
1. **Identifique os Contextos Delimitados:**
   - Separe o domínio em partes menores e coesas.
2. **Use Linguagem Ubíqua:**
   - Nomeie classes, métodos e variáveis de acordo com o domínio.
3. **Mantenha Agregados Simples:**
   - Não sobrecarregue entidades ou objetos de valor com responsabilidades desnecessárias.

### Cuidados
1. **Evite Misturar Contextos:**
   - Cada contexto delimitado deve ser independente.
2. **Não Generalize Demais:**
   - Mantenha o foco em resolver problemas reais do domínio.

---

## Conclusão 🎯

O Domain-Driven Design é uma abordagem poderosa para modelar sistemas complexos que refletem fielmente o domínio de negócio. Apesar de sua curva de aprendizado, os benefícios em termos de alinhamento, manutenção e evolução tornam o DDD indispensável em projetos de grande porte. 🚀
