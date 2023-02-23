# Week 1 — App Containerization
![App Containerization](assets/wk1/wk1banner.png)

## Synopsis:
Week 1 deep dived into functional use cases of docker. I started learning about Docker a week prior to the bootcamp starting and this week maximized the knowledge gained so far. I got to build and run containers for the front and back end of the cruddur application, play around with the app's codebase, implement best practices in creating their images, implement health checks on both builds, push my custom image to docker hub and re-pull for use etc. The [Homework Challenges](#homework-challenges) streched me soo much this week!!! and Im glad it did. I ended up deploying one of the images on a Google Kubernetes Engine(GKE)

## [Required Homework](#required)
### 1. Containerize Application:
> **PS**
>
> This is the Gitpod worspaces version
Succeeded in containerizing the front and back end cruddur app codebases and testing them with gitpod's url to affirm the success. Here's the final outpu.
![Gitpod Final](assets/wk1/gitpodfinal.png)
- Started by writing a docker file for backend end(Python-flask), defined the paths and commands needed to build the image for the container. Find the well dcumented docker file  [here](https://github.com/wandexdev/aws-bootcamp-cruddur-2023/blob/main/backend-flask/Dockerfile)
![Gitpod Final](assets/wk1/backenddfile.png)
- Prepared the python environment in my working directory by installing dependencies, setting environment variables for CORS, running the below command and unlocking the gitpod port:
    ```pip3 install -r requirements.txt```

    ```shell
        export BACKEND_URL="*"
        export FRONTEND_URL="*"
    ```
    ```shell
        python3 -m flask run --host=0.0.0.0 --port=4567
    ```
> ```-m``` mean module
> ```--host=0.0.0.0``` binds host to entire ipv4 for easy access
> ```--port=4567``` is the port allocated for it to run
- Test-Run the gitpod url with ```/api/activities/home```
![backend json](assets/wk1/jsonbackend.png)
- Finally built the image with ```docker build -t flask-backend ./flask-backend``` from the repository root directory
- A running image is a container so I ran ```docker container run --rm -p 4567:4567 -d backend-flask```
> ```--rm``` kills and remove the container once stopped to prevent pile up
> ```-p 4567:4567``` specifies port of Host OS binded to port of conainer 
> ```-d``` mean to run process in detached mode not current terminal

<!--- place holder for ensuring environmet variables are saved-->

### 2. Cnfigure