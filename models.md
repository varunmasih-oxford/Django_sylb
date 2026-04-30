# Django 04 – Models

## 1. Introduction to ORM (Object Relational Mapping)

Django uses an **ORM (Object Relational Mapper)** to communicate with the database using Python code instead of raw SQL.

| Database Concept | Django Equivalent |
|------------------|-------------------|
| Table            | Model class       |
| Row              | Object instance   |
| Column           | Model field       |
| Query            | QuerySet          |

> Instead of writing `SELECT * FROM courses`, you write `Course.objects.all()` — Django handles the SQL for you.

---

## 2. Creating Models and Model Fields

Models are defined inside `models.py` of your Django app.

```python
# main/models.py

from django.db import models

class Course(models.Model):
    title           = models.CharField(max_length=100)
    instructor      = models.CharField(max_length=100)
    duration_weeks  = models.IntegerField()
    price           = models.FloatField()
    is_active       = models.BooleanField(default=True)
    created_at      = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

### Common Field Types

| Field Type      | Purpose                        |
|-----------------|--------------------------------|
| `CharField`     | Short text (requires max_length)|
| `TextField`     | Long text                      |
| `IntegerField`  | Whole numbers                  |
| `FloatField`    | Decimal numbers                |
| `BooleanField`  | True / False                   |
| `DateField`     | Date only                      |
| `DateTimeField` | Date and time                  |
| `EmailField`    | Email address                  |
| `ImageField`    | Image upload                   |
| `FileField`     | File upload                    |

---

## 3. Performing Migrations

Whenever you **create or modify** a model, run migrations to sync the database.

```bash
# Step 1 – Generate migration file
python manage.py makemigrations

# Step 2 – Apply migration to the database
python manage.py migrate
```

> Think of it as a two-step commit: `makemigrations` writes the plan, `migrate` executes it.

---

## 4. Interacting with the Database using ORM

Django ORM supports all four **CRUD** operations.

### Create

```python
from main.models import Course

Course.objects.create(
    title="Django for Beginners",
    instructor="Varun",
    duration_weeks=6,
    price=2999
)
```

### Read

```python
Course.objects.all()          # All records
Course.objects.get(id=1)      # Single record by primary key
```

### Filter

```python
Course.objects.filter(is_active=True)    # Active courses
Course.objects.filter(price__lt=3000)    # Price less than 3000
Course.objects.filter(title__contains="Django")  # Title contains word
```

### Update

```python
course = Course.objects.get(id=1)
course.price = 3499
course.save()
```

### Delete

```python
course = Course.objects.get(id=1)
course.delete()
```

---

## 5. Using the Django Shell

Django's interactive shell lets you test ORM queries live.

```bash
# Start the shell
python manage.py shell
```

```python
# Import your model
from main.models import Course

# Run queries
Course.objects.all()
Course.objects.count()
Course.objects.filter(instructor="Varun")

# Exit
exit()
```

---

## Quick Reference – Filter Lookups

| Lookup             | Example                                  | Meaning               |
|--------------------|------------------------------------------|-----------------------|
| `__lt`             | `price__lt=3000`                         | Less than             |
| `__gt`             | `price__gt=500`                          | Greater than          |
| `__lte`            | `price__lte=3000`                        | Less than or equal    |
| `__gte`            | `price__gte=500`                         | Greater than or equal |
| `__contains`       | `title__contains="Django"`               | Contains substring    |
| `__icontains`      | `title__icontains="django"`              | Case-insensitive      |
| `__startswith`     | `title__startswith="Dj"`                 | Starts with           |
| `__in`             | `id__in=[1, 2, 3]`                       | In a list             |


---


# Django 05 – Admin Panel

## Step 1: Create a Superuser

```bash
python manage.py createsuperuser
```

It will prompt you for:
- Username
- Email address
- Password

---

## Step 2: Register Your Model in `admin.py`

```python
# main/admin.py

from django.contrib import admin
from .models import Course

admin.site.register(Course)
```

---

## Step 3: Run the Server and Login

```bash
python manage.py runserver
```

Visit → `http://127.0.0.1:8000/admin`

Login with the superuser credentials you created in Step 1.

---

## Step 4: Performing CRUD in the Admin Panel

Once logged in, your `Course` model will be listed under the app name.

| Operation | How to do it in Admin |
|-----------|----------------------|
| Create    | Click "Add Course" → fill the form → Save |
| Read      | Click "Courses" → see the full list of records |
| Update    | Click any record → edit the fields → Save |
| Delete    | Open a record → scroll down → click the red Delete button |

---

## Step 5: Customize the Admin Display

By default, the list view shows `Course object (1)`. Use `ModelAdmin` to make it more useful.

```python
# main/admin.py

from django.contrib import admin
from .models import Course

@admin.register(Course)
class CourseAdmin(admin.ModelAdmin):
    list_display  = ('title', 'instructor', 'price', 'is_active')  # columns to show
    list_filter   = ('is_active', 'instructor')                     # right-side filter sidebar
    search_fields = ('title', 'instructor')                         # search bar at the top
    ordering      = ('-created_at',)                                # newest first
```

### `ModelAdmin` Options

| Option         | What it does                                      |
|----------------|---------------------------------------------------|
| `list_display` | Columns shown in the list view                    |
| `list_filter`  | Filter sidebar on the right                       |
| `search_fields`| Adds a search bar at the top                      |
| `ordering`     | Default sort order (`-` prefix = descending)      |

---

## When to Use Admin vs Frontend

| Task | Use Admin | Use Frontend |
|------|-----------|--------------|
| Testing your models | Yes | No |
| Managing data yourself | Yes | No |
| Letting normal users interact | No | Yes |
| Custom pages and forms | No | Yes |

> The admin panel gives you full CRUD out of the box the moment you register your model — no templates, no views, no URLs needed.

---

## Django Learning Order

```
Models → Migrations → Shell / Admin CRUD → Views → URLs → Templates → Forms
```

You can practice full CRUD right now using the admin panel before touching any frontend topics.

