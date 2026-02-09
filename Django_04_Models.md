# Django 04 Models

- Introduction to ORM (Object Relational Mapping)
- Creating Models and Model Fields
- Performing Migrations
- Interacting with the Database using ORM
- Using the Django shell for database queries
---

## 1. Introduction to ORM (Object Relational Mapping)

Django uses an **ORM (Object Relational Mapper)** to communicate with the database using Python code instead of SQL.

| Database Concept | Django Equivalent |
| ---------------- | ----------------- |
| Table            | Model             |
| Row              | Object            |
| Column           | Model Field       |

This allows developers to work with databases using Python classes and objects.

---

## 2. Creating Models and Model Fields

Models are created inside the `models.py` file of a Django app.

Example: `main/models.py`

```python
from django.db import models

class Course(models.Model):
    title = models.CharField(max_length=100)
    instructor = models.CharField(max_length=100)
    duration_weeks = models.IntegerField()
    price = models.FloatField()
    is_active = models.BooleanField(default=True)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

### Common Model Fields

| Field Type      | Purpose         |
| --------------- | --------------- |
| `CharField`     | Short text      |
| `TextField`     | Long text       |
| `IntegerField`  | Whole numbers   |
| `FloatField`    | Decimal numbers |
| `BooleanField`  | True/False      |
| `DateField`     | Date only       |
| `DateTimeField` | Date and time   |
| `EmailField`    | Email address   |
| `ImageField`    | Image upload    |
| `FileField`     | File upload     |

After creating a model, Django treats it like a database table.

---

## 3. Performing Migrations

Whenever you create or modify models, you must run migrations.

### Step 1: Create Migration Files

```bash
python manage.py makemigrations
```

### Step 2: Apply Migrations

```bash
python manage.py migrate
```

These commands create and update database tables automatically.

---

## 4. Interacting with the Database using ORM

Django ORM allows CRUD operations (Create, Read, Update, Delete).

### Create a Record

```python
from main.models import Course

Course.objects.create(
    title="Django for Beginners",
    instructor="Varun",
    duration_weeks=6,
    price=2999
)
```

### Retrieve Records

```python
Course.objects.all()
Course.objects.get(id=1)
```

### Filter Records

```python
Course.objects.filter(is_active=True)
Course.objects.filter(price__lt=3000)
```

### Update a Record

```python
course = Course.objects.get(id=1)
course.price = 3499
course.save()
```

### Delete a Record

```python
course = Course.objects.get(id=1)
course.delete()
```

---

## 5. Using the Django Shell for Database Queries

Django provides an interactive shell to test ORM queries.

Start the shell:

```bash
python manage.py shell
```

Import the model:

```python
from main.models import Course
```

Run queries:

```python
Course.objects.all()
Course.objects.count()
Course.objects.filter(instructor="Varun")
```

Exit the shell:

```python
exit()
```

---


