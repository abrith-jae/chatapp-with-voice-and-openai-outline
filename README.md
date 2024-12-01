# Voice Assistant with OpenAI's GPT-3 and IBM Watson

## Step 1: Understanding the interface
In this project, the goal is to create an interface that allows communication with a voice assistant, and a backend to manage the sending and receiving of responses.

The frontend will use HTML, CSS and Javascript with popular libraries such as Bootstrap for basic styling, Font Awesome for icons and JQuery for efficient handling of actions. The user interface will be similar to other voice assistant applications, like Google Assistant. The code for the interface is provided and the focus of the course is on building the voice assistant and integrating it with various services and APIs. The provided code will help you to understand how the frontend and backend interact, and as you go through it, you will learn about the important parts and how it works, giving you a good understanding of how the frontend works and how to create this simple web page.

Run the following commands to receive the outline of the project, rename it to save it with another name and finally move into that directory.

1. git clone https://github.com/arora-r/chatapp-with-voice-and-openai-outline.git
2. mv chatapp-with-voice-and-openai-outline chatapp-with-voice-and-openai
3. cd chatapp-with-voice-and-openai

The next section gives a brief understanding of how the frontend works.


## Step 2: Understanding the server
The server is how the application will run and communicate with all your services. Flask is a web development framework for Python and can be used as a backend for the application. It is a lightweight and simple framework that makes it quick and easy to build web applications.

With Flask, you can create web pages and applications without needing to know a lot of complex coding or use additional tools or libraries. You can create your own routes and handle user requests, and it also allows you to connect to external APIs and services to retrieve or send data.

This guided project uses Flask to handle the backend of your voice assistant. This means that you will be using Flask to create routes and handle HTTP requests and responses. When a user interacts with the voice assistant through the frontend interface, the request will be sent to the Flask backend. Flask will then process the request and send it to the appropriate service.

The code provided gives the outline for the server in the server.py file.


## Step 3: Running the application
Docker allows for the creation of “containers” that package an application and its dependencies together. This allows the application to run consistently across different environments, as the container includes everything it needs to run. Additionally, using a Docker image to create and run applications can simplify the deployment process, as the image can be easily distributed and run on any machine that has Docker installed. This can help to ensure that the application runs in the same way in development, testing, and production environments.

The git clone from Step 1 already comes with a Dockerfile and requirements.txt for this application. These files are used to build the image with the dependencies already installed. Looking into the Dockerfile you can see its fairly simple, it just creates a python environment, moves all the files from the local directory to the container, installs the required packages, and then starts the application by running the python command.

3 different containers need to run simultaneously to have the application run and interact with Text-to-Speech and Speech-to-Text capabilities.

Small prerequisites:
You need to run these commands with a single click to fulfill some of the prerequisites:

1. mkdir /home/project/chatapp-with-voice-and-openai/certs/
2. cp /usr/local/share/ca-certificates/rootCA.crt /home/project/chatapp-with-voice-and-openai/certs/

### 1. Starting the application
This image is quick to build as the application is quite small. These commands first build the application (running the commands in the Dockerfile) and tags (names) the built container as voice-chatapp-powered-by-openai, then runs it in the foreground on port 8000. You'll need to run these commands everytime you wish to make a new change to one of the files.

1. docker build . -t voice-chatapp-powered-by-openai
2. docker run -p 8000:8000 voice-chatapp-powered-by-openai


## Step 4: Integrating Watson Speech-to-Text
Speech-to-Text functionality is a technology that converts speech into text using machine learning. It is useful for accessibility, productivity, convenience, multilingual support, and cost-effective solutions for a wide range of applications. For example, being able to take a user's voice as input for a chat application.

Using the embedded Watson Speech-to-Text AI model that was deployed earlier, it is possible to easily convert your speech-to-text by a simple API. This result can then be passed to OpenAI API for generating a response.

Implementation
You will be updating a function called speech_to_text that will take in audio data received from the browser and pass it to the Watson Speech-to-Text API. Open worker.py from the explore


## Step 5: Integrating OpenAI API
It's time to give your voice assistant a brain! With the power of OpenAI's GPT-3.5 API, you can pass the transcribed text and receive responses that answer your questions.

Normally, you would need to get an API key by creating an OpenAI account, when using our labs, however this has been taken care for so you can proceed with your project without worrying about it.

OpenAI process message function
You will be updating the function called openai_process_message, which will take in a prompt and pass it to OpenAI's GPT-3 API to receive a response. Essentially, it's the equivalent of pressing the send button to get a response from ChatGPT.

Go ahead and update the openai_process_message function in the worker.py


## Step 6: Integrating Watson Text-to-Speech
Time to give your assistant a voice using Text-to-Speech functionality.

Once we have processed the user's message using OpenAI, let's add the final worker function that will convert that response to speech, so you get more personalized feel as the Personal Assistant is going to read out the response to you. Just like other virtual assistants do like Google, Alexa, Siri etc.

Text-to-Speech function
In the worker.py file, the text_to_speech function passes data to Watson's Text-to-Speech API to get the data as spoken output.

This function is going to be very similar to speech_to_text as you will be utilizing your request library again to make an HTTP request. Let's dive into the code.

Again, remember to replace the ... for the base_url variable with the URL for your Text-to-Speech model (for example,https://sn-watson-tts.labs.skills.network).


## Step 7: Putting everything together by creating Flask API endpoints
Now by using the functions we defined in the previous sections; we can connect everything together and complete the assistant.

The changes in this section will be for the server.py file.


## Step 8: Testing your personal assistant
The assistant is now complete and ready to use.

Now that we've updated the code quite considerably, it is a good time to rebuild our docker image and test to see that it is working as expected in this environment.

Assuming the Text-to-Speech and Speech-to-Text model URLs are correctly set, you just need to rebuild the image for the application and rerun it so it has all the latest changes.

This step assumes that you have no running container for the application. If you do, please press Crtl (^) and C at the same time to stop the container.

1. docker build . -t voice-chatapp-powered-by-openai
2. docker run -p 8000:8000 voice-chatapp-powered-by-openai

Then just open the application on a new tab (or if you already have the tab running - refresh that page).
