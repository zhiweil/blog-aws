---
title: django
tags: [python]
keywords: python
last_updated: March 27, 2019
summary: "django is a python frame for web development"
sidebar: mydoc_sidebar
permalink: python-django.html
folder: python
---

## Basics
Create a python project and setup virtualenv for it. Then run the following 
command to install django and create a project.

```bash
pip install django==2.1.5
django-admin startproject myproj .
# start up local server, which runs at http://localhost:8000
python manage.py runserver
```

* Each django project consists of multiple django apps. An app can be added to project by:
```bash
python manage.py startapp products
```

## Add page to APP
* Add page method to APP's views.py.
    ```python
    from django.http import HttpResponse
    from django.shortcuts import render


    def index(request):
        return HttpResponse('Hello World')
    ```
* Add URL mapping to APP's urls.py
    ```python
    from django.urls import path
    from . import views
    
    urlpatterns = [
        path('', views.index),
        path('new/', views.new_product)
    ]
    ```
* (OPTIONAL) Add URL mapping to PROJECT'S urls.py. You only need to do this once for each APP.
    ```python
    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('products/', include('products.urls')),
    ]
    ```
    
## DB migration
To add a DB table model to APP, and migrate DB engine to create the table.
* Create table in APP's models.py.
```python
from django.db import models


class Product(models.Model):
    name = models.CharField(max_length=255)
    weight = models.FloatField()
    age = models.IntegerField()
    image_url = models.CharField(max_length=2083)
```

* (OPTIONAL) Add APP's configuration class **(products.apps.ProductsConfig)** to PROJECT's settings.py.
    ```python
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'products.apps.ProductsConfig',
    ]
     ```

* Run the following commands to create and apply the new migration.
```bash
python manage.py makemigrations
python manage.py migrate
```

## Admin Console
* Create a superuser account.
    ```bash
    python manage.py createsuperuser
    ```
    
* Load admin console into a browser by address: http://localhost:8000/admin

* Add APP to admin console by adding APP's DB models to APP's admin.py. Now you should be
    able to mange the "Product" table from admin console.
    ```python
    from django.contrib import admin
    from .models import Product
    
    admin.site.register(Product)
    ```
    To customize the admin console for "Product", you could create a ProductAdmin class as follows.
    ```python
    from django.contrib import admin
    from .models import Product, Order
    
    class ProductAdmin(admin.ModelAdmin):
        list_display = ('name', 'price', 'stock')
    
    admin.site.register(Product, ProductAdmin)
    ```
    
## Template
* Create a folder under APP by name "templates".
* Add a HTML file to the new folder, such as "index.html"
* Change the view functions in views.py to render the new HTML file.
    ```python
    from django.shortcuts import render
    
    def index(request):
        return render(request, 'index.html')
    ```
* Import DB Model class into views.py, query objects of the Model and pass objects to template.
    ```python
    from django.shortcuts import render
    from .models import Product
    
    def index(request):
        products = Product.objects.all()
        return render(request, 'index.html', {"products": products})
    ```
    
* In the template file (index.html), display the "products" parameter in a loop.
    ```pyhon 
    <h1>Products</h1>
    
    <ul>
        { % for product in products % }
            <li>{ { product.name } } (${ { product.price } })</li>
        { % endfor % }
    </ul>
    ```
    
### Template TAG
"{ %  for product in products % }" - template tags are used to execute some logic such as loops and conditions.

### Evaluation TAG
"{ { product.name } }" - evaluation tags are used to render values on HTML page.

### Base Template
* Create "templates" directory under the PROJECT root and create a base HTML template file.
* Add a code blocks inside HTML body.
    ```html
    { % block content % }
    { % endblock % }
    ```
* Add this base template to PROJECT settings.py -> add to TEMPLATES list.
* All the APPs of the PROJECT reference the template by adding the following to the top of its own templates
```html
{ % extends 'base.html' % }
```