---
title: Session 8 - Bases de données
excerpt: ""
order: 8
---

1. TOC
{:toc}


Dans cette session nous verrons comment manipuler une base de données
relationelle avec
l'[ORM](https://fr.wikipedia.org/wiki/Mapping_objet-relationnel)
SQLAlachemy.

Dans les sessions précédentes, certains exemples utilisaient déjà une
base de données, cependant l'équipe enseignante avait décidé qu'elle
serait masquée : il vous était demandé de manipuler les données (lire
les données de la base de données) en utilisant des fonctions python
qui s'occupaient d'intérogger la base de données.

Pour bien illustrer ces étapes, nous démarrerons cette session à
partir d'une archive ZIP [tp_bdd.zip](https://github.com/badock/FlaskSar2019ExampleApp/archive/tp_bdd.zip) contenant un projet minimal, auquel
nous allons ajouter une base de données.

# Mise en place d'une connexion avec une base de données

Tout d'abord, ajouter cette ligne (si elle n'est pas déjà présente) dans le fichier `requirements.txt`:
```
flask-sqlalchemy
```

Cela indique à votre environnement de développement (PyCharm dans
notre cas) qu'il faut que la bibliothèque `flask-sqlalchemy` doit être
disponible dans notre application Flask.

Ensuite, créer un dossier `database` à la racine de votre dossier
d'application Flask, et créer un fichier `database/database.py` avec
le contenu suivant:

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()


def init_database():
    db.create_all()
```

Ce fichier fait les choses suivantes:
- une variable public `db` qui contient un objet initialisé avec une instance de l'ORM SQLAlchemy.
- une fonction `init_database`, dont l'appel initialise le modèle de données défini par l'application Flask (création des tables, initialisation des clés étrangères, contraintes de bases de données, etc)

Nous allons maintenant prendre en compte SQLAlchemy dans notre application. Pour cela, faites les modifications suivantes dans le fichier `app.py`:

1. en début de fichier, ajouter l'import suivant:
```python
from database.database import db, init_database
```
2. après la définition de la variable `app`, ajouter les lignes suivantes:
```python
db.init_app(app) # (1) flask prend en compte la base de donnee
with app.test_request_context(): # (2) bloc execute a l'initialisation de Flask
    init_database()
```

Ce que nous venons de faire, c'est de faire prendre consience à Flask
que la base de données existe, et nous avons demandé à Flask
d'initialiser la base de données quand l'application est démarrée.

Maintenant en lançant Flask, nous pouvons constater deux choses:
- on ne sait pas où sont stockées les données
- quelques avertissements apparaissent dans la console

Pour résoudre ces deux points, ajouter les instructions suivantes dans
le fichier `app.py`, juste après la définition de la variable `app`:

```python
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///database/database.db"
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False
```

# Définition de classes "Modèle"

Avec SQLAlchemy, il n'y a pas besoin de définir des tables avec des
instructions SQL. A la place, on définit des classes Python qui
étendent `db.Model` et on y ajoute des attributs utilisants la classe
`db.Column`, dont le premier argument définit le type de l'attribut,
et les autres arguments définissent des contraintes.

Dans cet exercice, nous allons définir une classe tâche. Créer le
fichier `database/models.py`, et y mettre le code suivant:

```python
from database.database import db

class Task(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    label = db.Column(db.Text)
    isDone = db.Column(db.Boolean)
```

Une liste non exaustive des types disponibles est:
- `Integer`: entier
- `Float`: nombre réel
- `String(size)`: chaine de caractère avec pour taille maximale `size`
- `Text`: texte sans limite de taille
- `DateTime`: stockage d'une date-heure ([datetime](https://docs.python.org/3/library/datetime.html#datetime.datetime))
- `Boolean `: booléen
- `PickleType`: stockage d'un objet python
- `LargeBinary`: stockage d'un objet binaire (fichier)

Vous trouverez plus d'informations sur les types disponibles et
comment les utiliser avec ce lien vers la documentation de
[flask-sqlalchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/models/#simple-example).

# Ajouter, modifier et supprimer des données

L'ORM `SQLAlchemy` introduit le concept de
[Session](https://docs.sqlalchemy.org/en/13/orm/session_basics.html),
qui sert de tampon entre la base de données et les données que
l'application Flask manipule. Une session sera utilisée en particulier
pour ajouter/modifier et supprimer des données.

On peut utiliser la fonction `session.add` pour:

- 1) ajouter un nouvel objet en base de données

```python
from database.database import db

# Creation d'une nouvelle tache. Elle n'est visible que localement
new_task = Task(label="tache1", isDone=False)

# Ajout de la tache dans la base de donnees
db.session.add(new_task)
db.session.commit() # Sauvegarde les informations dans la base de donnees

```

- 2) modifier un objet existant

```python
# Modification d'une tache
existing_task.isDone = not existing_task.isDone # On inverse l'etat d'une tache
db.session.add(existing_task)
db.session.commit() # Sauvegarde les informations dans la base de donnees
```

et la fonction `session.delete` permet de supprimer un objet de la base de données:
```python
# Suppression d'une tache
db.session.delete(existing_task)
db.session.commit() # Sauvegarde les informations dans la base de donnees
```

# Selectionner et filtrer des données

Plutôt que d'écrire des requêtes en langage SQL, SQLAlchemy propose de selectionner et filtrer les informations de la base de données en utilisant des fonctions python:


| Action        | Code         | type de retour | 
|:-------------|:------------------| :-----  |
| Lister toutes les tâches  | `Task.query.all()` | `list<Task>` |
| Récupéerer le premier élément des résultats d'une requête | `Task.query.first()` | `Task` ou `None` |
| Tâches qui ne sont pas finies | `Task.query.filter_by(isDone=False).all()` | `list<Task>` |
| Tâche qui correspond à un identifiant | `Task.query.filter_by(id=task_id).first() ` | `Task` ou `None` |
| Selection des paires d'attributs `label` et `isDone` | `db.session.query(Task.label, Task.isDone).filter_by(isDone=False).all()` | `list<tuple(str, bool)>` |
| Selection des paires d'attributs `label` et `isDone`, distinctes | `db.session.query(Task.label, Task.isDone).filter_by(isDone=False).distinct().all()` | `list<tuple(str, bool)>` |

# Mise en place de relations entre classes "Modèle"

En SQL il est possible de définir des [clés
étrangères](https://fr.wikipedia.org/wiki/Cl%C3%A9_%C3%A9trang%C3%A8re)
qui permettent de définir des relations entre différentes tables.

Il est possible de faire la même chose avec `SQLAlchemy`, en définissant des relations. Il existe principalement 2 types de relations:
- `OneToMany`: les classes A et B sont liées, un élément de A peut avoir plusieurs B, mais un élément de A ne peut avoir au maximum qu'un B
- `ManyToMany`: les classes A et B sont liées, un élément de A peut avoir plusieurs B, et un élément de A peut avoir plusieurs B

Traditionnellement, les relations "ManyToMany" étaient implémentées avec des tables d'association, comme il est montré dans ce lien de [la documentation de SQLAlchemy](https://docs.sqlalchemy.org/en/13/orm/basic_relationships.html#many-to-many). *Cependant, nous ferons le choix de représenter une relation "ManyToMany" entre une classe `A` et `B` avec une classe intermédiaire `C` et deux relations "OneToMany" `A->C` et `B->C`*.

Dans notre exemple de gestionnaire de tâches, nous allons introduire le concept de "liste de tâches". Il y aura plusieurs listes de tâches, au sein desquelles seront créées des tâches. Nous aurons les arités suivantes:
- Une liste de tâches pourra avoir plusieurs tâches
- Une tâche sera associée à une seul liste de tâches

Le code suivant illustre le modèle de tâches:
```python
class TaskList(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.Text)
    tasks = db.relationship('Task', backref='task_list', lazy='dynamic')


class Task(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    label = db.Column(db.Text)
    isDone = db.Column(db.Boolean)
    task_list_id = db.Column(db.Integer, db.ForeignKey('task_list.id'))
```


# Exercice
