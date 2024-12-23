# Proxy Design Pattern 🛡️

O **Proxy**, ou Proxy Pattern, é um padrão de projeto estrutural que fornece um **substituto ou representante** para outro objeto. Ele age como uma interface para o objeto real, controlando o acesso a ele. Isso permite adicionar funcionalidades como controle de acesso, caching e lazy initialization sem modificar o objeto original.

---

## Contexto Histórico 🕰️

O Proxy foi descrito pelo **Gang of Four** no livro _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994). Ele surgiu da necessidade de mediar o acesso a objetos complexos, protegidos ou remotos, especialmente em sistemas distribuídos e de alto desempenho.

---

## O que é o Proxy? 🤔

O Proxy é um objeto que controla o acesso a outro objeto, chamado de **real subject** (objeto real). Ele pode fazer isso por várias razões, como:

1. **Controle de Acesso:** Garantir que apenas usuários ou processos autorizados possam acessar o objeto real.
2. **Melhoria de Desempenho:** Usar cache ou lazy loading para reduzir operações dispendiosas.
3. **Gerenciamento de Recursos:** Proteger objetos caros ou limitados, como conexões de rede ou arquivos.

---

## Tipos Comuns de Proxy 🔍

1. **Virtual Proxy:**  
   Usado para atrasar a criação e inicialização de objetos caros até que sejam realmente necessários.

2. **Remote Proxy:**  
   Representa um objeto em outro espaço de memória, como em sistemas distribuídos.

3. **Protection Proxy:**  
   Controla o acesso ao objeto real, garantindo que apenas usuários ou processos autorizados possam interagir com ele.

4. **Cache Proxy:**  
   Armazena resultados de operações caras para melhorar o desempenho.

---

## Quando Usar o Proxy? 🛠️

1. **Objetos Caros ou Complexos:**  
   Quando você deseja evitar a criação ou carregamento de objetos até que eles sejam necessários.

2. **Controle de Acesso:**  
   Quando o acesso a objetos precisa ser restrito.

3. **Sistemas Distribuídos:**  
   Quando você precisa acessar objetos em locais remotos, como servidores ou APIs externas.

---

## Benefícios do Proxy 🌟

1. **Flexibilidade:**  
   Permite adicionar funcionalidades sem modificar o objeto real.

2. **Desempenho Melhorado:**  
   Reduz o uso de recursos ao atrasar ou evitar operações dispendiosas.

3. **Segurança:**  
   Facilita o controle de acesso e a proteção de objetos sensíveis.

---

## Desvantagens do Proxy 🚨

1. **Complexidade Adicional:**  
   Introduz uma camada extra de abstração, que pode complicar o sistema.

2. **Latência Potencial:**  
   Em alguns casos, o Proxy pode adicionar latência desnecessária.

---

## Exemplo Prático: Sistema de Imagens 📷

Vamos criar um exemplo onde o Proxy controla o carregamento de imagens grandes em um editor gráfico, usando o **Virtual Proxy**.

---

### Implementação

#### **Interface Comum**

```javascript
// Image.js
class Image {
  display() {
    throw new Error("Método 'display()' deve ser implementado.");
  }
}

module.exports = Image;
```

---

#### **Classe Real**

```javascript
// RealImage.js
const Image = require('./Image');

class RealImage extends Image {
  constructor(filename) {
    super();
    this.filename = filename;
    this.loadFromDisk();
  }

  loadFromDisk() {
    console.log(`Carregando imagem de ${this.filename}`);
  }

  display() {
    console.log(`Exibindo ${this.filename}`);
  }
}

module.exports = RealImage;
```

---

#### **Classe Proxy**

```javascript
// ProxyImage.js
const Image = require('./Image');
const RealImage = require('./RealImage');

class ProxyImage extends Image {
  constructor(filename) {
    super();
    this.filename = filename;
    this.realImage = null;
  }

  display() {
    if (!this.realImage) {
      this.realImage = new RealImage(this.filename);
    }
    this.realImage.display();
  }
}

module.exports = ProxyImage;
```

---

#### **Uso do Proxy**

```javascript
const ProxyImage = require('./ProxyImage');

// Criando uma imagem via Proxy
const image = new ProxyImage('foto.png');

// A imagem é carregada apenas quando exibida pela primeira vez
image.display(); // Saída: Carregando imagem de foto.png
                 //         Exibindo foto.png

// Exibindo novamente, sem recarregar
image.display(); // Saída: Exibindo foto.png
```

---

## Quando Evitar o Proxy? ❌

- **Sistemas Simples:**  
  Se o sistema não exige controle de acesso ou otimização de recursos, o Proxy pode ser excessivo.

- **Overhead Adicional:**  
  Quando a camada extra do Proxy introduz complexidade desnecessária.

---

## Alternativas ao Proxy 🌍

1. **Decorator:**  
   Use quando quiser adicionar comportamentos sem alterar o objeto original.

2. **Adapter:**  
   Ideal para traduzir interfaces incompatíveis, mas sem controle de acesso.

---

## Conclusão 🎯

O Proxy é um padrão poderoso para **controlar o acesso** a objetos reais e otimizar o uso de recursos. Ele é especialmente útil em sistemas que lidam com objetos caros, protegidos ou distribuídos. Embora introduza uma camada extra de complexidade, o Proxy pode trazer benefícios significativos em segurança, desempenho e flexibilidade.

Se usado corretamente, o Proxy pode transformar sistemas complexos em soluções eficientes e seguras! 🚀

---

**Pronto para explorar outro padrão? Diga-me qual você quer "detonar" a seguir! 😉**
