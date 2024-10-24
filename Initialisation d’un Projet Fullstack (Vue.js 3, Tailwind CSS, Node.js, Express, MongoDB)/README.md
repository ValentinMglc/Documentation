## Documentation Complète : Initialisation d’un Projet Fullstack (Vue.js 3, Tailwind CSS, Node.js, Express, MongoDB)
> [!NOTE]
> Cette documentation détaille le processus d'initialisation et de structuration d'un projet Fullstack avec un frontend en **Vue.js 3** et **Tailwind CSS**, et un backend en **Node.js** avec **Express** et **MongoDB**.

## Table des Matières :
1. Initialisation du projet frontend (Vue.js + Tailwind CSS)
2. Structure détaillée du projet frontend
3. Explications des dossiers et fichiers frontend
   
5. Initialisation du projet backend (Node.js + Express + MongoDB)
6. Structure détaillée du projet backend
7. Explications des dossiers et fichiers backend

## 1. Initialisation du projet frontend
Le projet frontend sera basé sur **Vue.js 3** et **Tailwind CSS**. Nous allons utiliser **Vite.js** pour sa rapidité et sa simplicité.

### **Étape 1 : Créer le projet Vue.js**

Exécutez les commandes suivantes pour initialiser un projet Vue.js avec Vite.js :
```
npm init vite@latest frontend
cd frontend
npm install
```
Cette commande va créer un projet Vue.js dans le dossier frontend. Vous pouvez choisir "Vue" comme framework lorsque Vite vous le demande.

### **Étape 2 : Installer Tailwind CSS**

Tailwind CSS est un framework CSS utilitaire qui permet de styliser les composants rapidement. Installez Tailwind CSS dans le projet :
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```
Cela va générer un fichier `tailwind.config.js` dans la racine de votre projet.

### **Étape 3 : Configurer Tailwind CSS**

Modifiez `tailwind.config.js` pour ajouter les chemins des fichiers à surveiller (pour l'utilisation de Tailwind) :
```
module.exports = {
  content: [
    './index.html',
    './src/**/*.{vue,js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```
Ensuite, créez ou modifiez un fichier CSS dans `src/assets/tailwind.css` pour inclure Tailwind :
```
/* src/assets/tailwind.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```
Enfin, importez ce fichier dans `main.js` :
```
import './assets/tailwind.css';
```
### **Étape 4 : Démarrer le serveur de développement**

Lancez le serveur de développement pour voir votre application Vue.js avec Tailwind CSS :
```
npm run dev
```
Votre projet frontend est maintenant prêt à être développé avec Vue.js et Tailwind CSS.

## 2. Structure détaillée du projet frontend
Voici la structure du projet frontend avec Vue.js 3 et Tailwind CSS :
```
frontend/
├── public/
│   └── index.html             # Fichier HTML principal
├── src/
│   ├── assets/                # Fichiers statiques (images, styles)
│   │   └── tailwind.css       # Fichier CSS principal avec Tailwind
│   ├── components/            # Composants Vue.js réutilisables
│   │   └── Navbar.vue         # Exemple de composant Navbar
│   ├── layouts/               # Layouts globaux (ex. Header, Footer)
│   │   └── DefaultLayout.vue  # Exemple de layout par défaut
│   ├── pages/                 # Pages Vue.js
│   │   └── Home.vue           # Page d'accueil
│   ├── router/                # Configuration du routeur Vue.js
│   │   └── index.js           # Fichier de configuration du routeur
│   ├── store/                 # Gestion de l'état global (Vuex ou Pinia)
│   │   └── index.js           # Fichier de configuration du store
│   ├── App.vue                # Composant racine de l'application
│   ├── main.js                # Point d'entrée principal pour Vue.js
│   └── vite.config.js         # Fichier de configuration de Vite.js
├── .gitignore                 # Fichiers à ignorer par Git
├── package.json               # Fichier de gestion des dépendances npm
├── tailwind.config.js         # Configuration de Tailwind CSS
├── postcss.config.js          # Configuration de PostCSS
└── node_modules/              # Dépendances npm
```

## 3. Explications des dossiers et fichiers frontend
1. `public/` : Contient des fichiers statiques et le fichier HTML principal.
2. `src/assets/` : Contient les fichiers statiques comme les styles ou les images.
3. `src/components/` : Contient les composants Vue.js réutilisables dans l'application.
4. `src/layouts/` : Contient les layouts utilisés pour structurer les pages.
5. `src/pages/` : Contient les composants représentant des pages entières.
6. `src/router/` : Contient la configuration du routeur Vue.js pour la navigation entre les pages.
7. `src/store/` : Contient la configuration de la gestion d'état globale (Vuex ou Pinia).
8. `App.vue` : Composant racine de l'application.
9. `main.js` : Point d'entrée principal pour l'application Vue.js.

## 4. Initialisation du projet backend
Le backend sera développé avec **Node.js**, **Express**, et **MongoDB**.

### **Étape 1 : Créer le projet Node.js**
Créez un nouveau dossier pour le backend, initialisez un projet Node.js et installez les dépendances :
```
mkdir backend
cd backend
npm init -y
npm install express mongoose dotenv
```
### **Étape 2 : Configurer l'environnement**
Créez un fichier `.env` à la racine de votre projet backend pour y placer les configurations sensibles (comme les URLs et les clés API) :
```
touch .env
```
Ajoutez les variables d'environnement suivantes :
```
MONGO_URI=mongodb://localhost:27017/votre_base_de_donnees
PORT=5000
```

### **Étape 3 : Connecter à MongoDB**
Créez un fichier de configuration `config/db.js` pour connecter votre backend à MongoDB via Mongoose :
```
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGO_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });
    console.log(`MongoDB Connected: ${conn.connection.host}`);
  } catch (error) {
    console.error(`Error: ${error.message}`);
    process.exit(1); // Arrêter le serveur si la connexion échoue
  }
};

module.exports = connectDB;
```

### **Étape 4 : Créer le serveur Express**
Dans le fichier `server.js`, initialisez le serveur Express et configurez la connexion à la base de données MongoDB :
```
const express = require('express');
const dotenv = require('dotenv');
const connectDB = require('./config/db');

dotenv.config();  // Charger les variables d'environnement
connectDB();      // Connexion à MongoDB

const app = express();
app.use(express.json());  // Parser le JSON

// Routes
app.get('/', (req, res) => {
  res.send('API is running...');
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```
Démarrez votre serveur avec :
```
node server.js
```

## 5. Structure du projet backend
Voici la structure du dossier backend :
```
backend/
├── config/
│   └── db.js                  # Configuration de la connexion MongoDB
├── controllers/
│   └── userController.js      # Contrôleurs pour la logique métier des utilisateurs
├── middlewares/
│   └── authMiddleware.js      # Middleware pour la gestion de l'authentification
├── models/
│   └── User.js                # Modèle utilisateur avec Mongoose
├── routes/
│   └── userRoutes.js          # Fichier de définition des routes utilisateurs
├── services/
│   └── userService.js         # Services pour encapsuler la logique de traitement des données
├── .env                       # Variables d'environnement (comme l'URL de MongoDB)
├── server.js                  # Fichier principal pour démarrer le serveur Express
├── package.json               # Fichier de gestion des dépendances npm
├── package-lock.json          # Fichier généré pour le verrouillage des versions
└── node_modules/              # Dépendances Node.js (généré par npm)
```

## 6. Explications des dossiers backend
1. `config/db.js` : Contient la logique de connexion à la base de données MongoDB.
2. `controllers/` : Contient les fichiers de contrôleurs qui gèrent la logique métier et répondent aux requêtes HTTP.
3. `middlewares/` : Contient les middlewares comme la gestion de l'authentification.
4. `models/` : Contient les schémas de données utilisés pour interagir avec MongoDB via Mongoose.
5. `routes/` : Définit les routes qui mappent les requêtes HTTP aux contrôleurs.
6. `services/` : Contient la logique métier qui interagit avec la base de données ou des services externes.
7. `.env` : Fichier de configuration des variables d'environnement.
8. `server.js` : Point d'entrée principal du serveur Express.
