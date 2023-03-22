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
