## Compose sample application

### NGINX proxy with Go backend

Project structure:

```
.
├── backend
│   ├── Dockerfile
│   └── main.go
├── compose.yaml
└── README.md
```

[`compose.yaml`](compose.yaml)

```yaml
services:
  backend:
    build:
      context: backend
      target: builder
    ports:
      - 3002:3002
```

The compose file defines an application with one service `backend`.

When deploying the application, docker compose maps port 3002 of the backend service container to the same port of the host as specified in the file.
Make sure port 3002 on the host is not already in use.

## Deploy with docker compose

```bash
$ docker compose up -d
Creating network "welcome_go_default" with the default driver
Building backend
Step 1/7 : FROM golang:1.13 AS build
1.13: Pulling from library/golang
...
Successfully built 4b24f27138cc
Successfully tagged welcome_go-backend-1:latest
Creating welcome_go-backend-1 ... done
```

## Expected result

Listing containers must show two containers running and the port mapping as below:

```bash
$ docker compose ps
NAME                   IMAGE                COMMAND               SERVICE   CREATED          STATUS          PORTS
welcome_go-backend-1   welcome_go-backend   "/code/bin/backend"   backend   23 seconds ago   Up 23 seconds   0.0.0.0:3002->3002/tcp
```

After the application starts, navigate to `http://localhost:3002` in your web browser or run:

```bash
$ curl localhost:3002

          ##         .
    ## ## ##        ==
 ## ## ## ## ##    ===
/"""""""""""""""""\___/ ===
{                       /  ===-
\______ O           __/
 \    \         __/
  \____\_______/


Hello from Docker!
```

Stop and remove the containers

```bash
$ docker compose down
[+] Running 2/2
 ✔ Container welcome_go-backend-1  Removed                                       0.1s
 ✔ Network welcome_go_default      Removed                                       0.1s
```
