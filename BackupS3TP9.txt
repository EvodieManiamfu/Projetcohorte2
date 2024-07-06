#Exercice 9: Backup S3

#Création des instances EC2 et S3 : Vous lancez une instance EC2 pour exécuter le script de backup du fichier archivé (contenant 4 fichiers). Vous créez un bucket S3 pour stocker les backups. Et arrêter une fois que nous atteignons 4 backups.
#Configuration des permissions avec IAM : créez un rôle IAM avec des permissions d'accès complet à S3 (S3 Full Access) attaché à l'instance EC2.
#Création du script de backup : Vous utilisé tar pour créer des archives de nos fichiers et dossiers.

#1. Créer un bucket S3 pour stocker les backups(nom role-role, create)
#2. Créer un bucket S3 pour stocker les backups,
#-cliquer sur IAM/role/create role/on choisit ec2/next/ rechercher:Amazons3FullAccess/next/nom-role:instance_storage_s3/create role
#3. Lancer une instance EC2 et attacher le rôle IAM à l'instance,
#selecte instance/action/securité/modify IAM role/ rechercher le nom du role pour associer ca avec l'instance/update IAM role
#4. Écrire et exécuter un script de backup sur l'instance EC2.

#verification de la connexion
S3 ls S3:// mywebserverstorage2 = instance_storage_s3
# créer un dossier
mkdir instance_storage2~
#changement de repectoire
cd instance_storage2
#créer un fichier
touch fichier1 fichier2

nano backup_script2.sh
#!/bin/bash

SOURCE_DIR="/home/ec2-user/instance_storage2"
S3_BUCKET="mywebserverstorage2"
DATE=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_FILE="/tmp/backup_$DATE.tar.gz"

tar -czf $BACKUP_FILE $SOURCE_DIR

aws s3 cp $BACKUP_FILE s3://$S3_BUCKET/

rm $BACKUP_FILE

crontab -e
* * * * * /home/ec2-user/backup_script2.sh
# permission
chmod u+x backup_script2.sh
./ backup_script2.sh