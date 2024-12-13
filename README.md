# Script de création d'utilisateurs en bash

## 1. Généralités
L'objectif du script est :
  * La création automatique d'utilisateurs
  * Que les utilisateurs à créer sont entrés en argument du script : `sudo ./addUsers.sh user1 user2 user3`  
    Attention, le script doit avoir les droits d'exécution !
  * Que le script doit avoir les pré-requis suivant :
    * Il doit y avoir une vérificationde la présence d'arguments. Sans arguments, le script affiche `Il manque les noms d'utilisateurs en argument - Fin du script` et il s'arrête.
    * Pour chaque utilisateur à créer, il doit y avoir une vérification de l'existence dans le système. S'il existe déjà, un message indiquera `L'utilisateur <nom_utilisateur> existe déjà.` et le script continue.
    * Pour chaque utilisateur créer, il doit y avoir une vérification de cette création. Si la création s'est bien effectuée, un message indiquera `L'utilisateur <nom_utilisateur> existe déjà.`. Sinon, un message indiquera `Erreur à la création de l'utilisateur <nom_utilisateur>.`. Dans tous les cas, le script continue.

## 2. Script
```bash

#!/bin/bash

# Check if any arguments is provided.
if [ $# -eq 0 ];then
    echo "Il manque les noms d'utilisateurs en argument - Fin du script"
    exit 1
fi

# Check if the user exist
args=$@

for user in $args;
do
    if grep -q "^$user:" /etc/passwd; then
        echo "L'utilisateur $user existe déjà"
    else
        if sudo useradd $user; then
            echo "L'utilisateur $user a été crée."
        else
            echo "Erreur à la création de l'utilisateur $user"
        fi
    fi
done

echo "[REMARQUE] Il n'est pas possible de continuer le script sans pour autant de nouveau rentrer des arguments!"
```

## 3. Commentaires
Il a été demandé que dans certains cas, le script doit continuer de fonctionner. Cependant, le script ne fonctionne qu'avec l'appel d'argument. Je ne pouvais donc pas rappeler le script sans remettre d'argument.
