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

## 📦 Installation

```bash
npm install @grpc/grpc-js @grpc/proto-loader
```

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

Ce fichier définit un service gRPC appelé  **`Greeter`** avec une seule méthode :

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

```js
rpc SayHello (HelloRequest) returns (HelloReply);
```
 - **`HelloRequest`** : message contenant un champ **`name`** (chaîne de caractères).
 - **`HelloReply`** : message contenant un champ **`message`** (la réponse personnalisée).

### 2. Fichier (`server.js`)
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

Le serveur renverra :1

```json
{
    "message": "Bonjour, TestUser !"
}
```

!(cap ecran\cap.png)


## Auteur

- **Mohamed Riadh Essridi**
- Classe : 4GL1
