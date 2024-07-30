#Exercice 3: serveur apache

#Journalisation d'un site web (avec deux pages : index.php et help.php sur un serveur apache), sur un fichier .txt, à chaque visite du client.
# Resolution
# J ai commencé par l'installation d'Apache
#Mettre à jour la liste des paquets

sudo yum update -y
#Installer Apache (HTTPD): Installez le serveur web Apache
sudo yum install -y httpd
#Démarrez le service Apache 
sudo systemctl start httpd
#Activez Apache pour qu'il démarre automatiquement au démarrage du système
sudo systemctl enable httpd
# Creer un dosssier var et changer de repectoire, dans le dossier var creer un dossier www en suite changer de repectoire et dans www creer un dossier html
mkdir var
cd var
mkdir www
cd www
mkdir html
cd html Dans html creer un fichier index.php et help.php

sudo nano index.php
<?php
$file = '/var/www/html/visits.txt';
$log = date('Y-m-d H:i:s') . " - Visited index.php\n";
file_put_contents($file, $log, FILE_APPEND);
?>
<h1>Welcome to the Home Page!</h1>

sudo nano help.php
<?php
$file = '/var/www/html/visits.txt';
$log = date('Y-m-d H:i:s') . " - Visited help.php\n";
file_put_contents($file, $log, FILE_APPEND);
?>
<h1>Welcome to the Help Page!</h1>

#Creer un fichier visits.txt et Configurer les permissions
sudo touch visits.txt
sudo chown apache:apache visits.txt
sudo chmod 777 visits.txt
#Tester les pages en ouvrant notre navigateur web et accédez à l'index 

http://44.220.133.127/index.php
http://44.220.133.127/help.php



