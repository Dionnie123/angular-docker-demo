# Develop Angular Project inside Docker for Beginners

By: Mark Dionnie Bulingit

## Setup Angular environment on Windows

1. Install NodeJS

https://nodejs.org/en

2. Install Angular CLI

```bash
  npm i @angular/cli
```

3. Create New Project named docker-demo

```bash
  ng new angular-docker-demo --skip-install
```

## Setup Docker on Windows

1. Install Docker Desktop

https://docs.docker.com/desktop/install/windows-install/

2. Enable CPU virtualization in your system BIOS - turning your processor into a powerhouse for running multiple operating systems and applications seamlessly.

https://www.bleepingcomputer.com/tutorials/how-to-enable-cpu-virtualization-in-your-computer-bios/

## Setup Docker in Project with VSCode

Install top VSCode extensions for Angular and Docker.

Reference: https://blog.cloudboost.io/developing-angular-applications-using-docker-6f4835a75195

1. Go to the project root and create a **_Dockerfile_** file.

```bash
FROM node:18-alpine
RUN mkdir -p /app
WORKDIR /app
COPY package.json /app/
RUN ["npm", "install"]
COPY . /app
EXPOSE 4200/tcp
CMD ["npm", "start", "--", "--host", "0.0.0.0", "--poll", "500"]

```

2. Login to Docker via Terminal

```bash
docker logout
```

```bash
docker login -u "mbulingit" -p "#PlmOknIjb123" docker.io
```

2. Build image base on Dockerfile

```bash
docker build -t angular-docker-demo-dev .
```

3. Run a docker container base on the image built. **_Make sure to run it on Powershell._**

```bash
docker run -it --rm -p 4200:4200 -v ${pwd}/src:/app/src angular-docker-demo-dev
```

4. Go to http://localhost:4200/ and start developing with live reload.

```bash
docker run -it --rm -p 4200:4200 -v ${pwd}/src:/app/src angular-docker-demo-dev
```

## Sharing Docker Image to others via DockerHub.

Install top VSCode extensions for Angular and Docker.

1. Tag your Docker Image

```bash
docker tag angular-docker-demo-dev mbulingit/angular-docker-demo-dev

```

2. Push you Docker Image to the Hub

```bash
docker push mbulingit/angular-docker-demo-dev

```

## Docker Notes:

Modifications outside src/ folder e.g package.json requires rebuild of image & container and repush on docker hub

1. Stop Container

```bash
docker stop angular-docker-demo-dev
```

2. Delete Container

```bash
docker rm angular-docker-demo-dev
```

3. Delete Image

```bash
docker rmi angular-docker-demo-dev
```

2. Repush Image to DockerHub

```bash
docker build -t angular-docker-demo-dev .
```

## Creating a distribution version of your app:

```bash
docker run -it --rm -v ${PWD}/src:/app/src -v ${PWD}/dist:/app/dist angular-docker-demo-dev npm run build
```

Explanation:
| Command | Current Directory | Container | Usage |
| ------------------------- | ----------------- | --------- | -------------------------------------------------------------------------------------------------------------- |
| docker run | | | Run a docker container |
| \-it | | | Allocates a pseudo-TTY. This is often used for an interactive shell. |
| \--rm | | | Ensures that each time a container is run, it is removed as soon as it finishes its task, for CI/CD processes. |
| \-v | | | Mount a directory. Any changes made in one location are reflected in the other. |
| \-v ${PWD}/src:/app/src   | /src              | /app/src  |${PWD} gets the absolute path of project root directory e.g C:\Users\...\angular-docker-demo\ |
| \-v ${PWD}/dist:/app/dist | /dist | /app/dist | |
| angular-docker-demo-dev | | | Docker image name |
| npm run build | | | Compiles the source code and outputs the **_/dist_** folder which can be deployed to hosting platform like Vercel. |
