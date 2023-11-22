# Ex-04-Django-Models
# STUDENT DETAILS :
Name : P.Jeshwanth kumar


Department : AIML

Reference N0 : 23002519
# AIM : 
Display user's details using template-view-model in django framework.
# STEP 1 :
Create django project and app using the following commands:

Django-admin startproject mymodels

Python manage.py startapp myapp
# STEP 2 :
```
create a user_profile models in model.py
from django.db import models
from django.contrib.auth.models import User

class UserProfile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    email = models.EmailField()
```
# STEP 3 :
Add the models in the admin interface using the code in admin.py
```
from django.contrib import admin

# Register your models here.
from .models import UserProfile

admin.site.register(UserProfile)
```
# STEP 4 :
Write the function based view to render the data from the models to the template in view.py
```
from django.shortcuts import render
from django.contrib.auth.decorators import login_required
from .models import UserProfile

@login_required
def user_profile(request):
    user = request.user
    user_profile, created = UserProfile.objects.get_or_create(user=user)

    context = {
        "user": user,
        "user_profile": user_profile,
        'firstname': user_profile.user.first_name,  # Access first name through the related User model
        "lastname": user_profile.user.last_name,  # Access last name through the related User model
    }
    return render(request, 'myapp/user_profiles.html', context)
```
 # STEP 5 :
Setup the url path for the templates using urls.py
```
"""mymodels URL Configuration

The urlpatterns list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/4.1/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path
from myapp import views
from myapp.views import user_profile

urlpatterns = [
    path("admin/", admin.site.urls),
    path('profile/', views.user_profile, name='user_profile'),
]
```
# STEP 6:
In settings.py file add the app created.

Now do the migrations process to initiate and save the models

Python mange.py makemigrations
Python manage.py migrate

Create a template as user_profiles.html
```
<!DOCTYPE html>
<html>
<head>
    <title>User Profile</title>
</head>
<body>
    <h1>User Profile</h1>
    <p><strong>Username:</strong> {{ user.username }}</p>
    <p><strong>First name:</strong> {{ firstname }}</p>
    <p><strong>Last name:</strong> {{ lastname }}</p>
    <p><strong>Email :</strong> {{ user.email }}</p>
    <p><strong>Custom Profile Data:</strong> {{ user_profile.custom_field }}</p>
</body>
</html>
```
# STEP 7:
Run the program using the command

Python manage.py runserver 8000

In the admin/ page you can view the models created

And  in the user_profile template page you can see the profile page of the user.

# OUTPUT : 
![image](https://github.com/Jeshwanthkumarpayyavula/ODD2023-WT-Ex-04-Django-Models/assets/145742402/ddb85349-d92b-4034-b757-170f22fbf314)
![image](https://github.com/Jeshwanthkumarpayyavula/ODD2023-WT-Ex-04-Django-Models/assets/145742402/f8cbebed-231a-4302-ad43-a8501aa62146)



# RESULT : 
User Profile displayed successfully using django framework.
