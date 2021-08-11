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

`(Env64) Z:\MachineLearning> cd Z:\MachineLearning\Projects\DjangoRestAPIDemo` 

* We are going to create a new Django project in the above folder with the command as shown below. In our example, the new Django project is called `APIProject`.

`(Env64) Z:\MachineLearning\Projects\DjangoRestAPIDemo> django-admin startproject APIProject`

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


