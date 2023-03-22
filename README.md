pip3 install 'django<4'

django-admin startproject ci_boutique .

Add thr following to .gitignore
*.sqlite3
*.pyc

Test by running
python3 manage.py runserver
Should be able to open in browser
Ctrl + C 

Run initial migrations
python3 manage.py migrate

Create Superuser
python3 manage.py createsuperuser

git remote -v
git add .
git commit -m "Initial commit"
git push

*** Login and auth
AllAuth
pip3 install django-allauth==0.41.0

In settings.py add the following below the templates
AUTHENTICATION_BACKENDS = [
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
]

Add the following to installed apps
    'django.contrib.auth', # may already be present
    'django.contrib.messages', # may already be present
    'django.contrib.sites',

    'allauth',
    'allauth.account',
    'allauth.socialaccount',

Add the following below authentication backends
SITE_ID = 1

Add "import os" to the top of the file

In urls.py under urlpatterns add
path('accounts/', include('allauth.urls')),

edit  from django.urls import path, include

python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py runserver
open in browser / admin/ login
change domain to whatever.example.com4Display nemw to whatever, onlu impotand if usingsocial media auth

git add .
git commit -m "add auth"
git push

Edit settings.py
ADD below site id
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
ACCOUNT_AUTHENTICATION_METHOD = 'username_email'
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_EMAIL_VERIFICATION = 'mandatory'
ACCOUNT_SIGNUP_EMAIL_ENTER_TWICE = True
ACCOUNT_USERNAME_MIN_LENGTH = 4
LOGIN_URL = '/accounts/login/'
LOGIN_REDIRECT_URL = '/'


python3 manage.py runserver
Check browser /admin/ 
add email to superuser, verified and primary, save

pip3 freeze > requirements.txt
mkdir templates
mkdir templates/allauth
git add .
git commit -m "setup allauth"
git push

** Bootstrap
copying templates
cp -r ../.pip-modules/lib/python3.8//site-packages/allauth/templates/* ./templates/allauth/
delete openid and tests folders

create a new base.html file in templates folder with the contents :
{% load static %}
```html
<!doctype html>
<html lang="en">
  <head>

    {% block meta %}
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    {% endblock %}

    {% block extra_meta %}
    {% endblock %}

    {% block corecss %}
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
    {% endblock %}

    {% block extra_css %}
    {% endblock %}

    {% block corejs %}
        <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
        <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
    {% endblock %}

    {% block extra_js %}
    {% endblock %}

    <title>Boutique Ado {% block extra_title %}{% endblock %}</title>
  </head>
  <body>
    <header class="container-fluid fixed-top"></header>

    {% if messages %}
        <div class="message-container"></div>
    {% endif %}

    {% block page_header %}
    {% endblock %}

    {% block content %}
    {% endblock %}

    {% block postloadjs %}
    {% endblock %}

    
  </body>
</html>
```

python3 manage.py startapp home
mkdir -p home/templates/home

create index.html in this folder with content
```html
{% extends "base.html" %}
{% load static %}

{% block content %}
    <h1 class="display-4 text-success">It works!</h1>
{% endblock %}
```

Edit views.py to add
```python
def index(request):
    """ A view to return the index page """

    return render(request, 'home/index.html')
```

create a new urls.py file in the "home" directory and insert the following

```python
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='home')
]
```

In the BASE urls.py add the following urlpattern
    path('', include('home.urls')),

In settings.py add the following in installed apps
    'home',

In settings.py TEMPLATES DIRS add

            os.path.join(BASE_DIR, 'templates'),
            os.path.join(BASE_DIR, 'templates', 'allauth'),

python3 manage.py runserver