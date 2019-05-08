---
title: Session 2 - Demander de l'information à l'utilisateur avec les formulaires
excerpt: ""
order: 1
---

# Session 2 - Demander de l'information à l'utilisateur avec les formulaires

1. TOC
{:toc}

## I- Rappels sur les formulaires

## II- Mise en place d'un formulaire pour editer des posts

### a) Récupération d'un code existant

Depuis PyCharm, récupérez le contenu de la branche
**TP1_formulaires**, en suivant les [instructions
suivantes](git.html#r%C3%A9cup%C3%A9rer-le-code-dune-branche-git-avec-pycharm).

### b) ajout du formulaire

Vous venez de récupérer un projet complet qui servira de base pour
expliquer les éléments introduits lors des prochaines sessions. Il
affiche des posts écrits par des auteurs, le tout stocké dans une base
de données simple.

**La seule chose qui manque à ce projet est un formulaire pour créer
ou éditer un post. Nous allons voir maintenant comment mettre en place
ce formulaire.**

Dans cet exemple d'application, un post contient 3 données:
1. un titre _sous forme de texte_
2. un contenu _sous forme de texte_

Dans le fichier `sar2019/forms.py`, nous allons créer une classe
`PostEditForm` qui contiendra une description "logique" de notre
formulaire:

```python
from flask_wtf import Form
from wtforms import StringField, TextAreaField
from wtforms.validators import DataRequired

class PostEditForm(Form):
    title = StringField('Title', validators=[DataRequired()])
    content = TextAreaField('Content', validators=[DataRequired()])
```

Nous pouvons faire les observations suivantes:
* La classe `PostEditForm` hérite de `flask_wtf.Form`, ce qui lui
  permet de mettre en place plusieurs comportements:
    * lier le contexte de variable Python avec le formulaire affiché en HTML
    * vérification des valeurs entrées par l'utilisateur
* La classe `PostEditForm` gère deux champs texte. Chaque champ
  texte, qui sont vérifiés à la soumission du formulaire : ici, il
  s'agira de vérifier que les valeurs envoyées ne sont pas vides.

Nous allons maintenant créer un objet `PostEditForm` dans la fonction
`create_or_process_post`, en lui passant:
* une représentation d'un post en base de données si on édite un post
* `None` si on crée un nouveau post

```python
post = database.models.Post.query.filter_by(id=post_id).first()
    
from sar2019.forms import PostEditForm
form = PostEditForm(obj=post)
```

```python
def display_post_form(post, form):
    return flask.render_template('edit_post_form.html.jinja2',
                                 form=form,
                                 post=post)
```

{% raw %}
```jinja
{% extends "layout.html.jinja2" %}
{% from "forms.html.jinja2" import render_field %}

{% block body %}
    <form action="{{ url_for("create_or_process_post", post_id=post.id) }}" method="post">
        {{ form.hidden_tag() }}

        {{ render_field(form.title) }}
        {{ render_field(form.content) }}

        <input type="submit">
    </form>
{% endblock %}
```
{% endraw %}

```python
def save_post_and_redirect_to_homepage(post, form):
    if post is None:
        post = database.models.Post()
        post.user_id = 1
    post.title = form.title.data
    post.content = form.content.data
    db.session.add(post)
    db.session.commit()

    return flask.redirect(flask.url_for('index'))
```

## V- Prochaine session

mise en page avec du CSS et le framework Bootstrap: [next session](session3.html)
