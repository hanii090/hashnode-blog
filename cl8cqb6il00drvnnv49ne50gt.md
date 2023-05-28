---
title: "How To Build Image Upload Full Stack Site Using python And Django"
seoTitle: "Image Uploader"
datePublished: Thu Sep 22 2022 07:24:50 GMT+0000 (Coordinated Universal Time)
cuid: cl8cqb6il00drvnnv49ne50gt
slug: how-to-build-image-upload-full-stack-site-using-python-and-django
canonical: https://dev.to/nothanii/how-to-build-image-upload-full-stack-site-using-python-and-django-2jhi
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1665701142020/fh0ppH554.png
tags: python, python3, full-stack, python-beginner, python-projects

---

## Set up 

First of all you have to install python and django in your Environment,Then You have To Create A superuser account Of Django

After All Setup you Have to Create Django Project Using This Cmd

django-admin startproject nameofyourproject .

now you have to start django server using this cmd
```
python manage.py runserver
```

Create a new django app by this cmd
```
python manage.py startapp nameofapp
```
in your root directory

 
![49xug3oe0j1xqs2q23hn.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663829785142/eEfX74xJG.png align="left")

now after opening setting in your editor look for installed apps and put the name of your django app name here like my app name is posts



![c25k4dajr8q1hr9qnwwf.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1663829820343/FU9qkas5h.jpg align="left")

now install pillow in your root folder
```
pip install pillow
```
create a model
in your django app
from **django.db** import models
 
```


 ##  Create your models here.
class Post (models.Model):
    title = models.CharField(max_length=200)
    image = models.ImageField(upload_to='images/')

    def __str__(self):
        return self.title

``` 
now make migrations using this cmd for the changing you've made

```
python manage.py makemigrations
``` 
now open **settins.py** file and put this code right after the static _url
```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')`
`import os
from pathlib import Path`
also in settings.py
Now Define Url Routes
In Your Django Directory
`from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('posts.urls')),
]
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL,document_root =settings.MEDIA_ROOT)`
and put this code in  in your django app urls.py
`from django.urls import path

from .views import HomePageView,CreatePostView


urlpatterns =[
    path('',HomePageView.as_view(),name='home'),
    path('post',CreatePostView.as_view(),name = 'add_post'),
]
``` 

creating **Views **in your django app


```
from django.views.generic import ListView,CreateView
from django.urls import reverse_lazy
from .forms import PostForm
from .models import Post


 ##  Create your views here.

class HomePageView(ListView):
    model = Post
    template_name = 'home.html'

class CreatePostView(CreateView):
    model = Post
    form_class = PostForm
    template_name = 'post.html'
    success_url = reverse_lazy('home')
``` 
Creating Templates For django app
make a folder in your root folder name templates
like this

![zoo3obhtxr6y7j4n68ct.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663830217270/LQT6Me2rW.png align="left")

 ## Home.html 



```
<!doctype html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>izna.</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
    <style>
        /* navbar */

        .navbar {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 60px;
            background: #1d1d1d;
            padding: 0 10vw;
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 9;
        }

        .brand {
            font-weight: 500;
        }

        .links-container {
            display: flex;
            list-style: none;
        }

        .link {
            text-transform: capitalize;
            color: #fff;
            text-decoration: none;
            margin: 0 10px;
            padding: 10px;
            position: relative;
        }

        .link:hover:not(.active) {
            opacity: 0.7;
        }

        .link.active::before,
        .seperator::before {
            content: '';
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 5px;
            height: 5px;
            border-radius: 50%;
            background: #fff;
        }

        .link.active::after,
        .seperator::after {
            content: '';
            position: absolute;
            bottom: 2px;
            left: 0;
            width: 100%;
            height: 1px;
            background: #fff;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            width: 100%;
            position: relative;
            background: #1d1d1d;
            color: #fff;
            font-family: 'roboto', sans-serif;
        }

        .hero-content {
            display: block;
            margin: 260px auto 20px;
            width: 100%;
            height: 40px;
            border-radius: 5px;
            padding: 10px;
            border: none;
            outline: none;
            text-transform: capitalize;
            width: 50%;
            font-family: 'Lobster', cursive;
            background: #ff3559;
            color: #fff;
        }

        @media (min-width: 768px) {
            .bd-placeholder-img-lg {
                font-size: 3.5rem;
            }
        }

        .footer {
            margin: 260px auto 20px;
            width: 100%;
            height: 30px;
            text-align: center;
            background: #ff3559;
            font-family: 'Lobster', cursive;
            line-height: 30px;
        }
    </style>
</head>

<body>

    <nav class="navbar">
        <h1 class="brand">izna.</h1>
        <div class="toggle-btn">
            <span></span>
            <span></span>
        </div>
        <ul class="links-container">
            <li class="links-item"><a href="#" class="link active">home</a></li>
        </ul>
    </nav>
    <br>

    <a href="{% url 'add_post' %}" class="hero-content">Upload your images Here</a>
    </br>
    <main>
        <div class="album py bg-light">
            <div class="container">
                <div class="row">
                    {% for post in object_list %}

                    <div class="col-md-2">
                        <p class="card-text">{{ post.title}}</p>
                        <div class="card mb-4 shadow-sm">
                            <img class="card-img-top" src="{{post.image.url}}" alt="{{post.title}}" width="200" height="200px">
                            </img>
                        </div>
                    </div>
                    {% endfor %}
                </div>
            </div>


    </main>
    <footer class="footer">&copy; 2021 Copyright @hAnii all right reserved</footer>
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
</body>

</html>
``` 
## post.html


```
<h1> Create a Post</h1>
<form method="post" enctype="multipart/form-data">
    {% csrf_token %} {{form.as_p }}
    <button type="submit">Submit New Post</button>
</form>

``` 
## Test app

Now Test App
```
python manage.py runserver
```



![5rnhdqirapaqq59dtfr6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663830344570/Iy1rQQhDT.png align="left")

If You Have Any Queries Feel Free To Ask












































