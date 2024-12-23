# Composite Design Pattern 🌳

O **Composite**, ou Composto, é um padrão de projeto estrutural que permite tratar **objetos individuais e composições de objetos de forma uniforme**. Ele é ideal para modelar estruturas hierárquicas, como árvores, em que elementos individuais e conjuntos de elementos precisam ser manipulados de maneira consistente.

---

## Contexto Histórico 🕰️

O Composite foi introduzido pelo **Gang of Four** no livro _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994). Ele foi projetado para resolver problemas de estruturas hierárquicas, como sistemas de arquivos, árvores de componentes gráficos ou menus.

---

## O que é o Composite? 🤔

O Composite permite organizar objetos em uma **estrutura de árvore**. Essa estrutura pode conter objetos simples (folhas) ou composições de objetos (nós). Ambos são tratados de forma consistente, graças a uma interface comum.

---

## Quando Usar o Composite? 🛠️

1. **Sistemas Hierárquicos:**  
   Quando você precisa representar hierarquias de objetos, como sistemas de arquivos, árvores ou menus.

2. **Tratamento Uniforme:**  
   Quando objetos simples e compostos precisam ser manipulados de maneira uniforme.

3. **Flexibilidade em Estruturas Recursivas:**  
   Quando você precisa permitir que composições de objetos sejam recursivamente aninhadas.

---

## Benefícios do Composite 🌟

1. **Facilidade de Uso:**  
   Permite manipular objetos individuais e composições de forma uniforme.

2. **Flexibilidade:**  
   Facilita a construção e modificação de estruturas complexas.

3. **Redução de Código Duplicado:**  
   Elimina a necessidade de tratar objetos simples e compostos separadamente.

---

## Desvantagens do Composite 🚨

1. **Complexidade Adicional:**  
   A implementação de uma estrutura hierárquica pode ser mais complexa do que manipular objetos simples.

2. **Validação Complicada:**  
   Validar composições complexas pode ser desafiador.

---

## Exemplo Prático: Sistema de Arquivos 🗂️

Vamos criar um exemplo onde representamos um **sistema de arquivos** com arquivos e diretórios.

---

### Implementação

#### **Interface Comum**

```javascript
// FileSystemItem.js
class FileSystemItem {
  getName() {
    throw new Error("Método 'getName()' deve ser implementado.");
  }

  display(indent = 0) {
    throw new Error("Método 'display()' deve ser implementado.");
  }
}

module.exports = FileSystemItem;
```

---

#### **Classe de Arquivo (Folha)**

```javascript
// File.js
const FileSystemItem = require('./FileSystemItem');

class File extends FileSystemItem {
  constructor(name) {
    super();
    this.name = name;
  }

  getName() {
    return this.name;
  }

  display(indent = 0) {
    console.log(`${' '.repeat(indent)}📄 ${this.name}`);
  }
}

module.exports = File;
```

---

#### **Classe de Diretório (Composto)**

```javascript
// Directory.js
const FileSystemItem = require('./FileSystemItem');

class Directory extends FileSystemItem {
  constructor(name) {
    super();
    this.name = name;
    this.children = [];
  }

  add(item) {
    this.children.push(item);
  }

  getName() {
    return this.name;
  }

  display(indent = 0) {
    console.log(`${' '.repeat(indent)}📁 ${this.name}`);
    for (const child of this.children) {
      child.display(indent + 2);
    }
  }
}

module.exports = Directory;
```

---

#### **Uso do Composite**

```javascript
const File = require('./File');
const Directory = require('./Directory');

// Criando arquivos
const file1 = new File('arquivo1.txt');
const file2 = new File('arquivo2.txt');
const file3 = new File('relatorio.pdf');

// Criando diretórios
const dir1 = new Directory('Documentos');
const dir2 = new Directory('Imagens');
const root = new Directory('Raiz');

// Montando a estrutura
dir1.add(file1);
dir1.add(file2);
dir2.add(file3);
root.add(dir1);
root.add(dir2);

// Exibindo a estrutura
root.display();
```

---

#### **Saída Esperada**

```
📁 Raiz
  📁 Documentos
    📄 arquivo1.txt
    📄 arquivo2.txt
  📁 Imagens
    📄 relatorio.pdf
```

---

## Quando Evitar o Composite? ❌

- **Estruturas Simples:**  
  Se você não precisa de uma hierarquia complexa, o Composite pode ser excessivo.

- **Baixa Frequência de Alterações:**  
  Se a estrutura hierárquica raramente muda, o esforço para implementar o Composite pode não valer a pena.

---

## Alternativas ao Composite 🌍

1. **Decorator:**  
   Use quando precisar adicionar funcionalidades a objetos individuais.

2. **Chain of Responsibility:**  
   Ideal para processamento sequencial de objetos sem hierarquia.

---

## Conclusão 🎯

O Composite é um padrão essencial para **estruturas hierárquicas complexas**, permitindo que objetos simples e compostos sejam tratados de forma uniforme. Ele promove organização e flexibilidade, mas deve ser usado com cautela em sistemas simples.

Se aplicado corretamente, o Composite pode transformar a maneira como você modela estruturas de dados complexas no seu sistema! 🚀

---

**Pronto para explorar outro padrão? Escolha o próximo para "detonarmos"! 😉**
