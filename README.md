# OCR  Project N°8  Deployez un modèle dans le Cloud

## Enoncés OCR (start-up de l'AgriTach "FRUITS!")
https://openclassrooms.com/fr/paths/164/projects/633/assignment

#### Projet:
Votre start-up souhaite développer une application mobile qui à partir d'une photo un fruit permettrait d'obtenir des informations sur ce fruit, afin de mettre en place une première version du moteur de classification des images de fruits.
Le développement de l’application mobile permettra de construire une première version de l'architecture Big Data nécessaire.

#### Données (Kaggle):
https://www.kaggle.com/moltean/fruits

#### BigData_AWS_Spark
Il faut **développer** dans un **environnement Big Data** une première chaîne de traitement des données qui comprendra:
1. le **preprocessing**
2. une étape de **réduction de dimension**
3. Rq: il n’est **pas nécessaire d’entraîner un modèle** pour le moment

L’**important est de mettre en place les premières briques de traitement** qui serviront lorsqu’il faudra passer à l’échelle en termes de volume de données !

#### **Contraintes**
<br>
Vous devrez tenir compte dans vos développements du fait que le volume de données va augmenter très rapidement après la livraison de ce projet.
Vous développerez donc des scripts en **Pyspark** et utiliserez par exemple le cloud **AWS** pour profiter d’une architecture Big Data (EC2, S3, IAM), basée sur un serveur EC2 Linux.

La mise en œuvre d’une architecture Big Data sous (par exemple) AWS peut nécessiter une configuration serveur plus puissante que celle proposée gratuitement (EC2 = t2.micro, 1 Go RAM, 8 Go disque serveur).

#### **Livrables attendus**

- Un **notebook** sur le cloud contenant les scripts en Pyspark exécutables
1. le preprocessing
2. une étape de réduction de dimension

- Les **images du jeu de données initial**

- la **sortie de la réduction de dimension** (une matrice écrite sur un fichier CSV ou autre) **disponible dans un espace de stockage** sur le cloud

- Un support de présentation pour la soutenance, présentant :
    1. les différentes briques d'architecture choisies sur le cloud ;
    2. leur rôle dans l’architecture Big Data ;
    3. les étapes de la chaîne de traitement.
   


### Approche (Lérys Granado)
https://github.com/LerysG/Project_8_OC_BigData_AWS_Spark

Pour la preuve-de-concept (et à cause des ressources limitées), j'ai sélectionné arbitrairement 4 catégories de fruits dans le jeux de données et sélectionné à chaque fois deux images random, **total = 8 images**. Je les ai uploadé sur mon serveur S3. Après une longue étape de set-up (j'ai essayé une dizaine d'instance), j'ai finalement opté d'héberger ce code sur une serveur **EC2 payant** (t2.medium, 4 Go RAM, 30 Go disque serveur, OS = Linux 18.x, 64-bit). L'étape d'installation de l'environement PySpark est **très très compliquée** . <br>

Récapitulation des étapes :

- sélectionner  la bonne instance (EC2 ou SageMaker), avec la bonne mémoire et suffisamment de stockage,

- créer un groupe de sécurité (configurer ssh particulièrement)

- installer PuttY (convertir clé .pem en .ppk, setup la connextion ssh, attention au username = "ec2-user" **ou** "ubuntu")

- créer un bucket S3 (et setup les autorisations/droits en .json, grâce au générateur de stratégie)

- créer un AMI user et bien garder les clés access et private

- **Dans le serveur EC2**, tout en ligne de commande :
    1. télécharger puis ```bash``` anaconda, https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh **vraiment faire attention aux versions**, 

    2. installer java ```sudo apt install openjdk-8-jre-headless```, **vraiment faire attention aux versions**, 

    3. installer scala 

    4. télécharger puis décompresser spark-3.0.3-hadoop-2.7, **vraiment faire attention aux versions**, 

    5. gérer les credentials jupyter,

    6. A rajouter configurer dans le path

    7. gérer les crédentials du S3 via les clés données par le AMI user (fichier .aws), ou ```aws configure```
    (le cas échéant installer awscli :   ```sudo apt install awscli```,
    
    8. ajouter tous les pkgs nécessaires (entre-autre tensorflow, py4j) ... dans ? requirements.txt ?

    9. ajuster le swap RAM de l'instance pour éviter les memory crashes