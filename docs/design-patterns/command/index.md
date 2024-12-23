# Command Design Pattern 📜

O **Command**, ou Padrão de Comando, é um padrão de projeto comportamental que encapsula uma solicitação como um objeto, permitindo que você parametrize ações, enfileire ou registre solicitações, e implemente operações como desfazer (undo) e refazer (redo).

---

## Contexto Histórico 🕰️

O Command foi introduzido pelo **Gang of Four** no livro _"Design Patterns: Elements of Reusable Object-Oriented Software"_ (1994). Ele foi projetado para resolver problemas de **encapsulamento de solicitações**, permitindo que ações sejam tratadas de forma flexível e independente do remetente ou do destinatário.

---

## O que é o Command? 🤔

O Command separa o **emissor** (quem solicita a ação) do **executante** (quem realiza a ação), encapsulando a solicitação em um objeto. Isso permite que comandos sejam manipulados como objetos, armazenados, enfileirados ou transmitidos entre diferentes partes do sistema.

Imagine um controle remoto universal onde cada botão representa um comando que pode ser configurado, armazenado ou alterado dinamicamente.

---

## Quando Usar o Command? 🛠️

1. **Histórico de Ações:**  
   Quando você precisa implementar operações como desfazer e refazer.

2. **Parametrização de Solicitações:**  
   Quando comandos precisam ser configurados ou passados dinamicamente.

3. **Sistemas Enfileirados:**  
   Quando você precisa enfileirar ou agendar ações para execução posterior.

---

## Benefícios do Command 🌟

1. **Desacoplamento:**  
   O cliente que solicita a ação não precisa conhecer os detalhes da execução.

2. **Flexibilidade:**  
   Permite armazenar e manipular comandos como objetos.

3. **Reusabilidade:**  
   Comandos podem ser reutilizados em diferentes contextos.

---

## Desvantagens do Command 🚨

1. **Complexidade Adicional:**  
   Adicionar comandos pode tornar o sistema mais complexo.

2. **Sobrecarga de Classes:**  
   Cada comando geralmente requer sua própria classe, aumentando o número de arquivos.

---

## Exemplo Prático: Controle Remoto Universal 📺

Vamos criar um exemplo onde usamos o padrão Command para controlar dispositivos eletrônicos com um controle remoto.

---

### Implementação

#### **Interface do Comando**

```javascript
// Command.js
class Command {
  execute() {
    throw new Error("Método 'execute()' deve ser implementado.");
  }

  undo() {
    throw new Error("Método 'undo()' deve ser implementado.");
  }
}

module.exports = Command;
```

---

#### **Comandos Concretos**

```javascript
// LightOnCommand.js
const Command = require('./Command');

class LightOnCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.turnOn();
  }

  undo() {
    this.light.turnOff();
  }
}

module.exports = LightOnCommand;

// LightOffCommand.js
const Command = require('./Command');

class LightOffCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.turnOff();
  }

  undo() {
    this.light.turnOn();
  }
}

module.exports = LightOffCommand;
```

---

#### **Receptor**

```javascript
// Light.js
class Light {
  turnOn() {
    console.log('Luz ligada.');
  }

  turnOff() {
    console.log('Luz desligada.');
  }
}

module.exports = Light;
```

---

#### **Invoker (Controle Remoto)**

```javascript
// RemoteControl.js
class RemoteControl {
  constructor() {
    this.commandHistory = [];
  }

  setCommand(command) {
    this.command = command;
  }

  pressButton() {
    if (this.command) {
      this.command.execute();
      this.commandHistory.push(this.command);
    }
  }

  pressUndo() {
    const command = this.commandHistory.pop();
    if (command) {
      command.undo();
    }
  }
}

module.exports = RemoteControl;
```

---

#### **Uso do Command**

```javascript
const Light = require('./Light');
const LightOnCommand = require('./LightOnCommand');
const LightOffCommand = require('./LightOffCommand');
const RemoteControl = require('./RemoteControl');

// Criando os dispositivos e comandos
const livingRoomLight = new Light();
const lightOn = new LightOnCommand(livingRoomLight);
const lightOff = new LightOffCommand(livingRoomLight);

// Configurando o controle remoto
const remote = new RemoteControl();
remote.setCommand(lightOn);

// Ligando a luz
remote.pressButton(); // Saída: Luz ligada.

// Desfazendo a ação
remote.pressUndo(); // Saída: Luz desligada.

// Configurando para desligar a luz
remote.setCommand(lightOff);
remote.pressButton(); // Saída: Luz desligada.
```

---

## Quando Evitar o Command? ❌

- **Sistemas Simples:**  
  Se o sistema não exige histórico de ações ou parametrização, o Command pode ser excessivo.

- **Baixa Frequência de Mudanças:**  
  Se os comandos raramente mudam, uma abordagem mais direta pode ser suficiente.

---

## Alternativas ao Command 🌍

1. **Strategy:**  
   Use quando precisar encapsular diferentes algoritmos ou comportamentos.

2. **Observer:**  
   Ideal para notificar múltiplos objetos sobre uma ação ou alteração de estado.

---

## Conclusão 🎯

O Command é um padrão poderoso para sistemas que precisam de **flexibilidade na execução de ações**, promovendo desacoplamento e permitindo funcionalidades como desfazer, refazer e enfileiramento. Embora adicione complexidade, ele é indispensável em sistemas complexos que exigem controle robusto sobre comandos.

Se usado corretamente, o Command pode transformar sistemas rígidos em soluções dinâmicas e configuráveis! 🚀

---

**Quer explorar outro padrão? Escolha o próximo para "detonarmos"! 😉**
