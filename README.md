# Standard de développement Gestisoft 
Ce document vise à identifier une procédure standard à suivre dans le cadre d’une nouvelle demande de développement.


Environnement de développement : Docker
---------------------------------------

Dans le cardre d'un développement de produit cette approche est priorisé

Docker donne une flexibilité au développer de configurer un environnement rapidement,

il est possible d'accéder à la derniere version BC disponible via **Artifacts**.

https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-running-container-development

Avec AL-GO for Github il est exist un script qui permet de créer un environnement Docker, compilé et installé l'app à partir VS Code
![image](https://github.com/gestisoft-DevProduits/.github/assets/44379762/d613cbf0-156c-49ef-93fb-351f0eea28e8)


Environnement de développement : Sandbox
----------------------------------------
***il faut toujours priorisé l'approche Docker***

Dans le cadre de développement dans l'environnement du client, le sandbox doit être nommé **sandboxDev** qui sera une copie de l'environnement production

Avec AL-GO for Github il est exist un script qui permet de créer un environnement Cloud , compilé et installé l'app à partir VS Code
![image](https://github.com/gestisoft-DevProduits/.github/assets/44379762/5f8a35e3-8f13-4947-846e-571c077b7727)


https://github.com/microsoft/AL-Go/blob/main/Scenarios/CreateOnlineDevEnv.md


Branching 
---------

l'utilisation de Pull Request est obligatoire lors d'un développement d'une nouvelle extension ou la modification d'une extension existante,

le développeur ne doit pas commit le code directement dans main, il faut toujours ajouter **Code Reviewer** dans chaque Pull Request.

il existe des regles dans Github qu'il faut activer dans chaque nouveau projet/Repo,

![image](https://github.com/gestisoft-DevProduits/.github/assets/44379762/13f6eb98-8712-4a49-8f75-9c5d5dc5e007)


Version d'extension 
-------------------
Avec AL-Go for Github il est possible de configurer la gestion de version générer par le build pipeline

il existe plusieurs stratégies proposé par l'outil, nous avons choisi l'option 2 qui permet de générer une version basé sur la date du build et l'heure en **UTC**, 

la version majeur et mineur sera récupéré de l'extension

![image](https://github.com/gestisoft-DevProduits/.github/assets/44379762/5cf5c83e-36e9-4b4d-b217-92a2cc632b72)

Pour activer cette option,  il faut modifier le fichier **settings.json** dans le dossier **AL-GO**

![image](https://github.com/gestisoft-DevProduits/.github/assets/44379762/c76c8c9a-dbed-498e-b868-f5943a9a8ea9)


déploiement: PTE
----------------- 

l'activation de déploiement automatisé est nécessaire pour accélerer et faciliter l'installation des extensions.

En suivant la recomandation de Freddy, voir Blog ci-dessous 

https://freddysblog.com/2022/05/06/deployment-strategies-and-al-go-for-github/

il faut configurer le déploiement automatisé vers un environnement QA et un environnement production,

Aprés chaque Pull Request le dépoliement s'execute dans le SandboxQA, 

à l'acceptation finale du développement il fau t executer **Publish To Environment** et choisir l'environnement production pour déployer en production.

Pour accélerer l'accés à la dérniere version du build disponible, il est recommandé d'activer l'aption **STORAGECONTEXT**

Cette option permet de faire un upload de la derniére version du build dans un **Blob Stoage** , l'URL peut être partgé dans le Teams du client 



déploiement: Développement de produit 
-------------------------------------

il existe toujours deux branches (main et Release)

**main** : contient une version stable avec du code qui n'est pas encore déployé.

**Release**: contient le code de la dérniére version diponible pour les clients. 

pour le développement d'une nouvelle fonction, il faut toujours créer une branche copie de main

Dans le modéle **fast track** un conseiller technique doit copier la branhce **Release** aprés l'accepation du développement il faut faire un Rebase code avec **main**

Pour chaque extension il existe un URL Blob Storage qui contient le dernier build disponible, il est partagé avec l'équipe BC dans le Teams **Business Central** dans le channel **Info Produit**

![image](https://github.com/gestisoft-DevProduits/.github/assets/44379762/46a26512-2546-4c9c-81af-fccdab87ee0c)
Dépendance à une extension sur GitHub
---------------------------------------
Si l'extension (X) a une dépendance avec une autre extension (Y) sur un Repo GitHub différent, le Repo étranger peut être ajouté aux chemins de vérification des dépendances **appDependencyProbingPaths** dans le fichier de paramètres AL-Go. La dépendance doit également être ajoutée au fichier app.json en tant que dépendance.

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/a2ec106f-ea7e-473d-9517-68ffd1cb4a58)

**appDependencyProbingPaths** doit avoir un tableau json avec la structure suivante :  
**repo** : contient le nom de l'orgnanisation + le nom de Repo ("gestisoft-DevProduits/GoReport") ou ("Gestisoft-clients/Dandurand").

**version**: on a deux choix :spécifier la version de Release de la dépendance ou utiliser (latest) pour récupérer la derniére version de Release de la dépendance.

**release_status** : specifier le  type de Release de la dépendance. Les artefacts peuvent être téléchargés à partir release, prerelease, or a draft. **Pour nos projets on utilise Release**.

**authTokenSecret**: pour télécharger les artefacts, un jeton d'accès est nécessaire. Dans ce cas, un secret doit être ajouté aux secrets de GitHub, notre secret est : **GHTOKENWORKFLOW**.

**projects** : spécifie le projet dans un repository multi-projets. « * » signifie tous les projets.

Documentation Technique des extensions 
---------------------------------------

AL Doc : L'outil ALDoc génère une documentation à partir d'informations symboliques et syntaxiques, de commentaires de code et de la structure globale de l'application en fonction du ou des fichiers .app d'entrée.

On peut intégrer AL Doc dans github workflows pour automatiser la génération de documentation ou générer la documentation localement avec l'aide de DocFx.

pour activer la documentation de code , il faut accéder à **Settings -> Pages -> Source -> Selectionner Github Actions**

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/704136a9-eeeb-4803-9d4d-4e84c3aa5c2b)

nous constaterons que le workflow qui génère la documentation a été ajouté dans la section Actions :

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/b1e43faa-d9ee-4ce6-815e-3a0738459b58)

C'est possible d'executer ce workflow manuellement pour générer la documentation technique .

Pour automatiser ce workflow on peut l'integrer dans le CI/CD workflow  en ajoutant 2 paramètres au fichier AL-Go-Settings.json :

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/aafe12eb-ac79-4192-b0cd-3f2e2e8d3736)

**deployToGitHubPages** = Détermine si le site de documentation de référence doit être déployé sur le GitHub pages pour le repository.

**continuousDeployment** = Détermine si la documentation de référence sera déployée en continu dans le CI/CD workflow .

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/75ec2da4-4ffb-4d41-a7cc-61125726bbaf)

on peut trouver notre documentation dans la section deploiement :

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/73a6c29b-a396-4f62-89ac-c03ba0fd651b)

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/7979cf73-3da8-4f1b-9c1e-c11182bc7981)

Pour faire des changements pour la documentation , on utilisera le bloc de code xml, le workflow traitera ce code et il va l'ajouter à la documentation automatiquement :

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/5f80ce58-3a38-47ab-aed2-f76e0fa68113)

Résultat : 

![image](https://github.com/gestisoft-DevProduits/.github/assets/143097318/99725f46-848c-4495-84a1-144132417264)

Compiler un build 
---------------------------------------
### éviter la compilation de Runtime Packages
"doNotPublishApps": true , va nous aider à éviter de compiler les runtime packages et générer le build correctement 
Sans DoNotPublishApps : 
![image](https://github.com/user-attachments/assets/2b260d5f-618d-41dc-a052-9f739a68f71b)
Avec le DoNotPublishApps :
![image](https://github.com/user-attachments/assets/0178d140-17d4-4997-8371-d05902cd0be2)

### BuildMode + ConditionalSettings
![image](https://github.com/user-attachments/assets/664ce5f4-9b0c-4500-80fd-12476eac21b4)
BuildMode nous aide à générer multiple builds :
![image](https://github.com/user-attachments/assets/746e4895-50d3-4543-a789-f5149221eaae)

On peut combiner entre BuildMode et ConditionalSettings pour configurer N build avec N process diffèrent :

Exemple :

Build 1.N : un build normal sans aucun paramétrage.

Build 1.N+1 : sera générer comme le build 1.N , avec l'artifact de Next Minor.

Build 1.N+2 : sera générer comme le build 1.N , avec l'artifact de Next Major et déployer dans l'envrionnement XYZ.

### incrementalBuilds
un bon feature si on utilise des multiples apps dans un même repository car ce paramètre permet de spécifier si le repo va re compiler tout le repo (multiple apps dans le repo) ou juste l'un des apps qui ont été modifié dans le repo .
Pour notre cas on n'a pas besoin de ce paramètre car on utilise une app par repo.

Deploiement
---------------------------------------

### Deploiment d'une App avec le CICD
Configuer le AAD , ajouter le AAD au tant que utilisateur dans BC environnement
![image](https://github.com/user-attachments/assets/02433b3a-bd9e-4b64-99d3-9b7ea3b8ffe3)
![image](https://github.com/user-attachments/assets/18811209-2655-41cb-b3b8-58a44fc48918)

Configurer les secrets environnement :
![image](https://github.com/user-attachments/assets/899f2c7b-f836-4348-81c2-43a59f6972c4)

https://github.com/microsoft/AL-Go/blob/main/Scenarios/RegisterSandboxEnvironment.md

Run CICD :

![image](https://github.com/user-attachments/assets/fe2431f6-28b6-4468-a85a-90b752316569)
