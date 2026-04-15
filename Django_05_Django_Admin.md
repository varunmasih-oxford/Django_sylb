# Django 05: Django Admin

## 1. Enabling and Using Django Admin

### File: `project/settings.py`

Ensure required apps are installed:

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
````

---

### File: `project/urls.py`

Enable admin route:

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

---

### Terminal Commands

```bash
python manage.py migrate
python manage.py createsuperuser
```

---

### Access Admin Panel

```
http://127.0.0.1:8000/admin/
```

---

## 2. Registering Models with Admin Site

### File: `main/models.py`

```python
from django.db import models

class Course(models.Model):
    title = models.CharField(max_length=100)
    instructor = models.CharField(max_length=100)
    duration_weeks = models.IntegerField()
    price = models.FloatField()

    def __str__(self):
        return self.title
```

---

### File: `main/admin.py`

```python
from django.contrib import admin
from .models import Course

admin.site.register(Course)
```

---

## 3. Customizing the Admin Interface

### File: `main/admin.py`

```python
from django.contrib import admin
from .models import Course

class CourseAdmin(admin.ModelAdmin):
    list_display = ('title', 'instructor', 'duration_weeks', 'price')

admin.site.register(Course, CourseAdmin)
```

### Explanation

* `list_display` → Controls columns shown in admin list view

---

## 4. Adding Search and Filters in Admin

### File: `main/admin.py`

```python
from django.contrib import admin
from .models import Course

class CourseAdmin(admin.ModelAdmin):
    list_display = ('title', 'instructor', 'duration_weeks', 'price')
    search_fields = ('title', 'instructor')
    list_filter = ('duration_weeks', 'price')

admin.site.register(Course, CourseAdmin)
```

### Features

* `search_fields` → Adds search bar
* `list_filter` → Adds filter sidebar

---

## 5. Managing Users and Permissions

### Access in Admin Panel

* Users
* Groups
* Permissions

---

### Creating and Managing Users

* Add new users
* Set passwords
* Assign roles

---

### Important User Fields

* `is_staff` → Can access admin panel
* `is_superuser` → Full access

---

### Using Groups

Steps:

1. Go to **Groups**
2. Create group (e.g., Editors)
3. Assign permissions:

   * Add
   * Change
   * Delete
4. Add users to group

---

## 6. Custom Permissions (Optional)

### File: `main/models.py`

```python
class Course(models.Model):
    title = models.CharField(max_length=100)

    class Meta:
        permissions = [
            ("can_publish", "Can publish course"),
        ]
```

---

### Run Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## Summary

1. Enable admin in `settings.py` and `urls.py`
2. Run migrations and create superuser
3. Register model in `admin.py`
4. Customize admin using `ModelAdmin`
5. Add search and filters
6. Manage users and permissions

---

```
