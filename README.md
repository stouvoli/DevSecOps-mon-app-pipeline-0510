# Application de Test DevSecOps : mon-app-pipeline

Cette application est une simple application web Node.js et Express utilisée comme support pour les ateliers de la formation "Réussir la Certification DevSecOps".

## Prérequis

* [Node.js](https://nodejs.org/) (v18+)
* [Docker Desktop](https://www.docker.com/products/docker-desktop/)

## Installation

1.  Clonez le dépôt (si ce n'est pas déjà fait).
2.  Naviguez dans le dossier du projet :
    ```sh
    cd mon-app-pipeline
    ```
3.  Installez les dépendances Node.js :
    ```sh
    npm install
    ```

## Utilisation

### Avec Docker (Méthode recommandée)

1.  **Construire l'image Docker :**
    ```sh
    docker build -t mon-app-pipeline .
    ```
2.  **Lancer le conteneur :**
    ```sh
    docker run -p 3000:3000 mon-app-pipeline
    ```

### Localement (pour le développement)

1.  **Démarrer le serveur :**
    ```sh
    node server.js
    ```

Une fois l'application démarrée, elle est accessible à l'adresse [http://localhost:3000](http://localhost:3000).