#Exercice 1: serveur node js

#Journalisation d'un site web (avec deux routes : home et help sur un serveur node js), sur un fichier .txt, à chaque visite du client.

#Resolution
# J'ai Commencer par mettre à jour les paquets existants en utilisant yum :
sudo yum update -y
#Vous pouvez installer Node.js en utilisant le gestionnaire de paquets NodeSource. Tout d'abord, ajoutez le dépôt NodeSource à votre système :
curl -fsSL https://rpm.nodesource.com/setup_16.x | sudo bash -
#Ensuite, installez Node.js :
sudo yum install -y nodejs
# Pour Vérifier que Node.js et npm sont installés correctement en exécutant les commandes suivantes :
node -v
npm -v

#Créer un projet Node.js
#creation d'un dossier:
mkdir my-website
cd my-website  # changeons de repectoire
npm init -y 

#Installez les modules express pour créer le serveur web et fs pour la gestion des fichiers :
npm install express

#Créez un fichier server.js et ajoutez le code suivant :

nano server.js
const express = require('express');
const fs = require('fs');
const path = require('path');

const app = express();
const port = 3000;

// Middleware pour journaliser les visites
app.use((req, res, next) => {
  const log = `${new Date().toISOString()} - ${req.method} ${req.url}\n`;
  fs.appendFile(path.join(__dirname, 'visits.log'), log, (err) => {
    if (err) {
      console.error('Failed to write log:', err);
    }
  });
  next();
});

// Route home
app.get('/', (req, res) => {
  res.send('Welcome to the Home Page!');
});

// Route help
app.get('/help', (req, res) => {
  res.send('Welcome to the Help Page!');
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}/`);
});

#Exécutez le serveur avec la commande suivante :

node server.js
# pour la journalisation:j'ai utilise une bibliothèque de journalisation winston :

npm install winston



# demander la page dans le port 3001
curl http://localhost:3001/
