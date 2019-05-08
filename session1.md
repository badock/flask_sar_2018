---
title: Session 1 - Développer une application Flask simple
excerpt: ""
order: 1
---

# Session 1 - Démarrer avec Flask et Python

1. TOC
{:toc}

Cette session part du principe que vous avez fait la [session
0](session0.html) : avant de démarrer cette session, assurez vous de
bien avoir un environnement de développement Flask fonctionnel. Si ce
n'est pas le cas, veuillez suivre les instructions de la [session
0](session0.html) avant de poursuivre avec les instructions
ci-dessous.


## I- Une première application web simple, avec Flask

Nous allons voir dans cette Section comment afficher un message dans le navigateur de l'utilisateur. Pour cela nous allons:

* Créer un nouveau projet Flask qui aura pour nom "FlaskTP1"
* Coder une vue qui affichera le texte "Helloworld" quand l'utilisateur accèdera à l'URL [http://127.0.0.1:5000/greetings](http://127.0.0.1:5000/greetings)

Pour cela, éditez le fichier `app.py` généré par PyCharm de manière à ce qu'il contienne le code suivant:

```python
@app.route('/greetings')
def some_function():
    return "Helloworld!"
```

Lancer l'application Flask, et accéder à cette vue via l'URL [http://127.0.0.1:5000](http://127.0.0.1:5000):

![capture d'écran montrant le resultation d'un helloworld avec Flask](/assets/img/session1/screen1.png)

Nous pouvons faire les observations suivantes:

* L'URL à entrer dans le navigateur est définie par la valeur passée à ```@app.route```
* Le résultat de l'execution de la fonction est envoyé au navigateur
* **Le nom donné à la fonction Python n'a pas d'importance**


## II- Envoyer du HTML à l'utilisateur

Dans cette sous-section nous allons voir comment renvoyer une réponse
HTML à l'utilisateur. Nous verrons d'abord qu'il est tout d'abord
possible de retourner une chaine de caractère contenant du HTML, mais
que ceci a l'inconvénient d'être peu lisible et de mélanger
difficilement HTML et Python. Nous verrons ensuite comment simplifier
cette cohabitation en utilisant des fichiers **Templates** basés sur
le moteur [Jinja2](http://jinja.pocoo.org/docs/2.10/).

### c) Retourner du HTML brut : une mauvaise idée

Nous pouvons faire une nouvelle vue, similaire à celle codée dans la
Section précédente, mais qui cette fois retournera une chaine de
caractère contenant du code HTML:

```python
@app.route('/greetings_with_html')
def some_function_html():
    return """
<html>
    <head>
        <title>Hello!</title>
    </head>
    <body>
        <b>Hello world!</b>
    </body>
</html>
    """
```

Nous pouvons faire les observations suivantes:
* La coloration synthaxique ne fonctionne pas pour le code HTML.
* Le mélange python et HTML est peu lisible
* Si notre application contient plusieurs fonctions suivant ce style
  de programmation, le fichier `app.py` deviendra rapidement très
  gros.
  
Une autre approche plus acceptable consiste à mettre le code HTML dans
un fichier HTML séparé du code Python, et de demander à la fonction
Python de servir le contenu de ce fichier HTML.

### b) Simplification grâce aux templates Jinja2

Flask utilise le moteur de template Jinja2

Nous allons maintenant voir comment utiliser les templates pour
générer des réponses utilisant du code HTML. Pour cela:

* 1) créer un fichier `first_template.html` dans le dossier templates
   (en violet dans PyCharm)
* 2) y mettre le contenu suivant:

```html
<html>
    <head>
        <title>Hello!</title>
    </head>
    <body>
        <b>Hello world!</b>
    </body>
</html>
```
- 3) créer une nouvelle fonction python dans le fichier `app.py`, qui reprendrait le code suivant:

```python
import flask # mettre cette ligne au debut de votre fichier Python

# [...]

@app.route('/greetings_with_html_and_template')
def some_function_html_with_template():
    return render_template("first_template.html")
```

Visiter la page [http://127.0.0.1:5000/greetings_with_html_and_template](http://127.0.0.1:5000/greetings_with_html_and_template), et constater que l'on obtient toujours le même affichage, tout en ayant:
- une séparation plus claire entre le code Python et le code HTML
- de la coloration synthaxique pour le code HTML

L'approche actuelle permet de servir une page HTML statique aux
utilisateurs. Nous verrons ensuite comment personnaliser cette page
avec Python et Flask.



### c) Une Template permet de générer du texte dynamiquement

Dans la Section II-a, nous avons vu comment servir une page HTML
statique. Le message affiché par cette page a toujours la même valeur,
ce qui limite son interet. Nous allons voir maintenant comment prendre
en compte des variables Python dans la réponse affichée à
l'utilisateur.

**L'objectif sera maintenant de prendre en compte des variables Python
la génération de la réponse. Nous verrons nottament comment faire une
boucle avec l'instruction `for` et comment faire une structure
conditionnelle avec l'instruction `if/else`**

Pour cela, nous allons:

- 1) modifier la fonction `some_function_html_with_template` afin qu'elle contienne le code suivant:

```python
colors = ["red", "blue", "green", "purple", "dark", "white"]
boolean_value = False
msg_if_boolean_value = "[boolean_value is true]"
msg_if_not_boolean_value = "[boolean_value is false]"
```

- 2) modifier l'appel à la fonction `render_template` afin qu'elle crée un contexte jinja2 qui contienne les variables Python précédemment crées:

```python
return render_template("first_template.html",
                       colors_arg=colors,
                       boolean_value_arg=boolean_value,
                       msg_if_boolean_value_arg=msg_if_boolean_value,
                       msg_if_not_boolean_value_arg=msg_if_not_boolean_value)
```

- 3) Modifier la template `templates/first_template.html` de manière à ce qu'elle affiche les variables passées à la fonction `render_template`:

```jinja
{% raw %}
<html>
    <head>
        <title>Hello!</title>
    </head>
    <body>
        {% for color in colors_arg %}
            {{ color }}
        {% endfor %}

        {% if boolean_value_arg %}
            {{ msg_if_boolean_value_arg }}
        {% else %}
            {{ msg_if_not_boolean_value_arg }}
        {% endif %}
    </body>
</html>
{% endraw %}
```

En visitant la page [http://127.0.0.1:5000/greetings_with_html_and_template](http://127.0.0.1:5000/greetings_with_html_and_template), vous obtiendez un résultat proche de:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session1/screen4.png)


On peut voir que les valeurs des variables Python sont bien prises en
compte pour former la réponse HTML : la réponse HTML sera adaptée au
contenu des variables Python. De plus des structures de contrôle
telles que les `if/else` et les `for/while` permettent de
structure le code Jinja2.

## III- Passer de l'information dans l'URL

Il est possible de récupérer des informations contenues dans l'URL que
l'utilisateur accède. Le code suivant définie une fonction qui prend 3
paramètres, et affiche un calcul:

```python
@app.route("/sum/<label>/<int:a>/<int:b>")
def compute_sum(label, a, b):
    c = a + b
    return label+" "+str(a)+" et "+str(b)+" est "+str(c)
```

Quand on accède à l'URL [http://127.0.0.1:5000/sum/la somme de
/3/5](http://127.0.0.1:5000/sum/la somme de /3/5), on obtient le
résultat suivant:

![capture d'écran montrant le programme d'installation de miniconda](/assets/img/session1/screen5.png)

## IV- Prochaine session

Demander de l'information avec les formulaires: [next session](session2.html)
