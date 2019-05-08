---
title: Session 0 - Démarrer avec Flask et Python
excerpt: ""
order: 1
---

# Session 0 - Démarrer avec Flask et Python

1. TOC
{:toc}

Dans le cas où vous ne disposiez pas déjà d'un environnement de
développement Python, nous en installerons un dans cette session en utilisant le logiciel
[*miniconda*](https://docs.conda.io/en/latest/miniconda.html) pour
configurer rapidement un tel environnement. Si vous disposez déjà d'un
environnement Python fonctionnel, vous pouvez sauter cette étape.


## I- Installation d'un environnement Python avec Miniconda et PyCharm

### A- Installation de Miniconda

Avec votre navigateur, visitez [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html) pour télécharger le programme d'installation qui correspond à votre configuration.

Il faut exécuter le programme d'installation sur votre machine. _Il est possible que le programme d'installation vous demande d'entrer un mot de passe administrateur_. Au cours de l'installation, utilisez les paramètres par défaut.

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen2.png)

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen4.png)

Vous pouvez le programme d'installation : Miniconda est maintenant installé sur l'ordinateur. 

**Si l'installation n'a pas fonctionné correctement ou des messages d'erreur sont apparus, parlez-en à un des enseignants afin de ne pas prendre de retard sur la session.**

### B- Installation de Git

### C- Installation et configuration de PyCharm


#### 1. Installation de PyCharm

Avec votre navigateur, visitez [https://www.jetbrains.com/pycharm/download](https://www.jetbrains.com/pycharm/download) pour télécharger le programme d'installation de "PyCharm" qui correspond à votre configuration.

Bien que la version *PyCharm Community* puisse être suffisante pour ce module, **nous vous conseillons vivement** de faire un compte sur le site [jetbrains.com](jetbrains.com) avec votre adresse email "imt-atlantique.net" et de récupérer ainsi gratuitement la version *PyCharm Professional*, cette dernière incluant un plug-in qui facilite beaucoup le développement d'un projet Flask.

Démarrez le programme d'installation récupéré sur le site [https://www.jetbrains.com/pycharm/download](https://www.jetbrains.com/pycharm/download), et continuez avec une installation utilisant les paramètres par défaut.

**Si l'installation n'a pas fonctionné correctement ou des messages d'erreur sont apparus, parlez-en à un des enseignants afin de ne pas prendre de retard sur la session.**

Une fois l'installation terminée avec succès, lancez "PyCharm". Vous devriez voir apparaitre un écran d'accueil qui ressemble à cela:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen1.png)

#### 2. Vérification de l'installation

Pour vérifier que "PyCharm" et "Miniconda" sont bien intégrés, nous allons créer un projet basique. Sur l'écran d'accueil de "PyCharm", cliquez sur *create new project*:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen13.png)

Un écran vous demandant des renseignements sur le projet apparaitra:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen9.png)

Faites les actions suivantes:
1. Sélectionnez "Flask" comme type de projet
2. Entrez un nom de projet (*FlaskExample*)
3. Sélectionnez "Conda"
4. Validez la création du projet

PyCharm va configurer le nouveau projet. Cette étape peut prendre quelques minutes. Au bout du compte, une fenêtre affichant le code du projet exemple devrait apparaitre:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen14.png)

Nous allons lancer le projet afin de voir si **Python** et **Flask** sont bien installés. Pour cela, cliquez sur l'icône en forme d'insecte en haut à droite de la fenêtre PyCharm:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen10.png)

Une console devrait apparaitre en bas de votre fenêtre PyCharm:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen11.png)


Avec votre navigateur web, tapez l'[URL](https://fr.wikipedia.org/wiki/Uniform_Resource_Locator) indiquée dans la console. Un texte devrait apparaitre dans le navigateur:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session0/screen8.png)



## II- Démarrer avec le projet exemple depuis Github

Nous allons maintenant récupérer un exemple de projet Flask
fonctionnel, qui met en oeuvre les éléments qui seront introduits au
cours des prochaines séances. Ce projet est hébergé sur la plateforme
d'hébergement de code source [Github](https://github.com). Nous vous
conseillons vivement de vous faire un compte sur Github, et de
l'utiliser pour héberger vos futurs projets de développement
logiciels.

### A- Récupération du code depuis Github

Le projet d'exemple est hébergé dans le dépot suivant:

[https://github.com/badock/FlaskSar2019ExampleApp](https://github.com/badock/FlaskSar2019ExampleApp)

Vous allez créer un nouveau projet PyCharm se basant sur le code
hébergé dans le dépot précédent:

![capture d'écran montrant comment récupérer le projet exemple sur Github](/assets/img/session0/screen15.png)

![capture d'écran montrant comment récupérer le projet exemple sur Github](/assets/img/session0/screen16.png)

![capture d'écran montrant comment installer les dépendances logicielles](/assets/img/session0/screen17.png)

### B- Lancement du projet et configuration du mode _deboggage_

![capture d'écran montrant comment installer les dépendances logicielles](/assets/img/session0/screen18.png)

## Next step : implementing a view

Congratulations! You can now proceed with the [next session](/session1)
