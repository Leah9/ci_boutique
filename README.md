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
