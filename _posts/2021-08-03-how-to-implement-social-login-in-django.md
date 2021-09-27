---
layout: single
title:  "How to implement social login in django"
date:   2021-08-03 11:30:40 +0530
categories: jekyll update
---
Nobody likes to manually sign up for an application. Social sign up makes this process easier.
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
{% highlight console %}
django-admin startproject djangoSocialApp .
python manage.py startapp home
{% endhighlight %}
Don't forget to  add both `home` and `social_django` as `INSTALLED_APPS`.
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
    'home',
]

{% endhighlight %}
Update urls.py
{% highlight python %}
urlpatterns = [
    path('admin/', admin.site.urls),         
    path("login/", views.login, name="login"),  # add this
    path("logout/", auth_views.LogoutView.as_view(), name="logout"), # add this
    path("", views.home, name="home"), # add this
]
{% endhighlight %}
Migrate the database
{% highlight python %}
python manage.py migrate
{% endhighlight %}
Create templates for login and home page
{% highlight html %}
#login.html
<div class="login">
<h5> Please sign up using your google account</h5>
    <div class="mybtn">
      <a class="btn btn-danger" {% raw %}href="{% url 'social:begin' 'google-oauth2' %}">{% endraw %}
        Signup with Google
      </a>
    </div>
</div>
#home.html
<div class="login">
    <div class="col-sm-12 mb-3">
        <h4 class="text-center"> Welcome {% raw %}{{ user.username }} {% endraw %}</h4>
    </div>
</div>

{% endhighlight %}
Create view functions
{% highlight python %}
from django.shortcuts import render
from django.contrib.auth.decorators import login_required

def login(request):
  return render(request, 'my_account/login.html')

@login_required
def home(request):
  return render(request, 'my_account/home.html')
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
    ...
    path('social-auth/', include('social_django.urls', namespace="social")),
    ...
]
{% endhighlight %}
## Step-3 Grab Authentication Keys
To obtain the Google OAuth2 credentials, Follow these steps:

1. Go to Google developer console
2. You now have to create a new project. In case you do not have any (see the drop-down at the top left of your screen)
3. Once the project is created you will be redirected to a dashboard. You will have to click on ENABLE API button
4. Select Google+ APIs from the Social APIs section
5. On the next page, click the Enable button to enable Google+ API for your project
6. To generate credentials, select the Credentials tab from the left navigation menu and hit the ‘create credentials’ button
7. From the drop-down, select OAuth client ID.
8. On the next screen, select the application type (web application in this case) and enter your authorized origins and redirect url


For now, just set the origin as http://localhost:8000 and the redirect URL as http://localhost:8000/complete/google-oauth2/.
Hit the create button and copy the client ID and client secret
## Step-4 Add OAuth Credentials in settings.py
Go to settings.py and add credentials
{% highlight python %}
SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = 
SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = 
{% endhighlight %}

Now that the configuration is done, we can start off using the social login in the Django application.

[Github]: https://github.com/amithah/Django_social_login/tree/main/PycharmProjects/djangoSocialApp
[Pipenv]: https://pypi.org/project/pipenv/
[social-auth-app-django]: https://github.com/python-social-auth/social-app-django
[Google Developers Console]:https://console.developers.google.com
