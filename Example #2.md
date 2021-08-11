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
