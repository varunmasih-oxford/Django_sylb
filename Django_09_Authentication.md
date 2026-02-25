# Django 09 Authentication

- Django’s built-in authentication system
- Login, Logout, and Registration views
- Using UserCreationForm and AuthenticationForm
- Restricting access with login_required decorator
- Permissions and Groups


# Django User Relationship (Project: edu | App: blog)

## 1. Project Structure

```
edu/
│
├── edu/
│   ├── settings.py
│   ├── urls.py
│
├── blog/
│   ├── models.py
│   ├── views.py
│   ├── admin.py
│
└── manage.py
```

---

## 2. What is User Relationship?

User relationship connects Django's built-in User model with your application models.

Example:

* One User → Many Blog Posts
* One Post → One Author

Relationship Type: **One-to-Many**

Implemented using `ForeignKey`.

---

## 3. Create User Relationship Model

Open:

```
blog/models.py
```

### Import Required Modules

```python
from django.db import models
from django.contrib.auth import get_user_model

User = get_user_model()
```

---

### Create Post Model

```python
class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)

    author = models.ForeignKey(
        User,
        on_delete=models.CASCADE
    )

    def __str__(self):
        return self.title
```

---

## 4. Explanation

### ForeignKey(User)

Each post is linked to one user.

Example:

```
User A → Post 1
User A → Post 2
User B → Post 3
```

One user can have multiple posts.

---

### on_delete=models.CASCADE

If a user is deleted:

* All related posts are deleted automatically.

---

## 5. Register Model in Admin

Open:

```
blog/admin.py
```

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

---

## 6. Create Database Tables

Run commands:

```
python manage.py makemigrations
```

```
python manage.py migrate
```

---

## 7. Create Superuser

```
python manage.py createsuperuser
```

Start server:

```
python manage.py runserver
```

Open admin panel:

```
http://127.0.0.1:8000/admin/
```

Add users and create posts from admin dashboard.

---

## 8. Display Posts in View

Open:

```
blog/views.py
```

```python
from django.shortcuts import render
from .models import Post

def all_posts(request):
    posts = Post.objects.all()
    return render(request, "posts.html", {"posts": posts})
```

---

## 9. Show Author in Template

```
templates/posts.html
```

```html
{% for post in posts %}
<h2>{{ post.title }}</h2>
<p>{{ post.content }}</p>
<p>Author: {{ post.author.username }}</p>
{% endfor %}
```

---

## 10. Show Logged-in User Posts

```python
def my_posts(request):
    posts = Post.objects.filter(author=request.user)
    return render(request, "posts.html", {"posts": posts})
```

This displays only the posts created by the logged-in user.

---

## 11. Relationship Types in Django

| Relationship | Field           | Example        |
| ------------ | --------------- | -------------- |
| One to Many  | ForeignKey      | User → Posts   |
| One to One   | OneToOneField   | User → Profile |
| Many to Many | ManyToManyField | Post ↔ Tags    |

---

## 12. Best Practice

Always use:

```python
from django.contrib.auth import get_user_model
User = get_user_model()
```

Instead of importing User directly.

Reason:
Supports custom user models in future projects.

---

## 13. Workflow Summary

```
Create Model
      ↓
Add ForeignKey(User)
      ↓
Register in Admin
      ↓
Run Migrations
      ↓
Create User
      ↓
Create Post Linked to User
```

---
