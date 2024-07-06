#Exercice 5: Crontab

#Le script doit afficher Hello World
#Journaliser l'exécution du fichier 
#tester avec tail -f pour voir en temps réel les logs

#1.j'ai commencé par créer un fichier appelé:
touch hello.log

#2.nano
nano hello.sh
#!/bin/bash
    echo "Hello world">> /home/ec2-user/hello.log

#3.crontab pour la journalisation
crontab -e
* * * * * /home/ec2-user/hello.sh

#4.Tester avec tail -f pour voir en temps réel les logs
tail -n 1 -f hello.log



