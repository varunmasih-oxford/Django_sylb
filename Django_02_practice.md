# Make it saperatly
# Django Assignment: Project-Level and Multiple Apps

## Objective

Create a Django project that demonstrates your understanding of:

- Django Projects
- Django Apps
- App Registration
- URL Routing
- Project-level URLs
- App-level URLs
- Views
- `HttpResponse`
- `include()`

You will create:

- 1 Django Project
- 2 Django Apps: `app1` and `app2`
- 5 Project-level URLs and Views
- 5 `app1` URLs and Views
- 5 `app2` URLs and Views

**Total: 15 URLs and 15 Views**

---

## Part 1: Create the Django Project

Create a Django project named `myproject`.

```bash
django-admin startproject myproject
cd myproject


Run the development server:

python manage.py runserver


Open the following URL in your browser:

http://127.0.0.1:8000/

Part 2: Create Two Django Apps

Create two apps named app1 and app2.

python manage.py startapp app1
python manage.py startapp app2


The project structure should look similar to:

myproject/
│
├── manage.py
│
├── myproject/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── views.py
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
│   ├── urls.py
│   └── views.py
│
└── app2/
    ├── migrations/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    ├── urls.py
    └── views.py

Part 3: Register Both Apps

Open:

myproject/settings.py


Add app1 and app2 to INSTALLED_APPS.

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

Both app1 and app2 must be registered successfully.

Part 4: Project-Level Views and URLs

Create:

myproject/views.py


Create 5 views:

home
about
contact
services
profile

Each view must return a different HttpResponse.

Example:

from django.http import HttpResponse

def home(request):
    return HttpResponse("This is the Home Page")


Create the other four views in the same way.

Project-Level URLs

Open:

myproject/urls.py


Create 5 project-level routes.

Required routes:

/
 /about/
/contact/
/services/
/profile/


The routes should be connected as follows:

URL	View
/	home
/about/	about
/contact/	contact
/services/	services
/profile/	profile
Part 5: App1 Views and URLs

Create 5 views inside:

app1/views.py


Required view names:

dashboard
students
courses
teachers
results

Example:

from django.http import HttpResponse

def dashboard(request):
    return HttpResponse("App1 Dashboard")


Create the remaining four views in the same way.

App1 URLs

Create:

app1/urls.py


Create the following 5 routes:

dashboard/
students/
courses/
teachers/
results/


The final URLs should be:

/app1/dashboard/
/app1/students/
/app1/courses/
/app1/teachers/
/app1/results/


Connect each URL to its corresponding view.

Part 6: Connect App1 to the Project

Open:

myproject/urls.py


Use include() to connect app1/urls.py.

The routing should work like this:

Browser
   ↓
myproject/urls.py
   ↓
app1/urls.py
   ↓
app1 view
   ↓
HttpResponse


For example:

http://127.0.0.1:8000/app1/dashboard/


should display the response from the dashboard view.

Part 7: App2 Views and URLs

Create 5 views inside:

app2/views.py


Required view names:

home
products
orders
customers
reports

Example:

from django.http import HttpResponse

def home(request):
    return HttpResponse("App2 Home Page")


Create the remaining four views in the same way.

App2 URLs

Create:

app2/urls.py


Create the following 5 routes:

home/
products/
orders/
customers/
reports/


The final URLs should be:

/app2/home/
/app2/products/
/app2/orders/
/app2/customers/
/app2/reports/


Connect each URL to its corresponding view.

Part 8: Connect App2 to the Project

Open:

myproject/urls.py


Use include() to connect app2/urls.py.

The routing should work like this:

Browser
   ↓
myproject/urls.py
   ↓
app2/urls.py
   ↓
app2 view
   ↓
HttpResponse


For example:

http://127.0.0.1:8000/app2/products/


should display the response from the products view.

Final Route Requirements
Project-Level Routes — 5
/
/about/
/contact/
/services/
/profile/

App1 Routes — 5
/app1/dashboard/
/app1/students/
/app1/courses/
/app1/teachers/
/app1/results/

App2 Routes — 5
/app2/home/
/app2/products/
/app2/orders/
/app2/customers/
/app2/reports/

Total
5 Project Views
+
5 App1 Views
+
5 App2 Views
=
15 Views

5 Project URLs
+
5 App1 URLs
+
5 App2 URLs
=
15 URLs

Submission Requirements
 Create Django project myproject
 Create app1
 Create app2
 Register app1 in settings.py
 Register app2 in settings.py
 Create myproject/views.py
 Create 5 project-level views
 Create 5 project-level URLs
 Create 5 views in app1
 Create 5 URLs in app1/urls.py
 Create 5 views in app2
 Create 5 URLs in app2/urls.py
 Connect app1 using include()
 Connect app2 using include()
 Test all 15 URLs in the browser
 Make sure every URL displays a different HttpResponse
Bonus Challenge

Give every view a unique response.

For example:

Home Page
About Page
Contact Page
Services Page
Profile Page

App1 Dashboard
App1 Students
App1 Courses
App1 Teachers
App1 Results

App2 Home
App2 Products
App2 Orders
App2 Customers
App2 Reports


This will make it easy to verify that every URL is connected to the correct view.
