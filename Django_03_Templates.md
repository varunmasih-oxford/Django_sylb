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

Static files include CSS, JavaScript, and images.

### Create Static Folder

```
main/
│
├── static/
│   └── main/
│       └── style.css
```

### Add CSS File

```css
body {
    font-family: Arial;
    background-color: #f2f2f2;
}
```

### Load Static in Template

At the top of `base.html`:

```html
{% load static %}
<link rel="stylesheet" href="{% static 'main/style.css' %}">
```

---

