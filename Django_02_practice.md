# Make it saperatly

# Django Assignment: Project-Level and Multiple Apps
Assignment Objective

Create a Django project that demonstrates your understanding of:

Django projects and apps
App registration
Project-level URL routing
App-level URL routing
Views
HttpResponse
Connecting URLs to views
Testing multiple routes in the browser

You will create one Django project, two Django apps (app1 and app2), and a total of 15 routes and 15 views.

Part 1: Create the Django Project

Create a project named:

django-admin startproject myproject
cd myproject


Run the development server:

python manage.py runserver


Verify that Django is working by opening:

http://127.0.0.1:8000/

Part 2: Create Two Django Apps

Inside the project directory, create:

python manage.py startapp app1
python manage.py startapp app2


Your structure should look similar to:

myproject/
│
├── manage.py
│
├── myproject/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── app1/
│   ├── migrations/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
│
└── app2/
    ├── migrations/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    └── views.py

Part 3: Register Both Apps

Open:

myproject/settings.py


Add both applications to INSTALLED_APPS:

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'app1',
    'app2',
]

Requirement

Both app1 and app2 must be successfully registered.

Part 4: Create 5 Project-Level Routes and 5 Views

For this part, you will create 5 views directly in the project-level myproject package.

Create:

myproject/views.py


You need to create these five views:

home
about
contact
services
profile

Each view should return a different HttpResponse.

For example:

from django.http import HttpResponse

def home(request):
    return HttpResponse("This is the Home Page")


Create the remaining four views yourself.

Project-Level URLs

Open:

myproject/urls.py


Create 5 routes that connect to the 5 project-level views.

Your routes should be:

/
 /about/
/contact/
/services/
/profile/

Expected Result
Route	View
/	home
/about/	about
/contact/	contact
/services/	services
/profile/	profile
Part 5: Create 5 URLs and 5 Views in app1

Now work inside app1.

Create:

app1/urls.py


Create 5 views in:

app1/views.py


Use the following view names:

dashboard
students
courses
teachers
results

Each view should return a different HttpResponse.

For example:

from django.http import HttpResponse

def dashboard(request):
    return HttpResponse("App1 Dashboard")


Create the other four views in the same way.

App1 URLs

Create 5 routes in:

app1/urls.py


Use these routes:

app1/dashboard/
app1/students/
app1/courses/
app1/teachers/
app1/results/


Connect each URL to its corresponding view.

Part 6: Connect app1 to the Project

Open:

myproject/urls.py


Use include() to connect the app1 URLs.

Conceptually, the project should route:

app1/... → app1/urls.py


For example, when a user visits:

http://127.0.0.1:8000/app1/dashboard/


Django should:

myproject/urls.py
        ↓
app1/urls.py
        ↓
dashboard view
        ↓
HttpResponse

Part 7: Create 5 URLs and 5 Views in app2

Now work inside app2.

Create:

app2/urls.py


Create 5 views in:

app2/views.py


Use these view names:

home
products
orders
customers
reports

Each view should return a different HttpResponse.

App2 URLs

Create 5 routes:

app2/home/
app2/products/
app2/orders/
app2/customers/
app2/reports/


Connect each URL to the appropriate view.

Part 8: Connect app2 to the Project

Use include() in:

myproject/urls.py


to connect app2/urls.py.

The routing should work like:

app2/... → app2/urls.py


For example:

http://127.0.0.1:8000/app2/products/


should execute the products view from app2.

Final Route Requirements

Your project must contain exactly these groups of routes.

Project-Level — 5 Routes
/
/about/
/contact/
/services/
/profile/

App1 — 5 Routes
/app1/dashboard/
/app1/students/
/app1/courses/
/app1/teachers/
/app1/results/

App2 — 5 Routes
/app2/home/
/app2/products/
/app2/orders/
/app2/customers/
/app2/reports/


Total: 15 routes and 15 views.

Submission Requirements

Submit the complete Django project with:

 Django project named myproject
 app1 created
 app2 created
 Both apps registered in settings.py
 myproject/views.py created
 5 project-level views
 5 project-level URLs
 app1/views.py containing 5 views
 app1/urls.py containing 5 URLs
 app2/views.py containing 5 views
 app2/urls.py containing 5 URLs
 app1 connected using include()
 app2 connected using include()
 All 15 URLs tested in the browser
 Each URL displays the correct HttpResponse
Bonus Challenge

Add a unique message to every view. For example:

Welcome to the Home Page
Welcome to the About Page
Welcome to the Students Page
Welcome to the Products Page


This will make it easy to demonstrate that each URL is connected to the correct view.
