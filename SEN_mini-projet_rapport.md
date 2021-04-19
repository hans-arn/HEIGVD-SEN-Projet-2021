# SEN - Projet - Spear Phishing

> Auteurs : Jérôme Arn, Cassandre Wojciechowski
>
> Date : 14.06.2021

## 1 Descriptions d'outils

### 1.1 GoPhish

#### 1.1.1 Introduction 

GoPhish est un outil gratuit de phishing open source dont on trouve les fichiers sur [Github](https://github.com/gophish/gophish/releases). Il tourne sur Linux, Windows et mac OS. Avec cet outil, il est possible d'envoyer des e-mails de phishing et d'avoir un retour immédiat sur les e-mails qui ont été ouverts : combien ont été ouverts, sur lesquels le destinataire a cliqué, dans le cas d'une redirection sur une fausse page on peut connaître les données entrées par la victime. 

Cet outil a d'abord été pensé dans l'optique de tester les réactions de destinataires d'e-mails frauduleux. L'installation se veut très facile et l'utilisation aussi. L'interface web a l'avantage d'être très simple et claire. A côté des champs qui peuvent poser problème à l'utilisateur de l'outil, des bulles d'aide sont présentes pour l'orienter. 

#### 1.1.2 Installation

Comme établi précédemment, l'installation se veut très facile. La procédure suivante a été testée sur une machine Ubuntu 20.04 : 

- Télécharger le fichier .zip depuis le repo Github : https://github.com/gophish/gophish/releases
- Créer un répertoire `gophish` 
- Placer dans le répertoire `gophish` le fichier .zip précédemment téléchargé
- Décompresser le fichier .zip 
- Ajouter des permissions d'exécution sur le fichier binaire `gophish`

```sh
// Commandes à effectuer : 

$ mkdir gophish
$ mv gophish*.zip gophish
$ cd gophish
$ unzip gophish*.zip
$ cd gophish*
$ chmod u+x gophish
```

#### 1.1.3 Utilisation 

Pour utiliser l'outil GoPhish, il faut exécuter le fichier binaire `gophish` : 

```sh
$ ./gophish
```

![](images/SEN_projet_GoPhish2.png)

Il est possible qu'une erreur `fatal` apparaisse car le port 80 est déjà utilisé par un autre service et GoPhish souhaite par défaut s'en servir pour afficher les pages qu'il va cloner. Ceci est modifiable dans le fichier `config.json` décrit ci-après. 

Sur la capture d'écran ci-dessus, on observe à la cinquième ligne un nom d'utilisateur `admin` et un mot de passer composé de lettres et de chiffres. Il faut garder ces données le temps d'effectuer la première connexion sur l'outil. 

Pour se connecter sur l'interface graphique évoquée précédemment, il faut lancer un navigateur web et y entrer l'URL `https://127.0.0.1:3333`. Si cette URL ne fonctionne pas, il faut se référer au fichier `config.json` qui contient les adresses IP et les ports pour se connecter au service. En se connectant sur l'URL, on trouve la page suivante, sur laquelle il faut entrer les identifiants indiqués par les logs au démarrage : 

<img src="images/SEN_projet_GoPhish3.png" style="zoom:67%;" />

GoPhish nous demande ensuite de modifier le mot de passe. Après modification, nous accédons à la page d'accueil permettant d'entreprendre toutes sortes d'actions : 

<img src="images/SEN_projet_GoPhish5.png" style="zoom: 67%;" />

Pour lancer une campagne, il faut d'abord passer par plusieurs étapes : 

1. Créer un profil (`Sending Profiles`) qui va matérialiser un utilisateur imaginaire envoyant des e-mails
2. Créer un modèle d'e-mail (`Email Templates`) pour modéliser un e-mail donnant envie à un destinataire de l'ouvrir et le lire
3. Créer une fausse page (`Landing Pages`) qui sera créé par l'outil en cas de clic de l'utilisateur sur le lien comporté dans l'e-mail
4. Créer un groupe de personnes ciblées (`Users & Groups`), soit en important des e-mails depuis un .csv, soit manuellement
5. Finalement, créer la campagne (`Campaigns`) en composant avec les éléments créés précédemment
6. Lancer la campagne !

#### 1.1.4 Démonstration

Pour créer un profil, il faut se rendre sur l'onglet correspondant et entrer les informations requises : 

<img src="images/SEN_projet_GoPhish7.png" style="zoom: 67%;" />

Ci-dessus un exemple montrant un profil créé pour faire croire à une personne que Dolly Parton lui a envoyé un e-mail. Nous passons par un serveur smtp de Google. Nous avons du accéder au compte `cassandre.wojcie` et ajouter un mot de passe d'application pour autoriser GoPhish à passer par ce compte gmail. Une fois les informations complétées, il faut sauvegarder le profil créé. 

Pour créer un modèle d'e-mail, il faut se rendre sur l'onglet correspondant puis fournir les informations demandées : 

<img src="images/SEN_projet_GoPhish8.png" style="zoom:67%;" />

Sur l'exemple ci-dessus, un modèle a été trouvé sur [Github](https://github.com/criggs626/PhishingTemplates) pour faire croire à un destinataire que son compte Spotify rencontre des problèmes. Il suffit de copier le modèle mis à disposition sur Github, puis de le coller dans la fenêtre HTML figurant sur la page GoPhish. 

Pour créer une "landing page", il suffit d'entrer un lien en cliquant sur le bouton "Import Site", puis la page web entrée est copiée par GoPhish. 

<img src="images/SEN_projet_GoPhish9.png" style="zoom:67%;" />

Pour créer des groupes de destinataires, il est possible d'importer les informations depuis un fichier ou de les entrer manuellement. 

<img src="images/SEN_projet_GoPhish10.png" style="zoom:67%;" />

Il faut préciser les prénoms et noms, ainsi que l'adresse e-mail et éventuellement le poste. Les champs "nom" et "prénom" sont importants à compléter car ils peuvent être réutilisés dans le corps du message afin de personnaliser l'e-mail et de mettre en confiance le destinataire. 

Puis finalement, on peut lancer une campagne : 

<img src="images/SEN_projet_GoPhish11.png" style="zoom:67%;" />

Pour lancer la campagne, il faut indiquer les éléments créés que l'on souhaite utiliser. Le champs URL doit être complété en indiquant l'adresse à laquelle la page clonée sera matérialisée. Dans le cadre de cette démonstration, tout est effectué en local et l'adresse IP indiquée est l'adresse de la machine utilisée pour créer la campagne. Le port 8081 est le port indiqué dans le fichier `config.json`. Pour une campagne réelle, il faudrait passer par un serveur configuré à cet effet pour éviter de modifier toutes les configurations de notre machine personnelle et de devoir la laisser allumée tout au long de la campagne. 

Pour lancer la campagne, il faut cliquer sur le bouton "Launch Campaign". En allant sur la boite mail utilisée pour le test, nous y trouvons bien le mail créé précédemment : 

 <img src="images/SEN_projet_GoPhish12.png" style="zoom: 80%;" />

Nous observons que le prénom fourni est utilisé pour la formule de salutation et qu'un lien est inséré dans le mail. En cliquant sur le lien, nous arrivons sur la page suivante, dont on peut observer l'URL : 

<img src="images/SEN_projet_GoPhish13.png" style="zoom:67%;" />

L'URL est celle indiquée sur la page de lancement de campagne associée à un `rid`. 

#### 1.1.5 Conclusion 

En suivant le processus d'installation et d'utilisation, nous constatons que l'outil GoPhish est très bien fait et très simple d'utilisation. La documentation regorge d'informations pour aider l'utilisateur et beaucoup d'utilisateurs mettent à disposition des modèles d'e-mails ou de pages qui peuvent être utilisés pour lancer des campagnes. Le temps nécessaire à la prise en main de cet outil est très court. Dans le cadre de la rédaction de ce rapport, une heure a suffit pour l'installation et la démonstration. 

L'outil est très bien imaginé et son but est clair : lancer des campagnes de phishing au sein d'une entreprise pour déterminer les employés sensibles à ces e-mails. Lors de la démonstration, nous avons simplement redirigé l'utilisateur sur une page Youtube clonée, mais il serait plus probable que lors d'un audit de sécurité, le destinataire soit redirigé sur un formulaire afin d'entrer des informations. Celles-ci peuvent être récupérées par GoPhish. 

L'outil est très utile et permet d'automatiser de nombreuses tâches telles que le clonage d'une page et l'envoi en masse d'e-mails. 

### 1.2 SpiderFoot

#### 1.2.1 Introduction 

(à quoi il sert, OS, payant/gratuit, opensource ?)

#### 1.2.2 Installation

#### 1.2.3 Utilisation 

(comment il s'utilise ?)

#### 1.2.4 Démonstration

#### 1.2.5 Conclusion 

(utilité, intérêt, facilité)

### 1.3 instagramOSINT

#### 1.3.1 Introduction

 (à quoi il sert, OS, payant/gratuit, opensource ?)

#### 1.3.2 Installation

#### 1.3.3 Utilisation 

(comment il s'utilise ?)

#### 1.3.4 Démonstration

#### 1.3.5 Conclusion 

(utilité, intérêt, facilité)

### 1.4 Sherlock

#### 1.4.1 Introduction

 (à quoi il sert, OS, payant/gratuit, opensource ?)

#### 1.4.2 Installation

#### 1.4.3 Utilisation 

(comment il s'utilise ?)

#### 1.4.4 Démonstration

#### 1.4.5 Conclusion 

(utilité, intérêt, facilité)

## 2 Recherche d'informations sur une cible

### 2.1 Choix

justification du choix de cible

### 2.2 Eléments découverts

### 2.3 Portrait de la cible 

(caractéristiques, vulnérabilités, vecteurs d'attaque, conclusions)

## 3 Scénario d'attaque

### 3.1 Objectif de l'attaque 

(vol d'info, d'argent, malwares, ...)

### 3.2 Vecteur(s) d'attaque 

(réseau social, e-mail, ...)

### 3.3 Payload de l'attaque 

(site web, fichier infecté, ...)

## 4 Simulation d'attaque

### 4.1 Description de l'attaque 

(étapes)

### 4.2 Comportements de la cible

(vis-à-vis de l'attaque, réactions, actions ...)

### 4.3 Résultats obtenus

 (vs. résultats attendus)