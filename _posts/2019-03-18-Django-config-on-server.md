---
title: "Django configuration on LAMP"
date: 2019-03-14
tags: [linux, servers, python, django]
header:
  image: "/images/fort point.png"
excerpt: "Linux, Servers, LAMP, Python, django"
mathjax: "true"
---

# Configuration of Django on the server

## installing required packages
```bash
$ sudo apt install -y python3-pip python3-setuptools
$ sudo python3 -m pip install --upgrade pip
$ sudo python3 -m install django django-tinymce4-lite
```
## Creating a new project
In the directory permited by the server admin gives you, create the new project, here called `mysite`
```bash
$ django-admin startproject mysite
```
Now you have to edit the `settings.py` file inside the `mysite` directory.
1. Turn to off the `DEBUG` mode setting this variable to false.
1. Set the `ALLOWED_HOSTS` to the host name of your application

### Create a quick test app
Go to the mysite directory where the manage.py is located and type:
```bash
$ cd mysite
$ python3 manage.py startapp it_works
```
this would have been created a new directory called `it_works` and now we are going to edit this file with the following:
1. on the `mysite/urls.py` file 
  ```python
  from django.contrib import admin
  from django.urls import path, include

  urlpatterns = [
    path('', include('it_works.urls')),
    path('admin/', admin.site.urls),
  ]  
  ```
1. on the `it_works/urls.py` file
  ```python
  from django.urls import path
  from . import views

  app_name = 'it_works'

  urlpatterns = [
    path("", views.homepage, name="homepage"),
  ]  
  ```
1. on the `it_works/views.py` add the following three lines of code
  ```python
  from django.http import HttpResponse
  def homepage(request):
    return HttpResponse("It works! (served from Django)")
  ```
### Install Apache and the WSGI module to connect python with Apache
If you have already installed the Apache on your computer remove `apache2` from the next
```bash
$ sudo apt install -y apache2 libapache2-mod-wsgi-py3
```

#### Optional
Create a virtual envorionment for the application
```bash
$ sudo pip3 install virtualenv
$ virtualenv -p python3 myenv
$ python3 -m venv myenv
```
Run the vs `code` IDE inside the new virtual evnronment
```bash
$ source ./venv/bin/activate
(venv) $ code .
```
to deactivate the envronment
```bash
(venv) $ deactivate
```
### using static files
```bash
(venv) $ python manage.py collectstatic
```

### All steps in the console

1. Create a virtual environment with django
  ```bash
  $ mkdir manillas-prj && cd manillas-prj
  $ python3 -m venv ./venv
  $ source ./venv/bin/activate
  (venv) $ pip install --upgrade pip
  (venv) $ pip install django
  ```
1. Create and setup a project, a git repo, and hello server.
  ```bash
  (venv) $ django-admin startproject manillas .
  (venv) $ python3 manage.py help
  (venv) $ git init
  (venv) $ echo 'touch .gitignore with help of gitignore.io'
  (venv) $ git add . && git commit -m 'Initial commit'
  (venv) $ python3 manage.py runserver
  (venv) $ echo 'use the web browser to see hello in localhost:8000'
  ```
1.  Adding an app to the project in other console type.
  ```bash
  (venv) $ python3 manage.py startapp pages
  (venv) $ echo 'Add in settings a new installed app as pages.apps.PagesConfig'
  (venv) $ echo 'config new urls'
  ```
1.  Using templates, changing `HttpResponse('<h1>Hello</h1>')` by `render(request, 'pages/index.html')`.
  ```bash
  (venv) $ echo 'In the setting file config Templates directory'
  (venv) $ echo 'os.path.join(BASE_DIR, "templates")'
  (venv) $ mkdir 'templates'
  (venv) $ mkdir 'templates/pages'
  (venv) $ touch 'templates/pages/index.html'
  (venv) $ touch 'templates/pages/about.html'
  ```
  Create a `base.html` that handles the general content with `{{ "{% block content" }}%}`, and modify `index.html` and the others with `{{ "{% extends 'base.html" }}%}` and other metaprogramming template stuffs.
1. How to handle static files and paths
  1. configure static files in the project, go to `settings.py`
    ```
    # Static files (CSS, JavaScript, Images)
    # https://docs.djangoproject.com/en/2.1/howto/static-files/

    STATIC_ROOT = os.path.join(BASE_DIR, 'static')
    STATIC_URL = '/static/'
    STATICFILES_DIRS = [
        os.path.join(BASE_DIR, 'btre/static')
    ]
    ```
    and update the static files project management with `python manage.py collectstatic`
  1. create some directories inside `manillas/static` and inside the following `css`, `js`, `webfonts`
  1. create `img` directory inside `manillas/static` with only the images that will be static.
  1. use `{{ "{% extends 'base.html'" }}%}` and `{{ "{% load static" }}%}` to handle urls inside a template partial `{{ "{% url 'index'" }}%}` for an internal hyperlink.
  1. use a `partials` to handle top, navigation and footer bar for example (`partials/_topbar.html` and use (`{{ "{% include 'partials/_topbar.html'" }}%}`) inside `base.html`).
1. POSTGRESQL
  1. `sudo apt update && sudo apt install postgresql postgresql-contrib`
  1. check is working `sudo -u postgres psql` and `\q` for quit postgres
  1. Install pgadmin4 `python3 -m pip install wheel` and then `pip install ./pgadmin4-4.3-py2.py3-none-any.whl` but first you have to download `./pgadmin4-4.3-py2.py3-none-any.whl` from pgadmin web page.
  1. Create a new data base and set a password to `postgres` role user
    ```bash
    $ sudo -u postgres psql
    postgres=# \password postgres
    postgres=# CREATE DATABASE manillasdb OWNER postgres;
    postgres=# \q
    $ sudo ./venv/bin/python3 venv/lib/python3.6/site-packages/pgadmin4/pgAdmin4.py
    ```
    Then create a new database server under the postgres role and password. Do not forget to set the hostname.
  1. Install some packages `pip install psycopg2` and ` pip install psycopg2-binary`
  1. Setting up on Django on settings.py
  ```python
  DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'manillas',
        'USER': 'postgres',
        'PASSWORD': '************',
        'HOST': 'localhost',
    }
  }
  ```
  stop server and apply migrations with `python manage.py migrate` and restart.
  1. Define a schema for the data base for example:
  ```
  MODEL/DB FIELDS

  ### LISTING
  id: INT
  realtor: INT (FOREIGN KEY [realtor])
  title: STR
  address: STR
  city: STR
  state: STR
  zipcode: STR
  description: TEXT
  price: INT
  bedrooms: INT
  bathrooms: INT
  garage: INT [0]
  sqft: INT
  lot_size: FLOAT
  is_published: BOOL [true]
  list_date: DATE
  photo_main: STR
  photo_1: STR
  photo_2: STR
  photo_3: STR
  photo_4: STR
  photo_5: STR
  photo_6: STR

  ### REALTOR
  id: INT
  name: STR
  photo: STR
  description: TEXT
  email: STR
  phone: STR
  is_mvp: BOOL [0]
  hire_date: DATE

  ### CONTACT
  id: INT
  user_id: INT
  listing: INT
  listing_id: INT
  name: STR
  email: STR
  phone: STR
  message: TEXT
  contact_date: DATE

  ```
  and define all the related classes
  1. `python manage.py makemigrations`
  1. check details of the sql creation tables `python manage.py sqlmigrate listings 0001`
  1. `python manage.py migrate` this create the data base.
1. Configuring django admin with a superuser.
  1. `python manage.py createsuperuser` for django admin host
  1. register the models on `admin.py` with the line `admin.site.resgister(Listing)` now it will appear in the django admin service
1. Configuring a Media features
  1. defining a new `class ListingAdmin():` it is possible to define how the forms will be shown in the django admin service.
  1. Edit settings.py for media
    ```bash
    # Media Folder Settings
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
    MEDIA_URL = '/media/'
    ```
    and append to the `manillas/url.py` the following thre lines
    ```python
    from django.conf import settings
    from django.conf.urls.static import static
    urlpatterns = [
      ...
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    ```

