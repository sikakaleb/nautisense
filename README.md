
# Nautisense

Nautisense est une application Vue.js basée sur Vue 2 et Vue CLI 4, offrant une interface utilisateur interactive et optimisée.

##  Prérequis

Avant de commencer, assure-toi d'avoir les éléments suivants installés sur ton système :

- [Node.js](https://nodejs.org/) (Version recommandée : **16.20.2**)
- [npm](https://www.npmjs.com/) (Inclus avec Node.js)
- [Vue CLI](https://cli.vuejs.org/) (**4.x** recommandé)

###  Vérifier les versions installées :
```sh
node -v
npm -v
vue --version
```

##  Installation

Clone le dépôt GitHub et installe les dépendances :

```sh
git clone https://github.com/sikakaleb/nautisense.git
cd nautisense
npm install
```

##  Lancer le projet en mode développement

```sh
npm run serve
```

Ensuite, ouvre ton navigateur et accède à :

```
http://localhost:8080/
```

## 🔧 Commandes utiles

| Commande | Description |
|----------|------------|
| `npm run serve` | Démarre le serveur de développement |
| `npm run build` | Génère la version de production |
| `npm run lint` | Vérifie et corrige les erreurs de linting |

##  Déploiement

Une fois ton projet prêt pour la production, génère les fichiers optimisés avec :

```sh
npm run build
```

Les fichiers seront générés dans le dossier **`dist/`**, prêt à être déployé sur un serveur ou un service comme **Vercel, Netlify, Firebase Hosting, ou un hébergement classique**.

##  Google Maps API

Si tu utilises Google Maps, assure-toi d'ajouter une **clé API valide** dans ton projet.

🔧 **Modifie** `API_KEY.js` :
```js
export const API_KEY = 'TA_CLE_API_ICI';
```
**Générer une clé API** 👉 [Google Cloud Console](https://console.cloud.google.com/)

## 🛠️ Technologies utilisées

- **Vue.js 2.7.14**
- **Vue CLI 4.5.13**
- **Bootstrap 4.6**
- **Google Maps API**
- **Sass**


---



