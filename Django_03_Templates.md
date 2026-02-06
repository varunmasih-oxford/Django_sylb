# Django 03 Templates

- Introduction to Django Templates
- Template syntax and variables
- Template filters and tags
- Template inheritance and includes
- Static files and the {% static %} tag

---

## 1. Introduction to Django Templates

Templates allow you to separate **design (HTML)** from **logic (Python views)**.

Instead of returning plain text from a view, we return an HTML file.

### Create a Templates Folder

Inside your app (`main`), create this structure:

```
main/
│
├── templates/
│   └── main/
│       └── home.html
```

### Update Settings

Open `settings.py` and make sure `TEMPLATES` has `'APP_DIRS': True` (it is True by default).

### Create `home.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>
    <h1>Welcome to Django Templates</h1>
</body>
</html>
```

### Update View

`main/views.py`

```python
from django.shortcuts import render


def home(request):
    return render(request, 'main/home.html')
```

---

## 2. Template Syntax and Variables

Templates can display dynamic data using **double curly braces**.

### Update View

```python

def home(request):
    context = {
        'name': 'Varun',
        'course': 'Django Basics'
    }
    return render(request, 'main/home.html', context)
```

### Update Template

```html
<h1>Hello {{ name }}</h1>
<p>You are learning {{ course }}</p>
```

---

## 3. Template Filters and Tags

### Filters

Filters modify variables.

```html
<p>{{ name|upper }}</p>
<p>{{ name|lower }}</p>
<p>{{ name|length }}</p>
```

### Tags

Tags add logic like loops and conditions.

```html
<ul>
{% for item in items %}
    <li>{{ item }}</li>
{% endfor %}
</ul>

{% if name == 'Varun' %}
<p>Welcome back, instructor!</p>
{% else %}
<p>Welcome student!</p>
{% endif %}
```

### Pass Data from View

```python
context = {
    'name': 'Varun',
    'items': ['Python', 'Django', 'HTML']
}
```

---

## 4. Template Inheritance and Includes

Template inheritance avoids repeating the same layout.

### Create Base Template

`templates/main/base.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
</head>
<body>
    <header>
        <h2>My Website</h2>
        <hr>
    </header>

    {% block content %}
    {% endblock %}

</body>
</html>
```

### Extend Base Template

`home.html`

```html
{% extends 'main/base.html' %}

{% block title %}Home{% endblock %}

{% block content %}
<h1>Hello {{ name }}</h1>
<p>Welcome to the Django template system.</p>
{% endblock %}
```

### Includes

Reusable components can be included.

`templates/main/navbar.html`

```html
<nav>
    <a href="/">Home</a> |
    <a href="/about/">About</a>
</nav>
```

Use it inside `base.html`:

```html
{% include 'main/navbar.html' %}
```

---

## 5. Static Files and the `{% static %}` Tag
This guide explains how to properly use static files in Django, including CSS, JavaScript, fonts, and images.

Topics covered:

1. Creating the Static Folder Structure
2. Configuring Static Settings in `settings.py`
3. Loading Static Files in Templates
4. Linking CSS Files
5. Linking JavaScript Files
6. Using Images in Templates
7. Using Custom Fonts

---

## 1. Creating the Static Folder Structure

Inside your Django app (example: `main`), create a static directory like this:

```
main/
│
├── static/
│   └── main/
│       ├── css/
│       │   └── style.css
│       ├── js/
│       │   └── script.js
│       ├── images/
│       │   └── logo.png
│       └── fonts/
│           └── custom-font.woff2
```

The second `main` folder is important. It prevents name conflicts between apps.

---

## 2. Configuring Static Settings in `settings.py`

Open `settings.py` and ensure these settings exist:

```python
STATIC_URL = '/static/'

STATICFILES_DIRS = [
    BASE_DIR / "static",  # Optional global static folder
]

STATIC_ROOT = BASE_DIR / "staticfiles"  # Used when deploying
```

`STATICFILES_DIRS` is optional and used if you keep a project-level static folder.

Make sure `'django.contrib.staticfiles'` is included in `INSTALLED_APPS` (it is by default).

---

## 3. Loading Static Files in Templates

At the top of every HTML file where you want to use static files, add:

```html
{% load static %}
```

This enables Django’s static template tag.

---

## 4. Linking CSS Files

Inside your base template (`base.html`):

```html
{% load static %}
<link rel="stylesheet" href="{% static 'main/css/style.css' %}">
```

Now Django will correctly serve the CSS file.

---

## 5. Linking JavaScript Files

Place this before the closing `</body>` tag:

```html
<script src="{% static 'main/js/script.js' %}"></script>
```

---

## 6. Using Images in Templates

To display an image:

```html
<img src="{% static 'main/images/logo.png' %}" alt="Logo">
```

---

## 7. Using Custom Fonts

Define fonts inside your CSS file (`style.css`):

```css
@font-face {
    font-family: 'CustomFont';
    src: url('../fonts/custom-font.woff2') format('woff2');
}

body {
    font-family: 'CustomFont', Arial, sans-serif;
}
```

Make sure the path correctly points from the CSS folder to the fonts folder.

---

## 8. Static Files URL Behavior in Development

When `DEBUG = True`, Django automatically serves static files.

Run the server:

```bash
python manage.py runserver
```

Then open the browser and your CSS, JS, and images will load correctly.

---
