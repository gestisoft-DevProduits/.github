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
