# Django 02 Django Basics

- Understanding Django Apps
- Running the development server
- URL dispatcher and urls.py
- Views and HTTP response basics
- Creating your first Django app


---

## 1. Understanding Django Apps

In Django, a **project** is the entire website, while **apps** are individual components that handle specific functionality.

**Examples:**

| Feature          | Django App |
| ---------------- | ---------- |
| Blog system      | `blog`     |
| User accounts    | `accounts` |
| Product listings | `store`    |

Each app is modular and usually contains:

* models
* views
* urls
* templates

---

## 2. Create a Project and Run the Development Server

### Step 1: Create a Django project

```bash
django-admin startproject myproject
cd myproject
```

### Step 2: Run the server

```bash
python manage.py runserver
```

Open your browser and go to:

```
http://127.0.0.1:8000
```

You should see the default Django welcome page.

---

## 3. Creating Your First Django App

Inside the project directory, run:

```bash
python manage.py startapp main
```

Your project structure will now look like:

```
myproject/
│
├── manage.py
├── myproject/
│   ├── settings.py
│   ├── urls.py
│
└── main/
    ├── views.py
    ├── models.py
    ├── admin.py
```

---

### Step 4: Register the App

Open **`myproject/settings.py`** and add your app to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'main',
]
```

---

## 4. Views and HTTP Response Basics

A **view** is a Python function that handles a request and returns a response.

Open **`main/views.py`**:

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello, this is my first Django page!")
```

Here:

* `request` represents the user’s request
* `HttpResponse` is the response sent back to the browser

---

## 5. URL Dispatcher (`urls.py`)

Django uses URL patterns to decide which view should handle a request.

We will use:

1. A project-level `urls.py`
2. An app-level `urls.py`

---

### Step 5: Create App URLs

Create a new file: **`main/urls.py`**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

---

### Step 6: Connect App URLs to the Project

Open **`myproject/urls.py`**:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('main.urls')),
]
```

---

## Test the Application

Run the server again:

```bash
python manage.py runserver
```

Visit:

```
http://127.0.0.1:8000/
```

You should see:

Hello, this is my first Django page!

---

## How Everything Connects

1. The browser requests a URL (e.g., `/`)
2. Django checks `myproject/urls.py`
3. The request is passed to `main.urls`
4. The matching view function runs
5. The view returns an `HttpResponse`
6. The browser displays the response

---

## Add Another Page

### In `main/views.py`

```python
def about(request):
    return HttpResponse("This is the About Page")
```

### In `main/urls.py`

```python
urlpatterns = [
    path('', views.home, name='home'),
    path('about/', views.about, name='about'),
]
```

Now open:

```
http://127.0.0.1:8000/about/
```

---


