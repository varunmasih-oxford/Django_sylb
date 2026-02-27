# Django Blog Website with Frontend Integration and CRUD Operations

This project demonstrates:

-   Frontend integration using Django templates
-   Blog website layout
-   Create Blog
-   Read Blog
-   Update Blog
-   Delete Blog

------------------------------------------------------------------------

## 1. Create Django Project

``` bash
django-admin startproject myblog
cd myblog
```

Create app:

``` bash
python manage.py startapp blog
```

Project Structure:

    myblog/
    │
    ├── myblog/
    │   ├── settings.py
    │   ├── urls.py
    │
    ├── blog/
    │   ├── models.py
    │   ├── views.py
    │   ├── urls.py
    │
    └── manage.py

------------------------------------------------------------------------

## 2. Register App

### settings.py

``` python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```

------------------------------------------------------------------------

## 3. Configure Templates

Create:

    myblog/templates/

Update settings.py:

``` python
TEMPLATES = [
{
    'DIRS': [BASE_DIR / 'templates'],
}
]
```

------------------------------------------------------------------------

## 4. Create Blog Model

### blog/models.py

``` python
from django.db import models

class Blog(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    image = models.ImageField(upload_to='blog_images/', null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

------------------------------------------------------------------------

## 5. Run Migration

``` bash
python manage.py makemigrations
python manage.py migrate
```

------------------------------------------------------------------------

## 6. Create Superuser

``` bash
python manage.py createsuperuser
python manage.py runserver
```

------------------------------------------------------------------------

## 7. Register Model

### blog/admin.py

``` python
from django.contrib import admin
from .models import Blog

admin.site.register(Blog)
```

------------------------------------------------------------------------

## 8. Create Form

### blog/forms.py

``` python
from django import forms
from .models import Blog

class BlogForm(forms.ModelForm):
    class Meta:
        model = Blog
        fields = ['title', 'content', 'image']
```

------------------------------------------------------------------------

## 9. CRUD Views

### blog/views.py

``` python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Blog
from .forms import BlogForm

def blog_list(request):
    blogs = Blog.objects.all()
    return render(request, 'blog_list.html', {'blogs': blogs})

def blog_create(request):
    form = BlogForm(request.POST or None, request.FILES or None)
    if form.is_valid():
        form.save()
        return redirect('blog_list')
    return render(request, 'blog_form.html', {'form': form})

def blog_update(request, id):
    blog = get_object_or_404(Blog, id=id)
    form = BlogForm(request.POST or None, request.FILES or None, instance=blog)
    if form.is_valid():
        form.save()
        return redirect('blog_list')
    return render(request, 'blog_form.html', {'form': form})

def blog_delete(request, id):
    blog = get_object_or_404(Blog, id=id)
    if request.method == "POST":
        blog.delete()
        return redirect('blog_list')
    return render(request, 'blog_delete.html', {'blog': blog})
```

------------------------------------------------------------------------

## 10. URLs

### blog/urls.py

``` python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.blog_list, name='blog_list'),
    path('add/', views.blog_create, name='blog_create'),
    path('update/<int:id>/', views.blog_update, name='blog_update'),
    path('delete/<int:id>/', views.blog_delete, name='blog_delete'),
]
```

### myblog/urls.py

``` python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

------------------------------------------------------------------------

## 11. Templates

Create:

    templates/
    │
    ├── base.html
    ├── blog_list.html
    ├── blog_form.html
    └── blog_delete.html

### base.html

``` html
<!DOCTYPE html>
<html>
<head>
<title>Django Blog</title>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

<nav class="navbar navbar-dark bg-dark">
<div class="container">
<a class="navbar-brand" href="/">My Blog</a>
<a href="/add/" class="btn btn-success">Add Blog</a>
</div>
</nav>

<div class="container mt-4">
{% block content %}{% endblock %}
</div>

</body>
</html>
```

### blog_list.html

``` html
{% extends 'base.html' %}

{% block content %}
<h2>All Blogs</h2>

{% for blog in blogs %}
<div class="card mb-3">
<div class="card-body">
<h4>{{ blog.title }}</h4>
<p>{{ blog.content }}</p>

<a href="{% url 'blog_update' blog.id %}" class="btn btn-primary">Edit</a>
<a href="{% url 'blog_delete' blog.id %}" class="btn btn-danger">Delete</a>
</div>
</div>
{% endfor %}

{% endblock %}
```

### blog_form.html

``` html
{% extends 'base.html' %}

{% block content %}
<h2>Blog Form</h2>

<form method="POST" enctype="multipart/form-data">
{% csrf_token %}
{{ form.as_p }}
<button class="btn btn-success">Save</button>
</form>

{% endblock %}
```

### blog_delete.html

``` html
{% extends 'base.html' %}

{% block content %}
<h3>Are you sure you want to delete?</h3>

<form method="POST">
{% csrf_token %}
<button class="btn btn-danger">Delete</button>
<a href="/" class="btn btn-secondary">Cancel</a>
</form>

{% endblock %}
```

------------------------------------------------------------------------

## 12. Media Setup

### settings.py

``` python
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

### myblog/urls.py

``` python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```
