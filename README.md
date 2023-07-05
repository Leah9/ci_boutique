User stories for web shop


| As a      | I want to be able to          | So that i can |
|-----------|-----------------------|----------------|
| Shopper   | View a list of products       | Select one or many to purchase |
| Shopper   | View single product details   | Identify the product, read descripton, specification etc |
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
/* pad the top more when navbar is collapsed on mobile */
@media (max-width: 991px) {
    .header-container {
        padding-top: 116px;
    }

    body {
        height: calc(100vh - 116px);
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

# Setting up products
python3 manage.py startapp products
pip3 install pillow

In settings.py add the following to the INSTALLED_APPS section
'products',

mkdir products/fixtures

IN product/models.py...
```python
class Category(models.Model):
    name = models.CharField(max_length=254)
    friendly_name = models.CharField(max_length=254, null=True, blank=True)

    def __str__(self):
        return self.name

    def get_friendly_name(self):
        return self.friendly_name


class Product(models.Model):
    category = models.ForeignKey('Category', null=True, blank=True, on_delete=models.SET_NULL)
    sku = models.CharField(max_length=254, null=True, blank=True)
    name = models.CharField(max_length=254)
    description = models.TextField()
    price = models.DecimalField(max_digits=6, decimal_places=2)
    rating = models.DecimalField(max_digits=6, decimal_places=2, null=True, blank=True)
    image_url = models.URLField(max_length=1024, null=True, blank=True)
    image = models.ImageField(null=True, blank=True)

    def __str__(self):
        return self.name
```
To test the category / product classes are set correctly
python3 manage.py makemigrations --dry-run 
then if happy
python3 manage.py makemigrations
python3 manage.py migrate --plan
python3 manage.py migrate

in admin.py ADD the following

```python
from .models import Product, Category

# Register your models here.
admin.site.register(Product)
admin.site.register(Category)
```

### For demo purposes use categories.json and products.json
images from https://github.com/Code-Institute-Solutions/boutique_ado_v1/tree/bf096a773ea7e32253e20f58c1d6139317f681be/media
python3 manage.py loaddata categories
python3 manage.py loaddata products

The categories and products are then visible in admin

Fis the plural categorys issux in models.py
```python
class Category(models.Model):
    class Meta:
        verbose_name_plural = 'Categories'
```

## Improve display of categories and products in admin
ADD the following to products/admin.py
```python
class ProductAdmin(admin.ModelAdmin):
    list_display = (
        'sku',
        'name',
        'category',
        'price',
        'rating',
        'image',
    )

    ordering = ('sku',)


class CategoryAdmin(admin.ModelAdmin):
    list_display = (
        'friendly_name',
        'name',
    )


admin.site.register(Product, ProductAdmin)
admin.site.register(Category, CategoryAdmin)
```

## Setup all product view
In prducts/views.py

```python
from django.shortcuts import render
from .models import Product

# Create your views here.
def all_products(request):
    """ A view to show all products, sorting and search queries """
    products = Product.objects.all()
    context = {
        'products': products,
    }
    return render(request, 'products/products.html', context)
```

remove from django.contrib import admin from home/urls.py

touch products/urls.py

In products/urls.py
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.all_products, name='products')
]
```

In project_name/urls.py eg "boutique_ado/urls.py" add the following
```python
path('products/', include('products.urls')),
```

## Make inner template directories ?? WTF
mkdir -p products/templates/products
touch products/templates/products/products.html

In that file add
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
<div class="overlay"></div>
<div class="container">
    <div class="row">
        <div class="col">
            {{ products }}
        </div>
    </div>
</div>
{% endblock %}
```

In base.css add

```
.overlay {
	height: 100%;
	width: 100%;
	top: 0;
	left: 0;
	position: fixed;
	background: white;
	z-index: -1;
}
```

ptoducts/templates/products/products.html
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
    <div class="overlay"></div>
    <div class="container-fluid">
        <div class="row">
            <div class="col text-center mt-3">
                <h2 class="logo-font">Products</h2>
                <hr class="w-50 mb-1">
            </div>
        </div>
        <div class="row">
            <div class="product-container col-10 offset-1">
                <div class="row mt-1 mb-2"></div>
                <div class="row">
                    {% for product in products %}
                        <div class="col-sm-6 col-md-6 col-lg-4 col-xl-3">
                            <div class="card h-100 border-0">
                                {% if product.image %}
                                <a href="">
                                    <img class="card-img-top img-fluid" src="{{ product.image.url }}" alt="{{ product.name }}">
                                </a>
                                {% else %}
                                <a href="">
                                    <img class="card-img-top img-fluid" src="{{ MEDIA_URL }}noimage.png" alt="{{ product.name }}">
                                </a>
                                {% endif %}
                                <div class="card-body pb-0">
                                    <p class="mb-0">{{ product.name }}</p>
                                </div>
                                <div class="card-footer bg-white pt-0 border-0 text-left">
                                    <div class="row">
                                        <div class="col">
                                            <p class="lead mb-0 text-left font-weight-bold">£{{ product.price }}</p>
                                            {% if product.rating %}
                                                <small class="text-muted"><i class="fas fa-star mr-1"></i>{{ product.rating }} / 5</small>
                                            {% else %}
                                                <small class="text-muted">No Rating</small>
                                            {% endif %}
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        {% if forloop.counter|divisibleby:1 %}
                            <div class="col-12 d-sm-none mb-5">
                                <hr>
                            </div>
                        {% endif %}                        
                        {% if forloop.counter|divisibleby:2 %}
                            <div class="col-12 d-none d-sm-block d-md-block d-lg-none mb-5">
                                <hr>
                            </div>
                        {% endif %}
                        {% if forloop.counter|divisibleby:3 %}
                            <div class="col-12 d-none d-lg-block d-xl-none mb-5">
                                <hr>
                            </div>
                        {% endif %}
                        {% if forloop.counter|divisibleby:4 %}
                            <div class="col-12 d-none d-xl-block mb-5">
                                <hr>
                            </div>
                        {% endif %}
                    {% endfor %}
                </div>
            </div>
        </div>
    </div>
{% endblock %}
```

Make the link to products work

In templates/includes/main-nav.html add the following to the first dropdown
<a href="{% url 'products' %}" class="dropdown-item">All Products</a>

In home/templates/home/index.html
<a href="{% url 'products' %}" class="shop-now-button btn btn-lg rounded-0 text-uppercase py-3">Shop Now</a>

## Create the product detail view
In products/views.py add 
```python
from django.shortcuts import render, get_object_or_404

def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)
```

In products/urls.py add
```python
path('<product_id>', views.product_detail, name='product_detail'),
```
### Create the template...
cp products/templates/products/products.html products/templates/products/product_detail.html
replace econtent with below
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
    <div class="overlay"></div>
    <div class="container-fluid">
        <div class="row">
            <div class="col-12 col-md-6 col-lg-4 offset-lg-2">
                <div class="image-container my-5">
                    {% if product.image %}
                        <a href="{{ product.image.url }}" target="_blank">
                            <img class="card-img-top img-fluid" src="{{ product.image.url }}" alt="{{ product.name }}">
                        </a>
                        {% else %}
                        <a href="">
                            <img class="card-img-top img-fluid" src="{{ MEDIA_URL }}noimage.png" alt="{{ product.name }}">
                        </a>
                    {% endif %}
                </div>
            </div>
            <div class="col-12 col-md-6 col-lg-4">
                <div class="product-details-container mb-5 mt-md-5">
                    <p class="mb-0">{{ product.name }}</p>
                    <p class="lead mb-0 text-left font-weight-bold">${{ product.price }}</p>
                    {% if product.rating %}
                        <small class="text-muted"><i class="fas fa-star mr-1"></i>{{ product.rating }} / 5</small>
                    {% else %}
                        <small class="text-muted">No Rating</small>
                    {% endif %}
                    <p class="mt-3">{{ product.description }}</p>
                </div>
            </div>
        </div>
    </div>
{% endblock %}
```

In products/templates/products/products.html add the link to the individual product, two places
```python
{% url 'product_detail' product.id %}
```

## Implement search
In templates/base.html
change 
    <form method="GET" action="">
to
    <form method="GET" action="{ url 'products'%}">

AND in templates/includes/mobile-top-header.html do the same

IN products/temlates/views.py
Enuure the following is present in import
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from .models import Product

CHANGE / ADD to def all_products TO


## Set up categories

templates/includes/main-nav.html

```html
<div class="collapse navbar-collapse" id="main-nav">
    <ul class="navbar-nav w-auto mx-auto">
        <li class="nav-item dropdown">
            <a class="logo-font font-weight-bold nav-link text-black mr-5" href="#" id="all-products-link" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                All Products
            </a>
            <div class="dropdown-menu border-0" aria-labelledby="all-products-link">
                <a href="" class="dropdown-item">By Price</a>
                <a href="" class="dropdown-item ">By Rating</a>
                <a href="" class="dropdown-item ">By Category</a>
                <a href="{% url 'products' %}" class="dropdown-item">All Products</a>
            </div>
        </li>

        <li class="nav-item dropdown">
            <a class="logo-font font-weight-bold nav-link text-black mr-5" href="#" id="clothing-link" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                Clothing
            </a>
            <div class="dropdown-menu border-0" aria-labelledby="clothing-link">
                <a href="{% url 'products' %}?category=activewear,essentials" class="dropdown-item">Activewear &amp; Essentials</a>
                <a href="{% url 'products' %}?category=jeans" class="dropdown-item">Jeans</a>
                <a href="{% url 'products' %}?category=shirts" class="dropdown-item">Shirts</a>
                <a href="{% url 'products' %}?category=activewear,essentials,jeans,shirts" class="dropdown-item">All Clothing</a>
            </div>
        </li>

        <li class="nav-item dropdown">
            <a class="logo-font font-weight-bold nav-link text-black mr-5" href="#" id="homeware-link" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                Homeware
            </a>
            <div class="dropdown-menu border-0" aria-labelledby="homeware-link">
                <a href="{% url 'products' %}?category=bed_bath" class="dropdown-item">Bed &amp; Bath</a>
                <a href="{% url 'products' %}?category=kitchen_dining" class="dropdown-item">Kitchen &amp; Dining</a>
                <a href="{% url 'products' %}?category=bed_bath,kitchen_dining" class="dropdown-item">All Homeware</a>
            </div>
        </li>

        <li class="nav-item dropdown">
            <a class="logo-font font-weight-bold nav-link text-black" href="#" id="specials-link" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                Special Offers
            </a>
            <div class="dropdown-menu border-0" aria-labelledby="specials-link">
                <a href="{% url 'products' %}?category=new_arrivals" class="dropdown-item">New Arrivals</a>
                <a href="{% url 'products' %}?category=deals" class="dropdown-item">Deals</a>
                <a href="{% url 'products' %}?category=clearance" class="dropdown-item">Clearance</a>
                <a href="{% url 'products' %}?category=new_arrivals,deals,clearance" class="dropdown-item">All Specials</a>
            </div>
        </li>
    </ul>
</div>
```

In product/views.py
add
from .models import Product, Category
```python
def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None

    if request.GET:
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
    }
```

## Sorting Products
In templates/includes/main-nav.html
            <div class="dropdown-menu border-0" aria-labelledby="all-products-link">
                <a href="{% url 'products' %}?sort=price&direction=asc" class="dropdown-item">By Price</a>
                <a href="{% url 'products' %}?sort=rating&direction=desc" class="dropdown-item ">By Rating</a>
                <a href="{% url 'products' %}?sort=category&direction=asc" class="dropdown-item ">By Category</a>
                <a href="{% url 'products' %}" class="dropdown-item">All Products</a>
            </div>

In templates/views.py
```python
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from .models import Product, Category

# Create your views here.

def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))

            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)
            
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)


def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)
```

### Sorting display info
#### Add category diaplay to products page
in products/templates/products/products.html
under product price
 {% if product.category %}
                                            <p class="small mt-1 mb-0">
                                                <a class="text-muted" href="{% url 'products' %}?category={{ product.category.name }}">
                                                    <i class="fas fa-tag mr-1"></i>{{ product.category.friendly_name }}
                                                </a>
                                            </p>
                                            {% endif %}
also add in and product-detail.html

#### Add category hyperlink if all categories is selected
in products/templates/products/products.html 

<h2 class="logo-font">Products</h2>
                {% for c in current_categories %}
                    <a class="category-badge text-decoration-none" href="{% url 'products' %}?category={{ c.name }}">
                        <span class="p-2 mt-2 badge badge-white text-black rounded-0 border border-dark">{{ c.friendly_name }}</span>
                    </a>
                {% endfor %}

css to invert on hover...in ststic/css/base.css
a.category-badge > span.badge:hover {
    background: #212529 !important;
    color: #fff !important;
}

#### Sort dropdowns products/templates/products/products.hml

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
    <div class="overlay"></div>
    <div class="container-fluid">
        <div class="row">
            <div class="col text-center mt-3">
                <h2 class="logo-font">Products</h2>
                {% for c in current_categories %}
                    <a class="category-badge text-decoration-none" href="{% url 'products' %}?category={{ c.name }}">
                        <span class="p-2 mt-2 badge badge-white text-black rounded-0 border border-dark">{{ c.friendly_name }}</span>
                    </a>
                {% endfor %}
                <hr class="w-50 mb-1">
            </div>
        </div>
        <div class="row">
            <div class="product-container col-10 offset-1">
                <div class="row mt-1 mb-2">
                    <div class="col-12 col-md-6 my-auto order-md-last d-flex justify-content-center justify-content-md-end">
                        <div class="sort-select-wrapper w-50">
                            <select id="sort-selector" class="custom-select custom-select-sm rounded-0 border border-{% if current_sorting != 'None_None' %}info{% else %}black{% endif %}">
                                <option value="reset" {% if current_sorting == 'None_None' %}selected{% endif %}>Sort by...</option>
                                <option value="price_asc" {% if current_sorting == 'price_asc' %}selected{% endif %}>Price (low to high)</option>
                                <option value="price_desc" {% if current_sorting == 'price_desc' %}selected{% endif %}>Price (high to low)</option>
                                <option value="rating_asc" {% if current_sorting == 'rating_asc' %}selected{% endif %}>Rating (low to high)</option>
                                <option value="rating_desc" {% if current_sorting == 'rating_desc' %}selected{% endif %}>Rating (high to low)</option>
                                <option value="name_asc" {% if current_sorting == 'name_asc' %}selected{% endif %}>Name (A-Z)</option>
                                <option value="name_desc" {% if current_sorting == 'name_desc' %}selected{% endif %}>Name (Z-A)</option>
                                <option value="category_asc" {% if current_sorting == 'category_asc' %}selected{% endif %}>Category (A-Z)</option>
                                <option value="category_desc" {% if current_sorting == 'category_desc' %}selected{% endif %}>Category (Z-A)</option>
                            </select>
                        </div>
                    </div>
                    <div class="col-12 col-md-6 order-md-first">
                        <p class="text-muted mt-3 text-center text-md-left">
                            {% if search_term or current_categories or current_sorting != 'None_None' %}
                                <span class="small"><a href="{% url 'products' %}">Products Home</a> | </span>
                            {% endif %}
                            {{ products|length }} Products{% if search_term %} found for <strong>"{{ search_term }}"</strong>{% endif %}
                        </p>
                    </div>
                </div>
                <div class="row">
                    {% for product in products %}
                        <div class="col-sm-6 col-md-6 col-lg-4 col-xl-3">
                            <div class="card h-100 border-0">
                                {% if product.image %}
                                <a href="{% url 'product_detail' product.id %}">
                                    <img class="card-img-top img-fluid" src="{{ product.image.url }}" alt="{{ product.name }}">
                                </a>
                                {% else %}
                                <a href="{% url 'product_detail' product.id %}">
                                    <img class="card-img-top img-fluid" src="{{ MEDIA_URL }}noimage.png" alt="{{ product.name }}">
                                </a>
                                {% endif %}
                                <div class="card-body pb-0">
                                    <p class="mb-0">{{ product.name }}</p>
                                </div>
                                <div class="card-footer bg-white pt-0 border-0 text-left">
                                    <div class="row">
                                        <div class="col">
                                            <p class="lead mb-0 text-left font-weight-bold">${{ product.price }}</p>
                                            {% if product.category %}
                                            <p class="small mt-1 mb-0">
                                                <a class="text-muted" href="{% url 'products' %}?category={{ product.category.name }}">
                                                    <i class="fas fa-tag mr-1"></i>{{ product.category.friendly_name }}
                                                </a>
                                            </p>
                                            {% endif %}
                                            {% if product.rating %}
                                                <small class="text-muted"><i class="fas fa-star mr-1"></i>{{ product.rating }} / 5</small>
                                            {% else %}
                                                <small class="text-muted">No Rating</small>
                                            {% endif %}
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        {% if forloop.counter|divisibleby:1 %}
                            <div class="col-12 d-sm-none mb-5">
                                <hr>
                            </div>
                        {% endif %}                        
                        {% if forloop.counter|divisibleby:2 %}
                            <div class="col-12 d-none d-sm-block d-md-block d-lg-none mb-5">
                                <hr>
                            </div>
                        {% endif %}
                        {% if forloop.counter|divisibleby:3 %}
                            <div class="col-12 d-none d-lg-block d-xl-none mb-5">
                                <hr>
                            </div>
                        {% endif %}
                        {% if forloop.counter|divisibleby:4 %}
                            <div class="col-12 d-none d-xl-block mb-5">
                                <hr>
                            </div>
                        {% endif %}
                    {% endfor %}
                </div>
            </div>
        </div>
    </div>
{% endblock %}

IN product/views.py
    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))
            if sortkey == 'category':
                sortkey = 'category__name'
            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)

### JS to make dropdown work
in products/templates/products/products.html at the bottom


{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript">
		$('.btt-link').click(function(e) {
			window.scrollTo(0,0)
		})
	</script>
    
    <script type="text/javascript">
        $('#sort-selector').change(function() {
            var selector = $(this);
            var currentUrl = new URL(window.location);

            var selectedVal = selector.val();
            if(selectedVal != "reset"){
                var sort = selectedVal.split("_")[0];
                var direction = selectedVal.split("_")[1];

                currentUrl.searchParams.set("sort", sort);
                currentUrl.searchParams.set("direction", direction);

                window.location.replace(currentUrl);
            } else {
                currentUrl.searchParams.delete("sort");
                currentUrl.searchParams.delete("direction");

                window.location.replace(currentUrl);
            }
        })
    </script>
{% endblock %}

add the following to views.py
from djanfo.db.models.functions import Lower

### To top button
    <div class="btt-button shadow-sm rounded-0 border border-black">
        <a class="btt-link d-flex h-100">
            <i class="fas fa-arrow-up text-black mx-auto my-auto"></i>
        </a>	
    </div>
{% endblock %}

{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript">
		$('.btt-link').click(function(e) {
			window.scrollTo(0,0)
		})
	</script>
    

## Sorting Products
In templates/includes/main-nav.html
            <div class="dropdown-menu border-0" aria-labelledby="all-products-link">
                <a href="{% url 'products' %}?sort=price&direction=asc" class="dropdown-item">By Price</a>
                <a href="{% url 'products' %}?sort=rating&direction=desc" class="dropdown-item ">By Rating</a>
                <a href="{% url 'products' %}?sort=category&direction=asc" class="dropdown-item ">By Category</a>
                <a href="{% url 'products' %}" class="dropdown-item">All Products</a>
            </div>

In templates/views.py
```python
from django.shortcuts import render, redirect, reverse, get_object_or_404
from django.contrib import messages
from django.db.models import Q
from .models import Product, Category

# Create your views here.

def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None
    categories = None
    sort = None
    direction = None

    if request.GET:
        if 'sort' in request.GET:
            sortkey = request.GET['sort']
            sort = sortkey
            if sortkey == 'name':
                sortkey = 'lower_name'
                products = products.annotate(lower_name=Lower('name'))

            if 'direction' in request.GET:
                direction = request.GET['direction']
                if direction == 'desc':
                    sortkey = f'-{sortkey}'
            products = products.order_by(sortkey)
            
        if 'category' in request.GET:
            categories = request.GET['category'].split(',')
            products = products.filter(category__name__in=categories)
            categories = Category.objects.filter(name__in=categories)

        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    current_sorting = f'{sort}_{direction}'

    context = {
        'products': products,
        'search_term': query,
        'current_categories': categories,
        'current_sorting': current_sorting,
    }

    return render(request, 'products/products.html', context)


def product_detail(request, product_id):
    """ A view to show individual product details """

    product = get_object_or_404(Product, pk=product_id)

    context = {
        'product': product,
    }

    return render(request, 'products/product_detail.html', context)
```

## Add bag functionality
Add app
python3 manage.py startapp bag

In settings.py add 
'bag',
to INSTALLED APPS


In bag/views.py

from django.shortcuts import render

# Create your views here.

def view_bag(request):
    """ A view that renders the bag contents page """
    return render(request, 'bag/bag.html')

Create folder structure bag/templates/bag
within that folder create bag.html


copy contents of templates/home/index.html into bag/templates/bag/bag.html
within bag/templates/bag remove the contents of the block contents area.

copy contents of home/urls.py to new file bag/urls.py
edit it to read 
'''python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.view_bag, name='view_bag')
'''

In boutique_ado/urls.py add path('bag/', include('bag.urls')), to the urlpatterns section

In templates/base.html and templetes/includes/mobile-top-header.html
<li class="list-inline-item">
                        <a class="{% if grand_total %}text-info font-weight-bold{% else %}text-black{% endif %} nav-link" href="{% url 'view_bag' %}">
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


python3 manage.py runserver
to test


### Add content to bag template

in bag/templates/bag/bag.html
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
    <div class="overlay"></div>
    <div class="container mb-2">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">Shopping Bag</h2>
                <hr>
            </div>
        </div>

        <div class="row">
            <div class="col">
                {% if bag_items %}
                    <div class="table-responsive rounded"></div>
                {% else %}
                    <p class="lead mb-5">Your bag is empty.</p>
                    <a href="{% url 'products' %}" class="btn btn-outline-black rounded-0 btn-lg">
                        <span class="icon">
                            <i class="fas fa-chevron-left"></i>
                        </span>
                        <span class="text-uppercase">Keep Shopping</span>
                    </a>
                {% endif %}
            </div>
        </div>
    </div>
{% endblock %}

create new file bag/contexts.py

in thatfile add

'''python
from decimal import Decimal
from django.conf import settings

def bag_contents(request):

    bag_items = []
    total = 0
    product_count = 0

    if total < settings.FREE_DELIVERY_THRESHOLD:
        delivery = total * Decimal(settings.STANDARD_DELIVERY_PERCENTAGE / 100)
        free_delivery_delta = settings.FREE_DELIVERY_THRESHOLD - total
    else:
        delivery = 0
        free_delivery_delta = 0
    
    grand_total = delivery + total
    
    context = {
        'bag_items': bag_items,
        'total': total,
        'product_count': product_count,
        'delivery': delivery,
        'free_delivery_delta': free_delivery_delta,
        'free_delivery_threshold': settings.FREE_DELIVERY_THRESHOLD,
        'grand_total': grand_total,
    }

    return context
'''

in settings.py in the templates context processors section add
'bag.contexts.bag_contents',

at the bottom of the file add 
FREE_DELIVERY_THRESHOLD = 50
STANDARD_DELIVERY_PERCENTAGE = 10

in templates/base.html
change IF REQUIRED

<h4 class="logo-font my-1">Free delivery on orders over £{{ free_shipping_threshold }}!</h4> 
TO
<h4 class="logo-font my-1">Free delivery on orders over £{{ free_delivery_threshold }}!</h4> 

python3 manage.py runserver
to test

commit etc

## Adding products
in products/templates/products/product_detail.html add the form at the bottom of block content
'''html
<form class="form" action="" method="POST">
                        {% csrf_token %}
                        <div class="form-row">
                            <div class="col-12">
                                <p class="mt-3"><strong>Quantity:</strong></p>
                                <div class="form-group w-50">
                                    <div class="input-group">
                                        <input class="form-control qty_input" type="number" name="quantity" value="1" min="1" max="99" data-item_id="{{ product.id }}" id="id_qty_{{ product.id }}">
                                    </div>
                                </div>
                            </div>

                            <div class="col-12">
                                <a href="{% url 'products' %}" class="btn btn-outline-black rounded-0 mt-5">
                                    <span class="icon">
                                        <i class="fas fa-chevron-left"></i>
                                    </span>
                                    <span class="text-uppercase">Keep Shopping</span>
                                </a>
                                <input type="submit" class="btn btn-black rounded-0 text-uppercase mt-5" value="Add to Bag">
                            </div>
                            <input type="hidden" name="redirect_url" value="{{ request.path }}">
                        </div>
                    </form>

     
'''
 in static/base.css add
'''css
      .btn-outline-black {
    background: white;
    color: black !important; /* use important to override link colors for <a> elements */
    border: 1px solid black;
}
'''

test with  python3 manage.py runserver
