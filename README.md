# ML-Django-REST-API


### To deploy the model we have trained via an API, we have to structure our API to:

* Accept data and pre-process it
* Load the model we have trained using pickle
* Predict the outcome based on the data we have passed
* Return the prediction and confidence score as a JSON (JavaScript Object Notation)

## Step 1

Create a virtual environment and install required libraries:
C:\Users\Documents\mldeployment>python -m venv my_api
I named my virtual environment ‘my_api’, you can use any name you like.
After running that command, activate the virtual environment using the following command…
C:\Users\Documents\mldeployment>cd my_api
C:\Users\Documents\mldeployment\my_api>cd Scripts
C:\Users\Documents\mldeployment\my_api\Scripts>activate
You should get a result like this:
(my_api) C:\Users\Documents\mldeployment\my_api\Scripts>

## Step 2

Set up Django and Djangorest:
In your command line, create a Django project…
(my_api)C:\Users\Documents\mldeployment>django-admin startproject model_deploy
After that, navigate to the folder’s directory and create a Django app
(my_api)C:\Users\Documents\mldeployment\model_deploy>django-admin startapp api

In the model_deploy folder navigate to the settings.py file and add ‘rest_framework’ and ‘api’ which is the app we created under INSTALLED_APPS.

Then make migrations
(my_api)C:\Users\Documents\mldeployment\model_deploy>python manage.py makemigrations
Then migrate
(my_api)C:\Users\Documents\mldeployment\model_deploy>python manage.py migrate
Let’s check if everything is working by running the server
(my_api)C:\Users\Documents\mldeployment\model_deploy>python manage.py runserver


## Step 3

Building the API:
In api folder, navigate to the views.py file and paste the following code.
