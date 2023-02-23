FROM python:3.10-slim-buster

# Shows the working directory entry point of the container
# It creates the directory also
WORKDIR /backend-flask

# COPY command copies content from <host OS> to <container> 
# The file requirements.txt contains the libraries to be installed
COPY requirements.txt requirements.txt

# This command executes a command. 
# Its executing the installation of the python libraries residing in reqiurement.txt
RUN pip3 install -r requirements.txt

# "." here refers to the current working directory
# Thats "/backend-flask" for container and wherever you want to build for the Host OS
COPY . .

# Declare Enviroment variables in the container
ENV FLASK_ENV=development

EXPOSE ${PORT}

# CMD is used to exceute the process needed to put create the container image
# python3 -m flask run --host=0.0.0.0 --port=4567
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]