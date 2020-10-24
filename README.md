# Image Classification Web App

## What's Included
Folders and files:
1. saved_model - TensorFlow SavedModel Directory
2. templates - HTML templates
3. app.py - Flask app
4. image-classification-model.ipynb - Jupyter Notebook where the model was trained
5. requirements.txt - Libraries you need to install

## Environment
I used python 3.7 and TensorFlow 2.1.0. 

You should create a new virtual environment to avoid issues with existing installations.

Create a project folder and a virtual environment folder within:
```
$ mkdir myproject

$ cd myproject

$ py -3 -m venv venv
```

Activate the environment:
```
$ venv\Scripts\activate
```
Your shell prompt will change to show the name of the activated environment.

Within the activated environment, use the following command to install all the necessary packages:
```
(venv) $ python -m pip install -r requirements.txt
```


## Docker Instance
We are going to use a Docker image called TensorFlow Serving to create a Docker instance, which will serve the model we have in pets/model.
If you run ```$ sudo docker ps``` you'll see that you don't have any docker instance running (and that's ok now).

Launch the docker instance which will serve the TensorFlow SavedModel (in the pets folder):
```
$ sudo docker run -p PORT_NUMBER:8501 --name=CONTAINER_NAME -v "YOUR_SAVED_MODEL_PATH" -e MODEL_NAME=pets tensorflow/serving
```

- PORT_NUMBER: port of your localhost, it can we whatever you want. For this project I used 8502

- 8501: port exposed to the Docker instance, it's 8501 by default

- CONTAINER_NAME: name of the container

- YOUR_SAVED_MODEL_PATH: absolute path of the pets folder in your local machine

- DOCKER_MODEL_PATH = '/models/modelName/versionNumber': path where the model is going to be copied in the Docker instance

- MODEL_NAME: the environment model name must be the same as modelName in DOCKER_MODEL_PATH

 So, if you extracted the downloaded zip file in, say, /home/example/ , and want to use 8502 for the server port, the above
command will become:
```
$ sudo docker run -p 8502:8501 --name=pets -v "/home/example/pets/:/models/pets/1" -e MODEL_NAME=pets tensorflow/serving
```

In my case:
```
$ docker run -p 8502:8501 --name=horse-vs-human -v "C:/Users/Maria Grandu
ry/Documents/AI-ML-DL/Flask Web Apps/image-classification-web-app/saved_model/:/models/horse-vs-human/1" -e MODEL_NAME=horse-vs-human tensorflow/serving
```


Please note if you use any other port, you will have to change the MODEL_URI in the app.py file accordingly.

You can use Ctrl+C and close the terminal, if you open another one and run ```$ sudo docker ps``` you'll see that the instande is still running.

#### Notes
- The model is copied from your machine's 'home/models/pets' directory to the 'models/pets/1' directory in the Docker image instance.

- You can send requests to port 8502 of your localhost to get predictions from the model server that will instantiate.

- When making a post request to the TF model server, the data is sent after UTF-8 string encoding.


## Flask App
Once the docker instance is running, you can launch the flask app:

```
$ python app.py
```

And that's it! The default port for flask is 5000, so you can access the app by going to localhost:5000 in your browser