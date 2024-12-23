# Microservices Architecture (Arquitetura de Microsserviços) 🧩

A **Microservices Architecture**, ou Arquitetura de Microsserviços, é um padrão arquitetural onde o sistema é dividido em **serviços pequenos e independentes**, que trabalham juntos para formar uma aplicação maior. Cada microsserviço é implementado de forma independente, possui um propósito único e pode ser implantado separadamente.

---

## Como funciona a Arquitetura de Microsserviços? 🌐

Ao invés de construir um sistema monolítico onde todos os componentes estão interligados, a arquitetura de microsserviços separa o sistema em **módulos menores** que comunicam entre si por meio de APIs. Esses microsserviços são autônomos e frequentemente organizados em torno de **capacidades de negócio**.

### Diagrama Contextual 📊

```mermaid
graph TD
    CLIENTE[Cliente (App/Web)]
    AUTH[Microsserviço de Autenticação]
    ORDER[Microsserviço de Pedidos]
    INVENTORY[Microsserviço de Estoque]
    PAYMENT[Microsserviço de Pagamentos]
    DB_AUTH[(Banco Auth)]
    DB_ORDER[(Banco Orders)]
    DB_INVENTORY[(Banco Inventory)]
    DB_PAYMENT[(Banco Payment)]

    CLIENTE --> AUTH
    CLIENTE --> ORDER
    ORDER --> INVENTORY
    ORDER --> PAYMENT
    AUTH --> DB_AUTH
    ORDER --> DB_ORDER
    INVENTORY --> DB_INVENTORY
    PAYMENT --> DB_PAYMENT
```

Cada microsserviço gerencia sua própria lógica de negócio e dados, garantindo desacoplamento e independência.

---

## Vantagens e Desvantagens da Arquitetura de Microsserviços

### Vantagens 🌟

1. **Escalabilidade Independente:**
   - Cada microsserviço pode ser escalado de forma isolada, otimizando recursos.

2. **Desenvolvimento Modular:**
   - Equipes podem trabalhar em diferentes microsserviços simultaneamente.

3. **Facilidade de Implantação:**
   - Atualizações podem ser feitas em um microsserviço sem impactar o restante do sistema.

4. **Resiliência:**
   - Falhas em um microsserviço não necessariamente comprometem todo o sistema.

5. **Flexibilidade Tecnológica:**
   - Cada microsserviço pode ser implementado com tecnologias diferentes.

### Desvantagens ❌

1. **Complexidade Operacional:**
   - Gerenciar muitos microsserviços exige uma infraestrutura mais sofisticada.

2. **Sobrecarga de Comunicação:**
   - A comunicação entre serviços pode introduzir latência.

3. **Testes e Debugging:**
   - Testar um sistema distribuído é mais complexo do que um monolito.

4. **Consistência de Dados:**
   - Garantir transações consistentes entre serviços pode ser desafiador.

---

## Exemplo Prático com Node.js 🌐

Neste exemplo, construiremos um sistema simples com três microsserviços: **Autenticação**, **Pedidos** e **Estoque**.

### Microsserviço de Autenticação

#### Código Exemplo

```javascript
// authService.js
const express = require('express');
const app = express();

app.use(express.json());

const users = [
  { id: 1, username: 'user1', password: 'password1' },
  { id: 2, username: 'user2', password: 'password2' },
];

app.post('/login', (req, res) => {
  const { username, password } = req.body;
  const user = users.find((u) => u.username === username && u.password === password);

  if (user) {
    res.status(200).json({ message: 'Login successful', userId: user.id });
  } else {
    res.status(401).json({ error: 'Invalid credentials' });
  }
});

app.listen(4000, () => {
  console.log('Auth Service running on port 4000');
});
```

---

### Microsserviço de Pedidos

#### Código Exemplo

```javascript
// orderService.js
const express = require('express');
const app = express();

app.use(express.json());

const orders = [];

app.post('/orders', (req, res) => {
  const { userId, items } = req.body;
  const order = { id: orders.length + 1, userId, items, status: 'pending' };
  orders.push(order);
  res.status(201).json(order);
});

app.get('/orders', (req, res) => {
  res.status(200).json(orders);
});

app.listen(5000, () => {
  console.log('Order Service running on port 5000');
});
```

---

### Microsserviço de Estoque

#### Código Exemplo

```javascript
// inventoryService.js
const express = require('express');
const app = express();

app.use(express.json());

const inventory = [
  { id: 1, product: 'Laptop', quantity: 50 },
  { id: 2, product: 'Mouse', quantity: 150 },
];

app.get('/inventory', (req, res) => {
  res.status(200).json(inventory);
});

app.post('/inventory/reduce', (req, res) => {
  const { productId, quantity } = req.body;
  const product = inventory.find((item) => item.id === productId);

  if (product && product.quantity >= quantity) {
    product.quantity -= quantity;
    res.status(200).json({ message: 'Inventory updated', product });
  } else {
    res.status(400).json({ error: 'Insufficient stock or product not found' });
  }
});

app.listen(6000, () => {
  console.log('Inventory Service running on port 6000');
});
```

---

### Comunicação entre Microsserviços

Os microsserviços se comunicam por APIs REST. Por exemplo, o microsserviço de Pedidos pode verificar o estoque antes de criar um pedido.

---

## Boas Práticas e Cuidados a Tomar 🛠️

### Boas Práticas
1. **Desacoplamento Completo:**
   - Evite dependências diretas entre microsserviços.
2. **Automatize a Infraestrutura:**
   - Use ferramentas como Kubernetes ou Docker para gerenciar serviços.
3. **Monitoramento e Logs:**
   - Implemente monitoramento centralizado para identificar falhas.

### Cuidados
1. **Gerencie a Comunicação:**
   - Utilize filas ou eventos assíncronos para evitar dependências de tempo real.
2. **Tenha Estratégias de Recuperação:**
   - Configure retries e circuit breakers para lidar com falhas temporárias.

---

## Conclusão 🎯

A Arquitetura de Microsserviços é ideal para sistemas escaláveis, flexíveis e resilientes. Apesar de sua complexidade operacional, oferece enormes benefícios para equipes que precisam de autonomia e alta disponibilidade. Adote com cuidado e veja sua aplicação se transformar! 🚀
