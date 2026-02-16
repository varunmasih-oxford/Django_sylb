# Django 06 Forms

- Introduction to Django Forms
- Working with forms.Form and forms.ModelForm
- Handling form submission and validation
- Rendering forms in templates
- Customizing form widgets

---

# Form in Django

# Django Student Registration Form 

---

## Step 1: Create Student Model

File: appDemo/models.py

```python
from django.db import models

class Student(models.Model):
    GENDER_CHOICES = [
        ('Male', 'Male'),
        ('Female', 'Female'),
        ('Other', 'Other'),
    ]

    full_name = models.CharField(max_length=200)
    email = models.EmailField(unique=True)
    phone = models.CharField(max_length=15)
    age = models.IntegerField()
    gender = models.CharField(max_length=10, choices=GENDER_CHOICES)
    course = models.CharField(max_length=100)
    address = models.TextField()
    date_of_birth = models.DateField()
    registration_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.full_name
````

---

## Step 2: Run Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## Step 3: Create Form

File: appDemo/forms.py

```python
from django import forms
from .models import Student

class StudentRegistrationForm(forms.ModelForm):
    class Meta:
        model = Student
        fields = '__all__'
        widgets = {
            'date_of_birth': forms.DateInput(attrs={'type': 'date'}),
        }

    def clean_age(self):
        age = self.cleaned_data.get('age')
        if age < 5:
            raise forms.ValidationError("Age must be greater than 5")
        return age
```

---

## Step 4: Create Views

File: appDemo/views.py

```python
from django.shortcuts import render, redirect
from .forms import StudentRegistrationForm

def register_student(request):
    if request.method == "POST":
        form = StudentRegistrationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('registration_success')
    else:
        form = StudentRegistrationForm()

    return render(request, 'register_student.html', {'form': form})


def registration_success(request):
    return render(request, 'registration_success.html')
```

---

## Step 5: Configure URLs

File: appDemo/urls.py

```python
from django.urls import path
from . import views

urlpatterns = [
    path('register/', views.register_student, name='register_student'),
    path('success/', views.registration_success, name='registration_success'),
]
```

File: projectDemo/urls.py

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('appDemo.urls')),
]
```

---

## Step 6: Create Templates

Folder Structure:

```
appDemo/
    templates/
        register_student.html
        registration_success.html
```

---

### register_student.html

File: appDemo/templates/register_student.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>Student Registration</title>
</head>
<body>

    <h2>Student Registration Form</h2>

    <form method="POST">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Register</button>
    </form>

</body>
</html>
```

---

### registration_success.html

File: appDemo/templates/registration_success.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>Success</title>
</head>
<body>

    <h2>Registration Successful!</h2>
    <a href="/register/">Register Another Student</a>

</body>
</html>
```

---

## Step 7: Run Server

```bash
python manage.py runserver
```

Open in browser:

```
http://127.0.0.1:8000/register/
```

---

## How It Works

| Step            | Explanation                   |
| --------------- | ----------------------------- |
| GET Request     | Empty form loads              |
| POST Request    | Form submits data             |
| form.is_valid() | Validates form data           |
| form.save()     | Saves data to database        |
| redirect()      | Prevents duplicate submission |
| csrf_token      | Protects against CSRF attacks |

---

## Key Interview Concepts

* Difference between forms.Form and forms.ModelForm
* What is clean() and clean_fieldname()
* Why redirect after POST?
* What is CSRF?
* How Django handles validation

---

## Result

You now have a fully working Student Registration Form using Django ModelForm.

```
