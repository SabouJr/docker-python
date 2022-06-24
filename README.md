# DJANGO APP WITH POSTGRE in Docker Containers

## Dockerfile

---

```Docker
FROM python:3
```

> Prend l'image de python version 3 en tant que base

```Docker
WORKDIR /code
```

> On se place dans le répertoire /code

```Docker
COPY requirements.txt /code/
```

> On copie les fichiers "requirements.txt", qui contient les dépences nécessaires a un projet DJANGO dans le répertoire courant /code/

```Docker
RUN pip install -r requirements.txt
```

> Installe les dépendances nécessaires pour l'application DJANGO

```Docker
COPY . /code/
```

> On copie tous les fichiers du répertoire courant de la machine sur le répertoire courant de l'image (/code/)

---

## Docker compose

### Service POSTGRE
---

```Docker
image: postgre
```

> Prend l'image postgre comme base du container

```Docker
volumes:
    - ./data/db:/var/lib/postgresql/data
```

> Créer un volume entre le répertoire ./data/db de la machine et le répertoire /var/lib/postgresql/data du container


```Docker
environnement: 
      - POSTGRES_NAME=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres 
```

> Définition des variable d'environnement afin de se connecter à la base de donnée POSTGRE

### Service PYTHON

---

```Docker
build .
```

> Lance le build du Dockerfile situé dans le répertoire courant

```Docker
command: python manage.py runserver 0.0.0.0:8000
```

> Execute les commande nécessaire au lancement d'un projet Django, au lancement du container web

```Docker
volumes:
    - .:/code
```

> Créer un volume entre le répertoire . (racine) de la machine et le répertoire /code du container

```Docker
ports:
    - 8443:3000
```

> Relie le port 8443 de la machine avec le port 3000 du container

```Docker
environnement: 
      - POSTGRES_NAME=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres 
```

> Définition des variable d'environnement afin de se connecter à la base de donnée POSTGRE


```Docker
depends_on:
      - db
```

> Précise au container qu'il se lancera seulement si le container DB est lancé

---
