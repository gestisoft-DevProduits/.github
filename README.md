# Standard de développement Gestisoft 
Ce document vise à identifier une procédure standard à suivre dans le cadre d’une nouvelle demande de développement.


Environnement de développement : Docker
---------------------------------------

Dans le cardre d'un développement de produit cette approche est priorisé
Docker donne une flexibilité au développer de configurer un environnement rapidement, 
il est possible d'accéder à la derniere version BC disponible via Artifacts.
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

l'utilisation de Pull Request est recommandé lors d'un développement d'une nouvelle extension ou la modification d'une extension existante,

le développeur ne doit pas commit le code directement dans main, il faut toujours ajouter **Code Reviewer** dans chaque Pull Request.

il existe des regles dans Github qu'il faut activer dans chaque nouveau projet/Repo,

![image](https://github.com/gestisoft-DevProduits/.github/assets/44379762/13f6eb98-8712-4a49-8f75-9c5d5dc5e007)


Version d'extension 
-------------------
Avec AL-Go for Github il est possible de configurer la gestion de version générer par le build pipeline 
il existe plusieurs stratégies proposé par l'outil 
