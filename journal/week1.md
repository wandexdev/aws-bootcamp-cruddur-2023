# Week 1 — App Containerization
![App Containerization](assets/wk1/wk1banner.png)

## Synopsis:
Week 1 deep dived into functional use cases of docker. I started learning about Docker a week prior to the bootcamp starting and this week maximized the knowledge gained so far. I got to build and run containers for the front and back end of the cruddur application, play around with the app's codebase, implement best practices in creating their images, implement health checks on both builds, push my custom image to docker hub and re-pull for use etc. The [Homework Challenges](#homework-challenges) streched me soo much this week!!! and Im glad it did. I ended up deploying one of the images on a Google Kubernetes Engine(GKE)

## [Required Homework](#required)
### 1. Containerize Application:
> **PS**
>
> This is the Gitpod workspaces version

Succeeded in containerizing the front and back end cruddur app codebases and testing them with gitpod's url to affirm the success.
#### THE BACKEND:
- Started by writing a docker file for **backend end(Python-flask)**, defined the paths and commands needed to build the image for the container. Find the well documented docker file  [here](https://github.com/wandexdev/aws-bootcamp-cruddur-2023/blob/main/backend-flask/Dockerfile)
![Gitpod Final](assets/wk1/backenddfile.png)
- Prepared the python environment in my working directory by installing dependencies, setting environment variables for CORS, running the below command and unlocking the gitpod port:
    ```shell
        pip3 install -r requirements.txt
    ```
    ```shell
        export BACKEND_URL="*"
        export FRONTEND_URL="*"
    ```
    ```shell
        python3 -m flask run --host=0.0.0.0 --port=4567
    ```
> ```-m``` mean module
>
> ```--host=0.0.0.0``` binds host to entire ipv4 for easy access
>
> ```--port=4567``` is the port allocated for it to run
- Test-Run the gitpod url with ```/api/activities/home```
![backend json](assets/wk1/jsonbackend.png)
- Finally built the image with ```docker build -t flask-backend ./flask-backend``` from the repository root directory
- A running image is a **container** so I ran ```docker container run --rm -p 4567:4567 -d backend-flask``` to run the image.
>
> ```--rm``` kills and remove the container once stopped to prevent pile up
>
> ```-p 4567:4567``` specifies port of Host OS binded to port of container
> 
> ```-d``` mean to run process in detached mode not current terminal

<!--- place holder for ensuring environmet variables are saved-->
#### THE FRONTEND:
- Started the **Frontend (REACT-JS) with writing a docker file also, in the frontend directory. Find it [here](https://github.com/wandexdev/aws-bootcamp-cruddur-2023/blob/main/frontend-react-js/Dockerfile)
- Ran ```npm i``` in the root directory to prepare the envirnment. NPM is a node package manager, it helps to manage the node packages for the application to properly run.
- Built the image with ```docker build -t frontend-react-js```
> ```-t``` option is specifying the image name and when paired with ```:<anything>```, 
>
> it gives name + tag e.g ```mysql:latest```
- Ran the image and turn into a container with ```docker run -p 3000:3000 -d frontend-react-js```
![Gitpod Final](assets/wk1/gitpodfinal.png)
#### ALL CONTAINERS WITH DOCKER COMPOSE:
- Docker Compmose allows multiple containers defined in its .yml file to run them simultenously, created the yaml file [here](https://github.com/wandexdev/aws-bootcamp-cruddur-2023/blob/main/docker-compose.yml) in the root directory.
- Mounted volumes to the Host OS's directory by binding XXXXXX and this enables me make live changes to the application code in my directory while the changes syncs with the one in the container
![volumes](assets/wk1/dockercompose.png)
- Ran ```docker compose up``` and it ran the containers. 
### 2. Notification Endpoint for the OpenAI Document:
- Open API (formerly called SWAGGER) is a standard for defining general API's. It esaily augments services like API Gateway, api documentaion etc
- The Endpoint documentation can be uploaded to [readme.com](https://readme.com/) where the API documentation would be transformed to interactive hubs that help developers.
- I added an endpoint for notification in [this](https://github.com/wandexdev/aws-bootcamp-cruddur-2023/blob/main/backend-flask/openapi-3.0.yml) file. Here is the file and **OpenAPI SwaggerUI** preveiw:
![Open API](assets/wk1/openapinotification.png)
![Open API](assets/wk1/openapi2.png)
### 3. Flask Backend Endpoint for Notifications:
- The home section has similar schema and features with the intended notification section so the values were duplicated and refactored.
- I created the ```notifications_activites.py``` file from ```home_activities.py``` file.
- Sourced the ```notifications_activites.py``` file in the ```app.py``` file.
- Redirected the url from ```app.py``` to ```notifications_activites.py```
- Troubleshooted an error that lead to no json output when testing the Endpoint URL at ```.../api/activities/notifications```
- Final perfect output:
![Backend Endpoint](assets/wk1/backendEP.png)
### 4. React Page for Notifications:
- React js is the language used to write the frontend application code of the cruddur application.
- I created  ```NotificationsFeedPage.js``` and ```NotificationsFeedPage.css``` files.
- Sourced the ```NotificationsFeedPage.js``` file in the ```app.py``` file.
- ```DesktopNavigation.js``` already sorted
-Here's the final perfect output:
![React](assets/wk1/frontreact.png)
### 5. DynamoDB Local Container:
- DynamoDB is a fully managed NoSQL database service that supports key–value and document data structures and it institutes from Amazon Web Services(AWS)
- I added Dynamo db content (volumes, port binding etc) to the docker compose, and tested the data base by creating essentials in it from the terminal
- Here's the docker compose file:
![dynamo](assets/wk1/dynamo.png)
- Created a **table**, here are the commands and output:
![table](assets/wk1/dynamodbtable.png)
- Created an **item**:
![item](assets/wk1/item.png)
- Listed Tables with:
```shell
    aws dynamodb list-tables --endpoint-url http://localhost:8000
```
- Scan for **records**:
![table](assets/wk1/records.png)
### 6. Postgres Container:
- PostgreSQL, also known as Postgres, is a free and open-source relational database management system emphasizing that SQL compliant.
- I added Postgres content (volumes, port binding etc) to the docker compose file:
![postgres](assets/wk1/posgre.png)
- Postgres requires a driver/client installed before running properly in the environment its set to run so I added the driver confirugations to my gitpod yaml file.
![pgsql Gitpod](assets/wk1/pgsqlgitpod.png)
- Installed database explorer extension on my gitpod's vscode which I added to the gitpod's yaml file and used to test the functionality of the postgres database.
- On the terminal, I ran ```psql -U postgres --host localhost``` entered the password set then ```\l``` when let in the pgsql terminal.
![postgres](assets/wk1/defaulttables.png)
 

## [Homework Challenges](#challenges)
Here is a little summary before the details:
- [x] Ran the dockerfile CMD as an external Shell (.sh) script.
- [x] Pushed and tagged an image to DockerHub.
- [x] Used multi-stage building for a Dockerfile build
- [x] Implemented a healthcheck in the Version 3 Docker compose file
- [x] Installed Docker on localmachine and got the same containers running outside my Gitpod.
- [x] Launched an EC2 instance that has docker installed, and pulled an image to demonstrate my knowledge of docker processes. 

### 1. DockerFile CMD ran as an external SHELL Script
- I created two shell scripts. Each for the docker files in both frontend and backend directories.
- For **backend**, I created the ```flash.sh``` file with the entry points commands updated to it. I added the permissions for the file to run also in the docker file
    ```shell
        #!/bin/bash
        python3 -m flask run --host=0.0.0.0 --port=4567
    ```
 - Replace 

    ```Dockerfile
    CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]
    ``` 

    with:

```Dockerfile
        # Copy the shell script to the container
        COPY flask.sh flask.sh

        # Give Permissions to the script
        RUN chmod +x flask.sh

        # Execute external script instead of CMD commands
        CMD [ "/bin/bash", "flask.sh" ] 
```
- For the **frontend**, I created the ```npm.sh``` file with the entry points commands updated to it. I added the permissions for the file to run also in the docker file
    ```shell
        #!/bin/sh
        npm start
    ```
 - Replace 

    ```Dockerfile
        CMD ["npm", "start"]
    ```

    with:

    ```Dockerfile
            # Copy the shell script to the container
            COPY flask.sh npm.sh

            # Give Permissions to the script
            RUN chmod +x npm.sh

            # Execute external script instead of CMD commands
            CMD [ "/bin/bash", "npm.sh" ] 
    ```

- Build and run container with ```docker-compose up -d``` 
### 2. Pushed and tagged an image to DockerHub
- Docker Hub is kind of a registry where all the Docker Images are placed. Developers can select images from Docker Hub and use them to create containers or other images. Docker Hub is the world's largest registry of image containers.
- I signed up for a free docker hub account, created a new repository.
- Tagged the image and attached to repository because its already built and existing.
```shell
    docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]
```
- Pushed the repository to the regitry specifyd by the name or tag
- Loggeed in when push failed using the ```docker login``` command and pushed successfully.
![docker push](assets/wk1/dockerpush.png)
![docker push](assets/wk1/dockerhubp.png)
### 3. Used multi-stage building for a Dockerfile build
- Did two multi stage build with the first having a new **node** being built from an **alphine node** layer and the second from **nginx alpine** being built from the node image built in multi stage 1.
- Here are the end results docker files:
- **Multi-stage build one** Dockerfile:
```Dockerfile
    FROM node:16.18 AS build

    WORKDIR /frontend-react-js

    COPY package*.json ./

    RUN npm install

    COPY . .

    RUN npm run build


    # MULTIBUILD
    FROM node:16.18-alpine AS runtime 

    WORKDIR /frontend-react-js

    COPY --from=build /frontend-react-js /frontend-react-js

    # CHECK FRONTEND DISK SIZE
    RUN du -hc -d 1 /frontend-react-js

    EXPOSE 3000

    CMD ["npm", "start"]
```
- **Multi-stage build two** Dockerfile :
```Dockerfile
    FROM node:16.18 AS build

    ENV REACT_APP_BACKEND_URL=http://127.0.0.1:4567

    WORKDIR /frontend-react-js

    COPY package*.json ./

    RUN npm install

    COPY . .

    RUN npm run build

    # MULTIBUILD
    FROM nginx:stable-alpine AS runtime

    WORKDIR /usr/share/nginx/html

    # Remove default nginx static resources
    RUN rm -rf ./*

    # Copy build files from stage1 (it has all react weeb files)
    COPY --from=build /frontend-react-js/build .

    # Installs a custom nginx configuration file
    COPY nginx-conf/nginx.default.conf /etc/nginx/conf.d/default.conf

    EXPOSE 80

    # Base nginx image defines a start command for the container that launches nginx on port 80
    # nO Need for CMD as it's in the image.
    # CMD ["nginx", "-g", "daemon off;"]
```
### 4. Implemented a healthcheck in the Version 3 Docker compose file
- 
### 5. Installed Docker on localmachine and got the same containers running outside my Gitpod
- I used an ubuntu VM as my local machine
- Installed docker in it with the following commands individually:
```shell
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce -y
sudo systemctl status docker
```

- Installation Confirmation
    ![docker](assets/wk1/dockernew.png)
- Ran Docker commands without sudo by:
```
sudo usermod -aG docker ${USER}
```
- Git cloned the cruddur repository to get the application code structure on my VM
- Copied contents of ```docker-compose.yml``` to new one created for local named ```docker-compose_local.yml```. 
- Removed all gitpod envionment variables in docker compose and replaced them with localhost
- Built and ran the containers with ```docker compose -f "docker-compose_local.yml" up```
![docker](assets/wk1/dockercomposecmd.png)
![docker containers](assets/wk1/images.png)
### 6. Launched an EC2 instance that has docker installed, and pulled an image to demonstrate my knowledge of docker processes.
- Launched an EC2 instance with docker nstalled at lauch by putting my docker command in this bash script as user data:
```shell
    #!/bin/bash
    sudo apt-get update
    sudo apt-get -y install apt-transport-https ca-certificates curl gnupg lsb-release
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get -y install docker-ce docker-ce-cli containerd.io
    sudo usermod -aG docker ${USER}
``` 
- Pulled the images uploaded to docker hub


## Refrences:
- [Github Syntax Highlighting](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)
- [How To Run Custom Script Inside Docker](https://devopscube.com/run-scripts-docker-arguments/)
- [How to dockerize a react flask nginx project](https://blog.miguelgrinberg.com/post/how-to-dockerize-a-react-flask-project)
- [Docker docs: Managing repositories](https://docs.docker.com/docker-hub/repos/#:~:text=To%20push%20an%20image%20to,docs%2Fbase%3Atesting%20)