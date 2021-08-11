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
## Install Django 

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

