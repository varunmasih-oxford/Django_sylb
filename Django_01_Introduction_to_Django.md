# Django 01 Introduction to Django

- What is Django and why use it
- Django architecture (MVT – Model View Template)
- Setting up Python and Django environment
- Creating your first Django project
- Understanding manage.py and project structure
---

# Django Basics – Explanation with Examples

## 1. What is Django and Why Use It?

### What is Django?

Django is a **high-level Python web framework** used to build web applications quickly and efficiently.

Django helps developers create websites such as blogs, social media platforms, and e-commerce applications without writing everything from scratch.

Django follows the principle of **“batteries included”**, meaning it provides built-in features such as:

* Authentication system
* Admin panel
* Database handling (ORM)
* Security features

### Example

If you want to build a blog application:

* Without Django, you must manually handle database connections, user authentication, and routing.
* With Django, most of these features are already available.

### Why Use Django?

* Rapid development
* Secure by default
* Highly scalable
* Written in Python (easy to read and maintain)

---

## 2. Django Architecture (MVT – Model View Template)

Django follows the **MVT architecture**.

### MVT Components

* **Model** – Handles database structure and data
* **View** – Contains business logic
* **Template** – Handles presentation (HTML)

---

### Model

The Model defines the database schema using Python classes.

Example:

```python
# models.py
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=50)
    age = models.IntegerField()
```

This creates a `Student` table in the database.

---

### View

The View handles requests and returns responses.

Example:

```python
# views.py
from django.http import HttpResponse

def home(request):
    return HttpResponse("Welcome to Django")
```

---

### Template

Templates are HTML files used to display data.

Example:

```html
<!-- home.html -->
<h1>Hello {{ name }}</h1>
```

---

### MVT Workflow

1. User sends a request
2. View receives the request
3. Model interacts with the database
4. Template renders data
5. Response is sent back to the user

---

## 3. Setting Up Python and Django Environment

### Step 1: Install Python

Check Python installation:

```bash
python --version
```

If not installed, download it from:
[https://www.python.org](https://www.python.org)

---

### Step 2: Create Virtual Environment

```bash
python -m venv env
```

Activate the environment:

**Windows**

```bash
env\Scripts\activate
```

**Mac/Linux**

```bash
source env/bin/activate
```

---

### Step 3: Install Django

```bash
pip install django
```

Verify installation:

```bash
django-admin --version
```

---

## 4. Creating Your First Django Project

### Create a Django Project

```bash
django-admin startproject myproject
```

### Project Structure

```text
myproject/
│── manage.py
│── myproject/
    │── __init__.py
    │── settings.py
    │── urls.py
    │── asgi.py
    │── wsgi.py
```

---

### Run Development Server

```bash
python manage.py runserver
```

Open browser and visit:
[http://127.0.0.1:8000/](http://127.0.0.1:8000/)

---

## 5. Understanding manage.py and Project Structure

### manage.py

`manage.py` is a command-line utility used to manage Django projects.

Common commands:

```bash
python manage.py runserver
python manage.py startapp appname
python manage.py makemigrations
python manage.py migrate
```

---

### settings.py

Contains project configuration such as:

* Installed apps
* Database configuration
* Middleware

Example:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
]
```

---

### urls.py

Maps URLs to views.

Example:

```python
from django.urls import path
from .views import home

urlpatterns = [
    path('', home),
]
```

---
