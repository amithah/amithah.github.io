---
layout: single
title:  "How to implement social login in django"
date:   2021-08-03 11:30:40 +0530
categories: jekyll update
---
Signing up each time we start using an application is inconvenient. Social sign up makes this process easier.
 If you have an existing social account, you can login using that account. 
Let's explore how to do social sign up in Django using Google account.

This tutorial requires prior knowledge of Python3 and Django framework.
You need to Install [Pipenv]. Pipenv is a tool that aims to bring the best of all packaging worlds (bundler, composer, npm, cargo, yarn, etc.) 
to the Python world. It automatically creates and manages a virtualenv for your projects, as well as adds/removes packages from your Pipfile 
as you install/uninstall packages. It also generates the ever-important Pipfile.lock, which is used to produce deterministic builds. 


The source code for this project is available on [GitHub].
## Step 1- Creating  a Django app
Create a new folder and make it the current working directory.

{% highlight console %}
mkdir django_social_login
cd django_social_login
{% endhighlight %}

Now we can create a virtual environment using pipenv. Then install the required packages. In our case, we use [social-auth-app-django]
package for social authentication.

{% highlight console %}
pipenv install django social-auth-app-django
{% endhighlight %}

Create a new Django project and create a new app for authentication. 
Don't forget to  add both `core` and `social-auth-app-django` as `INSTALLED_APPS`.

{% highlight console %}
django-admin startproject djangoSocialApp
python manage.py startapp my_account
cd djangoSocialApp
{% endhighlight %}

{% highlight python %}
INSTALLED_APPS =
 [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # => third-party apps
    'social_django',
    # => my apps
    'my_account',
]

{% endhighlight %}

Migrate the database
{% highlight python %}
python manage.py migrate
{% endhighlight %}

## Step-2 Google authentication

In order to use social authentication using Google, we need to add
the authentication backend class for Google. Update settings.py by adding the below code.
{% highlight python %}
AUTHENTICATION_BACKENDS = [
    'social_core.backends.google.GoogleOAuth2',
]

{% endhighlight %}

Update the Project’s urlpatterns in urls.py to include the main auth URLs:

{% highlight python %}
urlpatterns = [
    path('admin/', admin.site.urls),    
    path('social-auth/', include('social_django.urls', namespace="social")),
]
{% endhighlight %}

[Github]: https://github.com/amithah/Django_social_login/tree/main/PycharmProjects/djangoSocialApp
[Pipenv]: https://pypi.org/project/pipenv/
[social-auth-app-django]: https://github.com/python-social-auth/social-app-django
