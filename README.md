# TP4 : Introduction à gRPC

Ce projet montre comment mettre en place un serveur gRPC simple en Node.js. Il expose un service `Greeter` qui répond à un nom fourni avec un message de salutation personnalisé.

---

## 📁 Fichiers

- `server.js` : le serveur gRPC.
- `hello.proto` : la définition du service gRPC (fichier Protobuf).

---

## 🧰 Technologies utilisées

- Node.js
- gRPC
- Protocol Buffers (Protobuf)
- Modules Node :
  - `@grpc/grpc-js`
  - `@grpc/proto-loader`
---

## ▶️ Lancement du serveur

1. Clonez ce dépôt sur votre machine locale :

```bash
git clone https://github.com/MedRiadh2001/TP4_SOA.git
```

2. Installez les dépendances :

```bash
npm install
```

3. Démarrez le serveur :

```bash
node server.js
```

Le serveur démarrera sur l'adresse 0.0.0.0:50051 et affichera dans la console :

```bash
Serveur gRPC démarré sur 0.0.0.0:50051
```

## 📄 Explication du fonctionnement

### 1. Fichier (`hello.proto`) 

```js
syntax = "proto3";
package hello;
service Greeter {
    rpc SayHello (HelloRequest) returns (HelloReply) {}
}
message HelloRequest {
    string name = 1;
}
message HelloReply {
    string message = 1;
}
```

Ce fichier définit un service gRPC appelé  **`Greeter`** avec une seule méthode :

```js
rpc SayHello (HelloRequest) returns (HelloReply);
```
 - **`HelloRequest`** : message contenant un champ **`name`** (chaîne de caractères).
 - **`HelloReply`** : message contenant un champ **`message`** (la réponse personnalisée).

### 2. Fichier (`server.js`)

```js
const grpc = require("@grpc/grpc-js");
const protoLoader = require("@grpc/proto-loader");
const path = require("path");
const PROTO_PATH = path.join(__dirname, "hello.proto");

const packageDefinition = protoLoader.loadSync(PROTO_PATH, {
  keepCase: true,
  longs: String,
  enums: String,
  defaults: true,
  oneofs: true,
});

const helloProto = grpc.loadPackageDefinition(packageDefinition).hello;

function sayHello(call, callback) {
  const { name } = call.request;
  const reply = { message: `Bonjour, ${name} !` };
  callback(null, reply);
}

function main() {
  const server = new grpc.Server();
  server.addService(helloProto.Greeter.service, {
    SayHello: sayHello,
  });
  const port = "0.0.0.0:50051";
  server.bindAsync(port, grpc.ServerCredentials.createInsecure(), () => {
    console.log(`Serveur gRPC démarré sur ${port}`);
  });
}

main();
```

Voici les grandes étapes du serveur :

 - Chargement du fichier **`.proto`** avec **`protoLoader`**.
 - Création du service **`Greeter`** et définition de l’implémentation de la méthode **`SayHello`**.
 - L'implémentation extrait le champ **`name`** et renvoie une réponse du type : **`Bonjour, nom !`**
 - Le serveur est lancé sur le port **`50051`** via **`server.bindAsync`**.

### 3. Exemple

Si un client envoie une requête avec :

```json
{
    "name": "TestUser"
}
```

Le serveur renverra :

```json
{
    "message": "Bonjour, TestUser !"
}
```

!(cap_ecran/cap.png)


## Auteur

- **Mohamed Riadh Essridi**
- Classe : 4GL1
