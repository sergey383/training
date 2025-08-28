## Docker Desktop how to install Windows
```
https://docs.docker.com/desktop/install/windows-install/
```

## Docker Cheat Sheet:

GENERAL COMMANDS:

Start the docker daemon
```
docker -d
```
Get help with Docker. Can also use –help on all subcommands
```
docker --help
```
Display system-wide information
```
docker info
```

IMAGES:

Build an Image from a Dockerfile
```
docker build -t <image_name>
```
Build an Image from a Dockerfile without the cache
```
docker build -t <image_name> . –no-cache
```
List local images
```
docker images
```
Delete an Image
```
docker rmi <image_name>
```
Remove all unused images
```
docker image prune
```

CONTAINERS:

Create and run a container from an image, with a custom name:
```
docker run --name <container_name> <image_name>
```
Run a container with and publish a container’s port(s) to the host.
```
docker run -p <host_port>:<container_port> <image_name>
```
Run a container in the background
```
docker run -d <image_name>
```
Start or stop an existing container:
```
docker start|stop <container_name> (or <container-id>)
```
Remove a stopped container:
```
docker rm <container_name>
```
Open a shell inside a running container:
```
docker exec -it <container_name> sh
```
Fetch and follow the logs of a container:
```
docker logs -f <container_name>
```
To inspect a running container:
```
docker inspect <container_name> (or <container_id>)
```
To list currently running containers:
```
docker ps
```
List all docker containers (running and stopped):
```
docker ps --all
```
View resource usage stats
```
docker container stats
```

## Using docker init to write Dockerfile and docker-compose configs

What is docker init?
docker init is a command-line utility that helps in the initialization of Docker resources within  a project. It creates Dockerfiles, Compose files, and .dockerignore files based on the project’s requirements.

This simplifies the process of configuring Docker for your project, saving time and reducing complexity.

How to use docker init?

Using docker init is easy and involves a few simple steps. First, go to the directory of your project where you want to set up Docker assets.

Let me Create a basic Flask app.
```
touch app.py requirements.txt
```

Copy the below code in the respective files
```
# app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_docker():
    return '<h1> hello world </h1'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```
```
# requirements.txt
Flask
```
```
docker init
```

The next thing you do is choose the application platform, for our example we are using python. It will suggest the recommended values for your project such as Python version, port, entrypoint commands.

You can either choose the default values or provide the values you want, and it will create your docker config files along with instructions for running the application on the fly.

Let’s see what this auto-generated config looks like.
```
# syntax=docker/dockerfile:1

# Comments are provided throughout this file to help you get started.
# If you need more help, visit the Dockerfile reference guide at
# https://docs.docker.com/engine/reference/builder/

ARG PYTHON_VERSION=3.11.7
FROM python:${PYTHON_VERSION}-slim as base

# Prevents Python from writing pyc files.
ENV PYTHONDONTWRITEBYTECODE=1

# Keeps Python from buffering stdout and stderr to avoid situations where
# the application crashes without emitting any logs due to buffering.
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# Create a non-privileged user that the app will run under.
# See https://docs.docker.com/go/dockerfile-user-best-practices/
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \
    appuser

# Download dependencies as a separate step to take advantage of Docker's caching.
# Leverage a cache mount to /root/.cache/pip to speed up subsequent builds.
# Leverage a bind mount to requirements.txt to avoid having to copy them into
# into this layer.
RUN --mount=type=cache,target=/root/.cache/pip \
    --mount=type=bind,source=requirements.txt,target=requirements.txt \
    python -m pip install -r requirements.txt

# Switch to the non-privileged user to run the application.
USER appuser

# Copy the source code into the container.
COPY . .

# Expose the port that the application listens on.
EXPOSE 5000

# Run the application.
CMD gunicorn 'app:app' --bind=0.0.0.0:5000
```
