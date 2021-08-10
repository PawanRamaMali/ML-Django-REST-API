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

