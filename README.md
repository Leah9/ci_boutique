User stories for web shop


| As a      | I want to be able to          | So that i can |
|-----------|-----------------------|----------------|
| Shopper   | View a list of products       | Select one or many to purchase |
| Shopper   | View single product details   | Identify the proce, read descripton, specification etc |
| Shopper   | View the total cost in basket | Avoid spending too much |
| Registration, login and user account      | |
| Shopper   | Log in / out                  | Acccess my account information |
| Shopper   | Recover password              | Restore account access |
| Shopper   | View my account informtion    | See previous order details |
| Searching for product                     |
| Shopper   | Sort products asc, desc       | Find cheapest or highest rated |
| Shopper   | Sort by category              | Only show what i am interested in buying |
| Shopper   | Search by product name        | Narrow down my list of items |



pip3 install 'django<4'

django-admin startproject ci_boutique .

Add thr following to .gitignore
*.sqlite3
*.pyc
.env

Test by running
python3 manage.py runserver
Should be able to open in browser
Ctrl + C 

Run initial migrations
python3 manage.py migrate

Create Superuser
python3 manage.py createsuperuser

*** SECRET KEY
in settings.py:
from dotenv import load_dotenv
load_dotenv()

IN settings.py
Remove the default Django secret_key
SECRET_KEY = str(os.getenv('SECRET_KEY'))

pip3 install python-dotenv

Create .env file with a secret key
SECRET_KEY= "whatever"





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
should display IT WORKS in green
 
Edit home/templates/home/index.html to read
```html
{% extends "base.html" %}
{% load static %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="container h-100">
        <div class="row h-100">
            <div class="col-7 col-md-6 my-auto">
                <h1 class="display-4 logo-font text-black">
                    The new collections are here
                </h1>
                <div class="my-5">
                    <h4>
                        <a href="" class="shop-now-button btn btn-lg rounded-0 text-uppercase py-3">Shop Now</a>
                    </h4>
                </div>
            </div>
        </div>
    </div>
{% endblock %}
```
Header, nav links and login or create account
EDIT templates/base.html to read
```html
{% load static %}

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
        <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
        <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
    {% endblock %}

    {% block extra_js %}
    {% endblock %}

    <title>Boutique Ado {% block extra_title %}{% endblock %}</title>
  </head>
  <body>
    <header class="container-fluid fixed-top">
        <div class="row">
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0 text-center text-lg-left">
                <a href="{% url 'home' %}" class="nav-link main-logo-link">
                    <h2 class="logo-font text-black my-0"><strong>Boutique</strong> Ado</h2>
                </a>
            </div>
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0">
                <form method="GET" action="">
                    <div class="input-group w-100">
                        <input class="form-control border border-black rounded-0" type="text" name="q" placeholder="Search our site">
                        <div class="input-group-append">
                            <button class="form-control btn btn-black border border-black rounded-0" type="submit">
                                <span class="icon">
                                    <i class="fas fa-search"></i>
                                </span>
                            </button>
                        </div>
                    </div>
                </form>
            </div>
            <div class="col-12 col-lg-4 my-auto py-1 py-lg-0">
                <ul class="list-inline list-unstyled text-center text-lg-right my-0">
                    <li class="list-inline-item dropdown">
                        <a class="text-black nav-link" href="#" id="user-options" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                            <div class="text-center">
                                <div><i class="fas fa-user fa-lg"></i></div>
                                <p class="my-0">My Account</p>
                            </div>
                        </a>
                        <div class="dropdown-menu border-0" aria-labelledby="user-options">
                            {% if request.user.is_authenticated %}
                                {% if request.user.is_superuser %}
                                    <a href="" class="dropdown-item">Product Management</a>
                                {% endif %}
                                <a href="" class="dropdown-item">My Profile</a>
                                <a href="{% url 'account_logout' %}" class="dropdown-item">Logout</a>
                            {% else %}
                                <a href="{% url 'account_signup' %}" class="dropdown-item">Register</a>
                                <a href="{% url 'account_login' %}" class="dropdown-item">Login</a>
                            {% endif %}
                        </div>
                    </li>
                    <li class="list-inline-item">
                        <a class="{% if grand_total %}text-info font-weight-bold{% else %}text-black{% endif %} nav-link" href="">
                            <div class="text-center">
                                <div><i class="fas fa-shopping-bag fa-lg"></i></div>
                                <p class="my-0">
                                    {% if grand_total %}
                                        £{{ grand_total|floatformat:2 }}
                                    {% else %}
                                        £0.00
                                    {% endif %}
                                </p>
                            </div>
                        </a>
                    </li>
                </ul>
            </div>
        </div>
    </header>

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

Create directories
mkdir static
mkdir media
mkdir static/css

create static/css/base.css with the content....

```html
html {
    height: 100%;
}

body {
    background: url('/media/homepage_background_cropped.jpg') no-repeat center center fixed;
    background-size: cover;
    height: calc(100vh - 164px);
    color: #555;
    font-family: 'Lato';
}

/* from Bulma */
.icon {
    align-items: center;
    display: inline-flex;
    justify-content: center;
    height: 1.5rem;
    width: 1.5rem;
}

.logo-font {
    text-transform: uppercase;
}

.main-logo-link {
    width: fit-content;
}

.shop-now-button {
    background: black;
    color: white;
    min-width: 260px;
}

.btn-black {
    background: black;
    color: white;
}

.shop-now-button:hover,
.shop-now-button:active,
.shop-now-button:focus,
.btn-black:hover,
.btn-black:active,
.btn-black:focus {
    background: #222;
    color: white;
}

.text-black {
    color: #000 !important;
}

.border-black {
    border: 1px solid black !important;
}

/* -------------------------------- Media Queries */

/* Slightly larger container on xl screens */
@media (min-width: 1200px) {
  .container {
    max-width: 80%;
  }
}

/* fixed top navbar only on medium and up */
@media (min-width: 992px) {
    .fixed-top-desktop-only {
        position: fixed;
        top: 0;
        right: 0;
        left: 0;
        z-index: 1030;
    }

    .header-container {
        padding-top: 164px;
    }
}
```

Sorts out issues with header
In templates/base.html change second div 'row' class to
<div id="topnav" class="row bg-white pt-lg-2 d-none d-lg-flex">

add MY font awesome link to top of  base.html
```html
<script src="https://kit.fontawesome.com/0067dce3fb.js" crossorigin="anonymous"></script>
```
AND
```html
<link rel="stylesheet" href="{% static 'css/base.css' %}">
```

IN settings.py add the following under STATIC_URL = '/static/'

STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

IN projectname/urls.py add the following near the top
from django.conf import settings
from django.conf.urls.static import static

and at the bottom after the closing square bracket
+ static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

CHECK IT WORKS
python3 manage.py runserver

## Adding nav bar
IN base.html top of body block, code is from bootstrap navbar
```html
 <div class="row bg-white">
    <nav class="navbar navbar-expand-lg navbar-light w-100">
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#main-nav" aria-controls="main-nav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        {% include 'includes/mobile-top-header.html' %}
        {% include 'includes/main-nav.html' %}
    </nav>
</div>
```
mkdir templates/includes

touch templates/includes/main-nav.html

touch templates/includes/mobile-top-header.html

IN base.html insert the following at the end of the header section
```html
<div class="row bg-white">
            <nav class="navbar navbar-expand-lg navbar-light w-100">
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#main-nav" aria-controls="main-nav" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                {% include 'includes/mobile-top-header.html' %}
                {% include 'includes/main-nav.html' %}
            </nav>
        </div>
        <div id="delivery-banner" class="row text-center">
            <div class="col bg-black text-white">
                <h4 class="logo-font my-1">Free delivery on orders over £{{ free_delivery_threshold }}!</h4>                
            </div>            
        </div>
```


