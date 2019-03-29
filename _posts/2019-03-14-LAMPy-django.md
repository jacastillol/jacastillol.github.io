---
title: "Web development in Django"
date: 2019-03-14
tags: [linux, servers, python, django]
header:
  image: "/images/fort point.png"
excerpt: "Linux, Servers, LAMP, Python, django"
mathjax: "true"
---

# Django over LAMPy
## Install
```bash
$ pip install django
```
## Settup a new project
To create a new project just type
```bash
$ django-admin startproject mysite
```
this will create the following tree structure:
```bash
mysite
├── manage.py
└── mysite
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```
now, inside `mysite` folder you can start the raw application called `main`
```bash
$ python manage.py startapp main
```
and you will obtain the following new tree structure:
```bash
mysite/
├── main
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── manage.py
└── mysite
    ├── __init__.py
    ├── __pycache__
    │   ├── __init__.cpython-36.pyc
    │   └── settings.cpython-36.pyc
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```
At this point you can see your webbrowser with
```bash
$ python manage.py runserver
```
and see http://localhost:8000.
In the `main` directory add the following `urls.py` file
```python
from django.urls import path
from . import views

app_name = "main"

urlpatterns = [
    path("", views.homepage, name="homepage")
]
```
and `views.py` file
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def homepage(request):
    return HttpResponse('Wwo this is an <strong>awesome</strong> tutorial')

```
finally modify `mysite/urls.py` with:
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('main.urls')),
    path('admin/', admin.site.urls),
]
```
## using models
Define a model with tree columns in a table, by default using sqlite3 
```python
from django.db import models

# Create your models here.
class Tutorial(models.Model):
    tutorial_title = models.ChardField(max_length=200)
    tutorial_content =  models.TextField()
    tutorial_published =  models.DateTimeField("date published")

    def __str__(self):
        return self.tutorial_title
```
After refresh the project you have to define the new model on the `settings.py` appending the line `'main.apps.MainConfig'` as is currently shown bellow
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'main.apps.MainConfig',
]
```
and after that run
```bash
$ python manage.py makemigrations
```
if all is right you will recieve:
```bash
Migrations for 'main':
  main/migrations/0001_initial.py
    - Create model Tutorial
```
you can see the created database:
```bash
$ python manage.py sqlmigrate main 0001
```
if all is right you will recieve:
```bash
BEGIN;
--
-- Create model Tutorial
--
CREATE TABLE "main_tutorial" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "tutorial_title" varchar(200) NOT NULL, "tutorial_content" text NOT NULL, "tutorial_published" datetime NOT NULL);
COMMIT;
```
finally migrate to the project
```bash
$ python manage.py migrate
```
and if all is right you will get:
```bash
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, main, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying main.0001_initial... OK
  Applying sessions.0001_initial... OK
```
### Using Shell for handling the project
This is an example on how to add a new data for the database from the django shell.
```bash
python manage.py shell
```
```python
Python 3.6.5 |Anaconda, Inc.| (default, Apr 29 2018, 16:14:56) 
Type 'copyright', 'credits' or 'license' for more information
IPython 6.4.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from main.models import Tutorial

In [2]: Tutorial.objects.all()
Out[2]: <QuerySet []>

In [3]: from django.utils import timezone

In [4]: new_tutorial = Tutorial(tutorial_title='To be', tutorial_content='... or not to be', tutorial_published=timezone.now())

In [5]: new_tutorial.save()

```
### Create a superuser for the app
```bash
python manage.py createsuperuser
```
go to the browser and enter in http://localhost:8000/admin/ and see you are now registered and have access. Now you need to define the access to the database editing the `main/admin.py` file
```python
from django.contrib import admin
from .models import Tutorial

# Register your models here.

admin.site.register(Tutorial)
```
you can control how the registers looks like modifying the last file adding the `TutorialAdmin` class as:
```python
class TutorialAdmin(admin.ModelAdmin):
    # First example commented:
    #fields = ["tutorial_title",
    #          "tutorial_published",
    #          "tutorial_content"]
    # Second example
    fieldsets = [
        ("Title/date", {"fields": ["tutorial_title", "tutorial_published"]}),
        ("Content", {"fields":["tutorial_content"]})
        ]

admin.site.register(Tutorial, TutorialAdmin)
```
you can customize even more by adding the TinyMCE 4. Follow this tutorial https://pythonprogramming.net/admin-apps-django-tutorial/.
## Render real pages
Instead of just a hello world, in the main page, we are going to define a real html.
## User Registration

 