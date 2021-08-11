We are going to create and test our APIs locally on a local server. To fully productionize the API you could easily transfer the created Django app below (code) to a remote server. I would provide links to some very simple and easy to follow articles in the end which describe how to transfer a Django app to a remote server from your local system. 


## Create New Python Environment

 * Open your command line terminal. First create a new python environment on your system. 
 * The virtual environment can be created in any location or directory on your system. Once the virtual environment has been created you need to activate it

```
# Example of creating a Python Virtual Environment

#First install virtual environment package using pip
python -m pip install virtualenv

# Create the Environment directory
mkdir MachineLearning
cd MachineLearning

# Create a virtual environment to isolate our package dependencies locally
python -m virtualenv Env64

#Activate the virtual environment
source Env64/bin/activate  # On Windows use `Env64\Scripts\activate`
```
## Install Django & Django REST Framework

* Activate your new environment and install Django in your environment by the following command. I am installing the version 2.2.0 as this is the stable version at the time of writing: `python -m pip install Django==2.2.0`

* Check if Django has installed correctly by checking its installed version with : `python -m django --version` That should give you an output: `2.2`

* Install Django REST Framework `python -m pip install djangorestframework` 

* Create New Django Project, once Django has installed properly, navigate to a folder of your choice in the command line. In our example we navigate to a folder called `DjangoRestAPIDemo`.

* We are going to create a new Django project in the above folder with the command as shown below. In our example, the new Django project is called `APIProject`.

```
(Env64) Z:\MachineLearning> cd Z:\MachineLearning\Projects\DjangoRestAPIDemo
(Env64) Z:\MachineLearning\Projects\DjangoRestAPIDemo> django-admin startproject APIProject
```

* By default Django creates a project with name specified and also creates a main app for the project with the same name inside the folder.

* Rename the top 'APIProject' folder to 'APIProjectFolder'.

* This is so that it's clear that the main app is called APIProject within the APIProjectFolder. Note here that the `__init__.py` inside any folder means that it is a Django app. In this case the APIProject is the main Django app and hence contains the `__init__.py` file. We can create more apps in the APIProjectFolder. 

## Create a new app 

* We are going to call our app Prediction. To create this app, first navigate inside the APIProjectFolder in the command line, and then run the command as below.
```
(Env64) Z:\MachineLearning\Projects\DjangoRestAPIDemo>cd APIProjectFolder 
(Env64) Z:\MachineLearning\Projects\DjangoRestAPIDemo\APIProjectFolder>django-admin startapp Prediction  
```
You would see that a new app call `Prediction` has been created inside the project folder with the signature `__init__.py` file inside it.

* Add our newly created app to project settings

* Open the settings.py file in the APIProject folder. Add our new app 'Prediction' in the installed apps section. You also need to add  'rest_framework' as an installed app. See below:

``` 
# Application definition in settings.py file

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'Prediction',
    'rest_framework',
]
```

## Migrate initial schema

At this point we need to migrate our existing schema to a database. This is because normally within Django, we create models to interact with databases. So that our project schema(models) are consistent with the database we need to migrate first. By default the database is a sqlite database. You could see the name of the default database in the settings.py file in the main APIProject app.

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

We can change this to some other database but this is not required now for this small tutorial. To migrate existing data to the database, ensure that you are inside the main project app folder and then run the command 'python manage.py migrate' from your command line terminal as shown.

`(Env64) Z:\MachineLearning\Projects\DjangoRestAPIDemo\APIProjectFolder>python manage.py migrate `

You would see messages such as below:

```
Operations to perform:                                                                                        
Apply all migrations: admin, auth, contenttypes, sessions                                                
Running migrations:                                                                                          
Applying contenttypes.0001_initial... OK                                                                   
Applying auth.0001_initial... OK                                                                          
Applying admin.0001_initial... OK                                                                         
Applying admin.0002_logentry_remove_auto_add... OK                                                          
Applying admin.0003_logentry_add_action_flag_choices... OK                                                  
Applying contenttypes.0002_remove_content_type_name... OK                                                   
Applying auth.0002_alter_permission_name_max_length... OK                                                   
Applying auth.0003_alter_user_email_max_length... OK                                                        
Applying auth.0004_alter_user_username_opts... OK                                                           
Applying auth.0005_alter_user_last_login_null... OK                                                         
Applying auth.0006_require_contenttypes_0002... OK                                                          
Applying auth.0007_alter_validators_add_error_messages... OK                                                
Applying auth.0008_alter_user_username_max_length... OK                                                     
Applying auth.0009_alter_user_last_name_max_length... OK                                                    
Applying auth.0010_alter_group_name_max_length... OK                                                        
Applying auth.0011_update_proxy_permissions... OK                                                           
Applying sessions.0001_initial... OK
```
You can see that the sqlite database file has been created in the project folder.

## Create Super User

* Next we are going to create a superuser for our project. This is so that the superuser can login to an admin dashboard and monitor the project as we would see later on.

* Run the command `python manage.py createsuperuser`. This would ask you for user details and password. Provide them and press enter every time. 

* Once the superuser has been created, the terminal would display `Superuser created successfully` in the end.

## Check Django is running successfully. 

* Type the following in the command line.

```
(Env64) Z:\MachineLearning\Projects\DjangoRestAPIDemo\APIProjectFolder>python manage.py runserver
```

If you followed the above steps correctly, you would see the following message in the command line.

```
System check identified no issues (0 silenced).
Django version 2.2, using settings 'ModelAPI.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.
```

* Open your web browser and go to the url `http://127.0.0.1:8000/`. You should see the following in your browser: 


## Views in Django

* We would first create a test API function view, to create a basic API end point, which performs basic operations such as addition on the data sent to it. This is just to demonstrate how a basic API end point can be created in Django. Later on we would embed functionality to perform predictions in the same API end point. 

* To create API views in the Django REST Framework, there are three ways this could be done - Function based views, Class based views and using Viewsets. 

* While function based and class based views are almost the same as in core Django, Viewsets are only found in the Django REST Framework. 

* I would show the basic example with both Function based and Class Based views.

## Create Urls

* Django app is a web app it only takes user commands in the form of urls or requests. 
* That is, to perform  a specific task in Django, the user sends a url, or in other words a request, to the server. 
* Depending on the content of the request, the server then performs some specific calculations and sends responses back to the user. 
* The responses could be http responses containing html code or it could return specific strings, json or other web format responses. 
* Also depending on the task to be performed, broadly speaking the user either sends a GET request or a POST request to the server. 
* The GET request is normally used when the user sends certain parameters to the server and expects to 'GET' something back from the server pertaining to the parameters sent. 
* A POST request is normally used when a user is 'POST'ing some data to the server. 
* Normally such data is sent in the form of data from a user FORM or in formats such as JSON.

Therefore the first thing we need to do is to define what urls perform what functions. These urls need to be defined in the main Django Project app's urls.py folder. This is because requests made to the server are first processed by the main app (the project app). So, we open the urls.py file in the APIProject directory.

Add the line as shown below:

```
urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('Prediction.urls')),
]

```

What this means is that when the user makes a request with a url starting with http://127.0.0.1:8000/api look for the remaining part of the url in the file of the same name - urls.py file located in the folder for our app Prediction. This would be more clear from the explanation afterwards. Note that the http://127.0.0.1:8000 would be replaced by the website address/domain name of your site once you have fully productionized your application. 

We next open the urls.py file in the Prediction folder. If the file doesn't exist create a file with the name urls.py in the Prediction folder. Add the following lines of code:

```
from django.urls import path
import Prediction.views as views

urlpatterns = [
    path('add/', views.api_add, name = 'api_add')
]
```

This is essentially the extension of the urls from the main project. So basically, what we are trying to achieve is that when a user sends a request starting with url http://127.0.0.1:8000/api/add/, run the function 'api_add' in the views.py inside the Prediction app.  


## Create API with API View Function 

Since we have created the url required to make an API request, now lets create the API function. 

* To define an API function in Django, we need to add a decorator '@api_view' before the function definition. 
* The decorator converts a python function to an API function. We also need to tell what kind of requests can be made to the server with this function. In our case we need to be able to make a POST request. 
* However, I have also added a GET to the api_view decorator just to demonstrate. 
* Our function is called 'api_add'. 
* The function with the decorator would look like below:

```
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response

# Create your views here.
@api_view(['GET', 'POST'])
def api_add(request):
    sum = 0
    response_dict = {}
    if request.method == 'GET':
        # Do nothing
        pass
    elif request.method == 'POST':
        # Add the numbers
        data = request.data
        for key in data:
            sum += data[key]
        response_dict = {"sum": sum}
    return Response(response_dict, status=status.HTTP_201_CREATED)
    
```

If you look at the function above, you would see what it is supposed to be doing.  If the request.method is GET, the function does nothing. 
If the request method is POST,  we would pass JSON data along with the POST request to the function. 
The function would take JSON inputs such as below.

```
{
	"x": 1,
	"y":2
}
```

* Next, the function would receive this request data in a Python dictionary variable called data. 
* Next, for every key in the dictionary, the function would iterate to calculate the sum. 
* Next we create a dictionary output for our sum. 
* This  response dictionary is then sent back with the Response function (imported from the Response module of the django rest framework). 
* The Response function is a highly versatile function and it determines the rendered output type of the response data on the fly. 
* This is determined by a content negotiation with the client application (i.e. web browser, mobile app etc.). 
* So, if the client application requires output in the form of a JSON, the Response function would convert the data to JSON in the output. 
* This is why we are able to send a Python dictionary as output with the Response. 
* You can read more about the Response function from the official documentation. 

Save the views.py file. And restart the server with 

```
python manage.py runserver
```

## Test API 

* The API should be ready. You can test the API in your browser by going to http://127.0.0.1:8000/api/add/, but this is not a recommended method to test the API. 
* The reason for this is that the moment you visit http://127.0.0.1:8000/api/add/ with your browser, you have basically already made an API request to the server but without any data. 
* However, since the Django REST Framework allows testing the API with the browser so I would also cover that as well but after the recommended test method below.
* To test our API we first need to use an application called Postman. 
* After opening postman, register and login. 
* Choose Create a request. Below 'Untitled Request', choose POST from the dropdown. 
* Enter 'http://127.0.0.1:8000/api/add/' in the request url.
* Select Body, raw and JSON from the dropdown
* Enter the JSON input data and press send. If the api worked correctly you should receive a response in the form of JSON at the bottom of the screen.

Alternative 

* To test the above in the browser. 
* Go to http://127.0.0.1:8000/api/add/ from your browser. 
* You would see a form with an input called 'Content' and a button called POST below. 
* In the content enter your JSON data and press the POST button.
* You would receive the output as before in JSON format at the top. 

You have created your very first REST API in Django.


## Create API with Class Based Views

* Next we are going to create the same API functionality as before but with a Class based view. 
* Class based views are recommended in Django, as class allows benefits such as inheriting properties from other classes and mixing properties (such as mixing authentication and permissions) which allow a lot more flexibility than function based views.
* To create a class based view we first import the standard APIView class from the rest_framework module. 

```
from rest_framework.views import APIView
```
The class based view would look like below. Note that this has been added in the same views.py file as before in the Prediction app. In the class we can change the class function, post, which handles POST requests.

```
# Class based view to add numbers
class Add_Values(APIView):
    def post(self, request, format=None):
        sum = 0
        # Add the numbers
        data = request.data
        for key in data:
            sum += data[key]
        response_dict = {"sum": sum}
        return Response(response_dict, status=status.HTTP_201_CREATED)
```
