```
devops-youtube-course-2025
├─ client
│  ├─ .dockerignore
│  ├─ .prettierrc
│  ├─ Dockerfile
│  ├─ eslint.config.js
│  ├─ index.html
│  ├─ package-lock.json
│  ├─ package.json
│  ├─ public
│  │  └─ vite.svg
│  ├─ README.md
│  ├─ src
│  │  ├─ App.css
│  │  ├─ App.jsx
│  │  ├─ assets
│  │  │  └─ react.svg
│  │  ├─ components
│  │  │  ├─ AddTaskDialog.jsx
│  │  │  ├─ LoadingSpinner.jsx
│  │  │  ├─ TaskCard.jsx
│  │  │  └─ TaskList.jsx
│  │  ├─ index.css
│  │  ├─ main.jsx
│  │  ├─ services
│  │  │  └─ taskService.js
│  │  └─ utils
│  │     └─ dateUtils.js
│  └─ vite.config.js
├─ docker-compose.yml
└─ server
   ├─ .dockerignore
   ├─ .prettierrc
   ├─ controllers
   │  └─ taskController.js
   ├─ Dockerfile
   ├─ eslint.config.js
   ├─ models
   │  └─ Task.js
   ├─ package-lock.json
   ├─ package.json
   ├─ routes
   │  └─ tasks.js
   └─ server.js

```

# sangam mukherjee| devops

# DevOps Full Course for Beginners 2025 | Git, Docker, CI/CD, AWS, Kubernetes | Part 1

## kubernetes

### setup kuber netes

**✅install choco,kubernetes,kubectl,minkube**

```
Set-ExecutionPolicy Bypass -Scope Process -Force; `
[System.Net.ServicePointManager]::SecurityProtocol = `
    [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; `
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

```
choco install kubernetes-cli -y
```

```
kubectl version --client
```

```
choco install minikube
```

```
minikube version
```

```
minikube start --driver=docker
```

### Build images for K8S

**✅ for backend**

➡️➡️run command from root

```
docker build -t backend:local ./server
```

**✅ for frontend**

➡️➡️run command from root

```
docker build --build-arg VITE_API_URL=http://13.212.94.28/api -t frontend:local  ./client
```

### configure for K8S

Here’s your corrected and polished version — all typos and formatting fixed for clarity and professional readability 👇

---

### ⚙️ Configure for K8S (Kubernetes)

**✅ Load images into Minikube**

➡️ Run the following commands step by step:

```bash
minikube image load backend:local
minikube image load frontend:local
minikube image ls
```

**Deploy Backend**

```bash
kubectl create deployment backend --image=backend:local
kubectl set env deployment/backend MONGO_URI="mongodb://mongo:27017/taskdb"
```

**Check backend status and logs**

```bash
kubectl get pods
kubectl logs deploy/backend
```

**Port-forward backend**

```bash
kubectl port-forward deployment/backend 5000:5000
```

**Deploy Frontend**

```bash
kubectl create deployment frontend --image=frontend:local
```

**Expose Frontend as a service**

```bash
kubectl expose deployment frontend --name=frontend-svc --port=80 --target-port=5173 --type=NodePort
```

**Access the Frontend via Minikube**

```bash
minikube service frontend-svc
```

# [﻿Docker & Jenkins DevOps - Part 2](https://www.notion.so/sangam-devops-part-2-28f714dc6fc480c283b9e1b46a4b2709?source=copy_link#28f714dc6fc48141928af27abb90256e)

# install and unistall docker

Here's a step-by-step guide to install Docker on Ubuntu:

### Step 1: Remove any old Docker versions

Before installing the new version, it's good practice to remove any older Docker packages that might be present.

```bash
sudo apt-get remove -y docker docker-engine docker.io containerd runc
```

### Step 2: Update your system

Run the following commands to update the package list on your system:

```bash
sudo apt-get update
```

### Step 3: Install dependencies

Next, install the necessary dependencies for Docker installation:

```bash
sudo apt-get install -y ca-certificates curl gnupg
```

### Step 4: Add Docker’s official GPG key

To ensure the security of the Docker packages, we need to add Docker’s official GPG key.

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### Step 5: Add Docker repository

We need to add Docker's official repository to our system to install Docker.

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo $VERSION_CODENAME) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Step 6: Install Docker

Now, update the package list again and install Docker:

```bash
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Step 7: Verify Docker Installation

To verify Docker has been installed correctly, run:

```bash
sudo docker run hello-world
```

This command will download a test image and print a success message if everything is installed correctly.

### Step 8: Run Docker without `sudo` (optional)

By default, Docker requires `sudo` to run. If you want to run Docker as your regular user without `sudo`, run the following command:

```bash
sudo usermod -aG docker $USER
```

After that, log out and log back in to apply the changes, or you can run:

```bash
newgrp docker
```

To verify if you can run Docker without `sudo`, try:

```bash
docker run hello-world
```

### Step 9: Enable Docker to start on boot

To ensure Docker starts automatically when your system boots, enable the Docker service:

```bash
sudo systemctl enable docker
```

### Step 10: Start Docker

Start the Docker service if it's not already running:

```bash
sudo systemctl start docker
```

### Step 11: Check Docker service status

Finally, check the status of Docker to ensure it's running:

```bash
systemctl status docker
```

---

### Uninstalling Docker (if needed)

If you need to uninstall Docker, run the following:

```bash
sudo apt-get purge -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker /var/lib/containerd
```

This will remove Docker and all related files.

---

#### Step 1: Install Docker Compose (if missing)

If Docker Compose is not installed, follow these steps to install it:

1. **Download the latest version of Docker Compose:**Replace `v2.17.3` with the latest version if necessary. Check the [﻿Docker Compose releases page](https://github.com/docker/compose/releases) for the most recent version.

```
sudo curl -L "https://github.com/docker/compose/releases/download/v2.17.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

1. **Make Docker Compose executable:**

```
sudo chmod +x /usr/local/bin/docker-compose
```

1. **Verify the installation:**This should output the version of Docker Compose if it is installed correctly.

```
docker-compose --version
```

You're all set! Let me know if you encounter any issues or need further assistance.

## Step 1: create \_.env \_in server folder

at : _**server\.env**_

```
PORT=5000
MONGO_URI=mongodb://mongo:27017/taskdb
```

### Step 2: change the _**docker-compose.yml**_

at _**docker-compose.yml**_

```
services:
mongo:
  image: mongo:6.0
  container_name: mongo
  restart: unless-stopped
  volumes:
    - mongo-data:/data/db
  networks:
    - mern-net

backend:
  build:
    context: ./server
    dockerfile: Dockerfile
  container_name: backend
  restart: unless-stopped
  env_file:
    - ./server/.env
  ports:
    - "5000:5000"
  depends_on:
    - mongo
  networks:
    - mern-net

frontend:
  build:
    context: ./client
    dockerfile: Dockerfile
    args:
      VITE_API_URL: http://localhost:5000/api
  container_name: frontend
  restart: unless-stopped
  ports:
    - "5173:5173"
  depends_on:
    - backend
  networks:
    - mern-net

volumes:
mongo-data:

networks:
mern-net:
  driver: bridge
```

### Step 3: Running the Complete Application

#### Step 3.1: Build and start everything

```
docker compose up --build -d
```

#### Step 3.2: Verify all containers are running

```
docker ps
```

You should see three containers: mongo, backend, and frontend.

#### Step 3.3: check the volumes list

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker ps`
CONTAINER ID   IMAGE                                   COMMAND                  CREATED         STATUS         PORTS                                         NAMES
1509f3ab128f   sangamdevopssourcecodepart-2-frontend   "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:5173->5173/tcp, [::]:5173->5173/tcp   frontend
7545645a9622   sangamdevopssourcecodepart-2-backend    "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:5000->5000/tcp, [::]:5000->5000/tcp   backend
65cffc3a8093   mongo:6.0                               "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   27017/tcp                                     mongo
```

#### Step 3.4: inspect the volume

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker volume inspect sangamdevopssourcecodepart-2_mongo-data`
[
    {
        "CreatedAt": "2025-10-17T16:03:10Z",
        "Driver": "local",
        "Labels": {
            "com.docker.compose.config-hash": "698999aee1e1706b77454cd06993abc7cce8fc85851834da62135f87f3c78118",
            "com.docker.compose.project": "sangamdevopssourcecodepart-2",
            "com.docker.compose.version": "2.40.0",
            "com.docker.compose.volume": "mongo-data"
        },
        `"Mountpoint": "/var/lib/docker/volumes/sangamdevopssourcecodepart-2_mongo-data/_data",`
        "Name": "sangamdevopssourcecodepart-2_mongo-data",
        "Options": null,
        "Scope": "local"
    }
]
```

### Step 4: Test the application

- Open [﻿http://localhost:5173](http://localhost:5173/) for the frontend
- Test API directly: [﻿http://localhost:5000/api/tasks](http://localhost:5000/api/tasks)

#### Step 4.2: Test the volume persistancy (by removing backend)

```
docker compose stop backend
docker compose rm -f backend
```

#### Step 4.3: restart backend

```
docker compose up -d backend
```

### [﻿ Step 5: Pushing Images to Registry](https://www.notion.so/sangam-devops-part-2-28f714dc6fc480c283b9e1b46a4b2709?source=copy_link#28f714dc6fc481159464db540866a839)

#### Step 5.1: build the images with proper tag (frontend+backend)

_**command ⬇️⬇️**_

```
docker build -t mern-backend:local ./server
docker build -t mern-frontend:local ./client --build-arg VITE_API_URL=http://localhost:5000/api
```

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker build -t mern-backend:local ./server`
[+] Building 3.1s (11/11) FINISHED                                                                                                                                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                     0.0s
 => => transferring dockerfile: 161B                                                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/node:20-alpine                                                                                                                        2.0s
 => [auth] library/node:pull token for registry-1.docker.io                                                                                                                              0.0s
 => [internal] load .dockerignore                                                                                                                                                        0.0s
 => => transferring context: 127B                                                                                                                                                        0.0s
 => [1/5] FROM docker.io/library/node:20-alpine@sha256:1ab6fc5a31d515dc7b6b25f6acfda2001821f2c2400252b6cb61044bd9f9ad48                                                                  0.0s
 => => resolve docker.io/library/node:20-alpine@sha256:1ab6fc5a31d515dc7b6b25f6acfda2001821f2c2400252b6cb61044bd9f9ad48                                                                  0.0s
 => [internal] load build context                                                                                                                                                        0.0s
 => => transferring context: 368B                                                                                                                                                        0.0s
 => CACHED [2/5] WORKDIR /app                                                                                                                                                            0.0s
 => CACHED [3/5] COPY package*.json ./                                                                                                                                                   0.0s
 => CACHED [4/5] RUN npm install                                                                                                                                                         0.0s
 => CACHED [5/5] COPY  . .                                                                                                                                                               0.0s
 => exporting to image                                                                                                                                                                   0.8s
 => => exporting layers                                                                                                                                                                  0.0s
 => => exporting manifest sha256:642afd8df9c9a6d3de71c21cd449dc596c55c3b274efb7371c7141becf48c7fe                                                                                        0.0s
 => => exporting config sha256:42ecc2627351e57d4da7bdc70c266bc469558bdba6e27eadd5b6baa9455fbac1                                                                                          0.0s
 => => exporting attestation manifest sha256:56e476f9bca8bc2dbdfcc3c42ab5553351bfed768bc86b4ae81dcc1fe05ff82b                                                                            0.0s
 => => exporting manifest list sha256:b9085a651a613c22ba2915a154634c5e72065ce69d82da3cae5d702e5b533ded                                                                                   0.0s
 => => naming to docker.io/library/mern-backend:local                                                                                                                                    0.0s
 => => unpacking to docker.io/library/mern-backend:local                                                                                                                                 0.7s

ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker build -t mern-frontend:local ./client --build-arg VITE_API_URL=http://localhost:5000/api`
[+] Building 7.1s (11/11) FINISHED                                                                                                                                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                     0.0s
 => => transferring dockerfile: 235B                                                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/node:20-alpine                                                                                                                        5.4s
 => [internal] load .dockerignore                                                                                                                                                        0.0s
 => => transferring context: 118B                                                                                                                                                        0.0s
 => [1/6] FROM docker.io/library/node:20-alpine@sha256:1ab6fc5a31d515dc7b6b25f6acfda2001821f2c2400252b6cb61044bd9f9ad48                                                                  0.0s
 => => resolve docker.io/library/node:20-alpine@sha256:1ab6fc5a31d515dc7b6b25f6acfda2001821f2c2400252b6cb61044bd9f9ad48                                                                  0.0s
 => [internal] load build context                                                                                                                                                        0.0s
 => => transferring context: 895B                                                                                                                                                        0.0s
 => CACHED [2/6] WORKDIR /app                                                                                                                                                            0.0s
 => CACHED [3/6] COPY package*.json ./                                                                                                                                                   0.0s
 => CACHED [4/6] RUN npm install                                                                                                                                                         0.0s
 => CACHED [5/6] COPY . .                                                                                                                                                                0.0s
 => CACHED [6/6] RUN npm run build                                                                                                                                                       0.0s
 => exporting to image                                                                                                                                                                   1.2s
 => => exporting layers                                                                                                                                                                  0.0s
 => => exporting manifest sha256:8c385cf64abd0d8ed7e91653cf6c262be51e9a19d37313e12a27a2ccb97612b0                                                                                        0.0s
 => => exporting config sha256:2632b5e71d234c1248ffd85e9cd46a5c2df5a744121e1015cb16ef066d2087a6                                                                                          0.0s
 => => exporting attestation manifest sha256:7a299c166f868c4f8e80959ceb14a1b039baaa6cfaa8f0bf66c1c17bdea51b6f                                                                            0.0s
 => => exporting manifest list sha256:d1e7af1817cd37a9178f47f8d992847c8a6480fe3f205b01f3b75a365b7c69c2                                                                                   0.0s
 => => naming to docker.io/library/mern-frontend:local                                                                                                                                   0.0s
 => => unpacking to docker.io/library/mern-frontend:local
```

#### Step 5.2: give Tag for Docker Hub (frontend+backend)

```
`format`
docker tag localImageNameWithTag username/hubImageNameWithTag
```

```
docker tag mern-backend:local abuabddullah/mern-backend:1.0
docker tag mern-frontend:local abuabddullah/mern-frontend:1.0
```

_**output ⬇️⬇️**_

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker tag mern-backend:local abuabddullah/mern-backend:1.0`

ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker tag mern-frontend:local abuabddullah/mern-frontend:1.0`
```

#### Step 5.3: docker login (for first time)

```
docker login
```

_**output ⬇️⬇️**_

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker login`
Authenticating with existing credentials... [Username: abuabddullah]

i Info → To login with a different account, run 'docker logout' followed by 'docker login'


Login Succeeded
```

#### Step 5.3: push both images to docker hub

_**command ⬇️⬇️**_

```
docker push abuabddullah/mern-backend:1.0
docker push abuabddullah/mern-frontend:1.0
```

_**output ⬇️⬇️**_

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker push abuabddullah/mern-backend:1.0`
The push refers to repository [docker.io/abuabddullah/mern-backend]
e3b95a58184d: Pushed
c74c90aa7c87: Pushed
a7c8cd890eeb: Pushed
e65a6806dece: Pushed
1613cb1167d8: Pushed
2d35ebdb57d9: Pushed
90f74824db5a: Pushed
f2fbe8556258: Pushed
c087321cece4: Pushed
1.0: digest: sha256:b9085a651a613c22ba2915a154634c5e72065ce69d82da3cae5d702e5b533ded size: 856

ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker push abuabddullah/mern-frontend:1.0`
The push refers to repository [docker.io/abuabddullah/mern-frontend]
1d4e4d43e07d: Waiting
d54b58b70029: Pushed
c087321cece4: Mounted from abuabddullah/mern-backend
eeee439769c4: Pushed
2d35ebdb57d9: Mounted from abuabddullah/mern-backend
c74c90aa7c87: Mounted from abuabddullah/mern-backend
898ee180d4c6: Pushed
f2fbe8556258: Mounted from abuabddullah/mern-backend
8e2084e73261: Pushed
a7c8cd890eeb: Mounted from abuabddullah/mern-backend
1.0: digest: sha256:d1e7af1817cd37a9178f47f8d992847c8a6480fe3f205b01f3b75a365b7c69c2 size: 856
```

#### Step 5.4: Test deployment from registry

_**delete all the local containers**_

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_9Zp33q3qjECNCQqjMBbzt.png?ixlib=js-3.7.0 "image.png")

_**pull the uploaded images from docker hub**_

```
docker pull abuabddullah/mern-backend:1.0
docker pull abuabddullah/mern-frontend:1.0
```

_**run the uploaded images of docker hub**_

```
docker network create mern-net
docker run -d --name mongo --network mern-net -v mongo-data:/data/db mongo:6.0
docker run -d --name backend --network mern-net \
-e MONGO_URI='mongodb://mongo:27017/taskdb' \
-p 5000:5000 abuabddullah/mern-backend:1.0
docker run -d --name frontend --network mern-net \
-p 5173:5173 abuabddullah/mern-frontend:1.0
```

_**output ⬇️⬇️**_

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker network create mern-net`
`docker run -d --name mongo --network mern-net -v mongo-data:/data/db mongo:6.0`
`docker run -d --name backend --network mern-net \
-e MONGO_URI='mongodb://mongo:27017/taskdb' \
-p 5000:5000 abuabddullah/mern-backend:1.0`
`docker run -d --name frontend --network mern-net \
-p 5173:5173 abuabddullah/mern-frontend:1.0`
Error response from daemon: network with name mern-net already exists
153c5fe9f6e281f1b5f99c228fd31394a7baf8109290375b06f08dc8ab566bd5
3f82ef9d56d0c9c9445cffb40af9a0fb7f01589749abd876c83437be9bc8cba6
8d0cb5daed8119afeb004690e03d0b547f37392cf8f35da887f84f063f763338
```

# [﻿Chapter 5 Jenkins Introduction & Setup](https://www.notion.so/sangam-devops-part-2-28f714dc6fc480c283b9e1b46a4b2709?source=copy_link#28f714dc6fc481debe63e80a8ced4322)

### Step 1: Pull the Jenkins image

_**command(gitbash) ✅✅✅**_

```
docker run -d --name jenkins \
-u 0 \
-p 8080:8080 -p 50000:50000 \
-v "jenkins_home:/var/jenkins_home" \
-v "//./pipe/docker_engine://./pipe/docker_engine" \
jenkins/jenkins:lts
```

_**command(powershell) ❌❌**_

```
docker run -d --name jenkins \
-u 0 \
-p 8080:8080 -p 50000:50000 \
-v jenkins_home:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
jenkins/jenkins:lts
```

### Step 2: install docker inside the jenkins

#### Step 2.1: exec docker in jenkins

```
docker exec -u 0 -it jenkins bash
```

_**output ⬇️⬇️**_

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker exec -u 0 -it jenkins bash`
root@de20c7846aea:/#
```

#### Step 2.2: install dependencies

```
apt-get update && apt-get install -y docker.io
```

_**output⬇️⬇️**_

```
_** root@de20c7846aea:/# **_`_**apt-get update && apt-get install -y docker.io**_`_**
Get:1 http://deb.debian.org/debian trixie InRelease [140 kB]
Get:2 http://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Get:3 http://deb.debian.org/debian-security trixie-security InRelease [43.4 kB]
Get:4 http://deb.debian.org/debian trixie/main amd64 Packages [9669 kB]

****
****

sysctl: permission denied on key "kernel.pid_max"
Processing triggers for libc-bin (2.41-12) ...
Processing triggers for systemd (257.8-1~deb13u2) ...
root@de20c7846aea:/#**_
```

#### Step 2.2.2: install dependencies

```
usermod -aG docker jenkins
```

```
exit
```

_**output⬇️⬇️**_

```
root@de20c7846aea:/# `usermod -aG docker jenkins`
root@de20c7846aea:/# `exit
`exit
```

#### Step 2.3: restart container

```
docker restart jenkins
```

_**output⬇️⬇️**_

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker restart jenkins`
jenkins
```

#### Step 2.4: check docker is available or not inside jenkins

```
docker exec -it jenkins docker --version
```

_**output⬇️⬇️**_

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ docker exec -it jenkins docker --version
Docker version 26.1.5+dfsg1, build a72d7cd
```

#### Step 2.4: check container is available inside the docker

```
docker exec -it jenkins docker ps -a
```

_**output❌error⬇️gitbash**_

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker exec -it jenkins docker ps -a`
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

_**output❌error⬇️gitbash**_

```
PS C:\Users\ooaaow\Desktop\ooaaow2\devops\SANGAM DEVOPS SOURCE CODE PART-2> `docker exec -it jenkins docker ps -a`
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

_**fix ✅✅**_

```
fixed on wsl
```

### Step 2.5: install docker compose in the jinkin's docker

_**executie jenkins ⬇️⬇️**_

```
docker exec -u 0 -it jenkins bash
```

```
mkdir -p /usr/libexec/docker/cli-plugins /usr/local/lib/docker/cli-plugins
```

#### Step 2.5.1: install curl

```
apt-get update && apt-get install -y curl
```

#### Step 2.5.2: download compose v2 binary for the container

```
curl -fL "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/libexec/docker/cli-plugins/docker-compose || \
curl -fL "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/local/lib/docker/cli-plugins/docker-compose
```

_**output ⬇️⬇️⬇️**_

```
root@2c0be9aa1821:/# curl -fL "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/libexec/docker/cli-plugins/docker-compose || \
curl -fL "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" \
  -o /usr/local/lib/docker/cli-plugins/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 73.0M  100 73.0M    0     0  10.5M      0  0:00:06  0:00:06 --:--:-- 13.5M
```

#### Step 2.5.3: make it executable

```
chmod +x /usr/libexec/docker/cli-plugins/docker-compose || chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

_**output ⬇️⬇️⬇️**_

```
root@b8115a1bc444:/# chmod +x /usr/libexec/docker/cli-plugins/docker-compose || chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
root@b8115a1bc444:/#
```

#### Step 2.5.3: show pluging files & check version

```
ls -l /usr/libexec/docker/cli-plugins /usr/local/lib/docker/cli-plugins
docker compose --version || echo "docker compose not available"
```

_**output ⬇️⬇️⬇️**_

```
root@b8115a1bc444:/# `ls -l /usr/libexec/docker/cli-plugins /usr/local/lib/docker/cli-plugins`
`docker compose --version || echo "docker compose not available"`
/usr/libexec/docker/cli-plugins:
total 64360
-rwxr-xr-x 1 root root 65898896 Apr 11  2025 docker-buildx
-rwxr-xr-x 1 root root        0 Oct 21 01:58 docker-compose

/usr/local/lib/docker/cli-plugins:
total 0
Docker version 26.1.5+dfsg1, build a72d7cd
root@b8115a1bc444:/#
```

### Step 2.5.4: check version

```
docker compose version
```

```
root@68b2d37c340d:/# `docker compose --version`
Docker version 26.1.5+dfsg1, build a72d7cd
```

#### Step 2.5.3: check in the url

[﻿localhost:8080/](http://localhost:8080/)

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_w4pQt1yh7R5bAnhSqbH-k.png?ixlib=js-3.7.0 "image.png")

#### Step 2.5.3: exit from the jenkins

```
exit
```

#### Step 2.5.5: get the password

```
docker exec -u 0 -it jenkins bash
cat /var/jenkins_home/secrets/initialAdminPassword
```

```
ooaaow@DESKTOP-2NU0LSC MINGW64 ~/Desktop/ooaaow2/devops/SANGAM DEVOPS SOURCE CODE PART-2
$ `docker exec -u 0 -it jenkins bash`
root@98fc9a0dbc35:/# `cat /var/jenkins_home/secrets/initialAdminPassword`
`8c5ae44b4fd94a8eb7727b9ad57437c1
`root@98fc9a0dbc35:/#
```

#### Step 2.5.6: setpu and install plugins

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_JIYsoe7Boh7qL54HtcqBa.png?ixlib=js-3.7.0 "image.png")

### Step 3: creating a freestyle jobs

1. On the Dashboard, click **New Item** (left sidebar or welcome screen).
2. In the "Enter an item name" field, type a name, e.g., first-job.
3. In the "Copy from" dropdown, select **Freestyle project** (or just check the Freestyle radio button).
4. Click **OK**.
   **Step 3.1: Configure the Project**

5. You're now in the project configuration page.
6. **General tab**: (Optional) Add a description like "My first Jenkins job".
7. Scroll to **Build** section:

   - Check **Add build step** > Select **Execute shell** (for Linux/Unix shell; if Windows, use "Execute Windows batch command").

8. In the **Command** textarea, enter your shell script: textecho "hello this is my first jenkins job"

   - This matches your console output.

9. (Optional) Under **Post-build Actions**, add nothing for now.
10. Click **Save** (bottom of page).
    **Step 3.2: Run (Build) the Project**

11. Back on the project page (http://localhost:8080/job/first-job/), click **Build Now** (top-right, blue play icon).

    - This triggers the job manually.
    - Alternative: Click **Build with Parameters** if you added params (not needed here).

12. A build queue may appear briefly; it starts automatically.
    **Step 3.3: Monitor and View Console Output**

13. Click the build number (e.g., #1) in the "Build History" box (left sidebar).
14. Click **Console Output** (main tab).
15. You'll see output like yours:

```
Started by user Asif A Owadud
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/first-job
[first-job] $ /bin/sh -xe /tmp/jenkins6796887185840995783.sh
+ echo hello this is my first jenkins job
hello this is my first jenkins job
Finished: SUCCESS
```

- **Explanation**:
  - Started by user: Who triggered it.
  - Running as SYSTEM: Jenkins user.
  - Building in workspace: Job's temp dir.
  - $ /bin/sh -xe ...: Shell executes the script (-x for debug trace, -e for error stop).
  - - echo ...: Traced command.
  - Finished: SUCCESS: Job completed without errors.

### Step 4: creating a freestyle jobs with parameters

_**multiple parameter can be added**_

![screencapture-localhost-8080-job-second-job-configure-2025-10-21-21_48_59.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/screencapture-localhost-8080-job-second-job-configure-2025-10-21-21_48_59_fPpEAkh86jf7Si44GgQMl.png?ixlib=js-3.7.0 "screencapture-localhost-8080-job-second-job-configure-2025-10-21-21_48_59.png")

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_sNRlmy6rTD1FSNnRzwyIG.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_4MYeVOK1QvnHHQEIsbuaU.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_rW7lU_yXI0SuUDrqCzlvB.png?ixlib=js-3.7.0 "image.png")

```
Started by user Asif A Owadud
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/second-job
[second-job] $ /bin/sh -xe /tmp/jenkins9504263231109121322.sh
+ echo Deploying to environment : production
Deploying to environment : production
Finished: SUCCESS
```

### Step 4: chaining jobs

**➡️one job is depended on another**

**Step 4.1: Create or Use Job1 (The Triggering Job)**

1. Dashboard > **New Item** (or edit existing first-job).
2. Name: job1-chain > Freestyle project > **OK**.
3. **Build** section > **Add build step** > **Execute shell**:

```
echo "Job1: Building the app..."
sleep 5  # Simulate build time
echo "Job1 complete! Triggering Job2..."
```

1. **Save**.
   **Step 4.2: Create Job2 (The Chained Job)**

1. Dashboard > **New Item**.
1. Name: job2-chain > Freestyle project > **OK**.
1. **Build** section > **Add build step** > **Execute shell**:

```
echo "Job2: Testing the app..."
sleep 3  # Simulate test time
echo "Job2 complete! Deployment ready."
```

1. **Save**.

#### **Step 4.3: Configure Chaining (Job1 Triggers Job2)**

1. Go to **Job1** config (http://localhost:8080/job/job1-chain/configure).
2. Scroll to **Post-build Actions** > **Add post-build action** > **Trigger/call builds on other projects**.
3. **Projects to build**: Enter job2-chain (comma-separated if multiple).
4. (Optional) **Block until the triggered projects finish their builds**: Check for sequential chaining (Job1 waits for Job2).
5. (Optional) **Trigger when build is stable**: Check (default; triggers only on SUCCESS).
6. **Save**.
   **Step 4.4: Run and Verify the Chain**

7. Go to **Job1** page > **Build Now**.
8. Monitor:

   - Job1 console: Shows "Job1 complete! Triggering Job2...".
   - After Job1 finishes: Job2 auto-starts (check Build History on Job2 page).

9. View chained output:

   - Job1 > Build #1 > Console Output: Ends with trigger log.
   - Job2 > Build #1 > Console Output: Shows "Job2: Testing the app...".

10. Full chain status: Job1 page > Build #1 > **Changes** or **Triggered Projects** (shows Job2 link).
    ![screencapture-localhost-8080-job-first-job-configure-2025-10-21-21_58_22.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/screencapture-localhost-8080-job-first-job-configure-2025-10-21-21_58_22_MZH0ManvuCF_lQuC7kuE4.png?ixlib=js-3.7.0 "screencapture-localhost-8080-job-first-job-configure-2025-10-21-21_58_22.png")

_**it triggers the second job⬇️⬇️**_

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_WIQjaUL9KNbLtBEYPZx9x.png?ixlib=js-3.7.0 "image.png")

### Step 5: job with artifacts

![screencapture-localhost-8080-job-third-job-with-artifacts-configure-2025-10-21-22_27_49.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/screencapture-localhost-8080-job-third-job-with-artifacts-configure-2025-10-21-22_27_49_uvFP3jEKtoWsgdDU6O2JC.png?ixlib=js-3.7.0 "screencapture-localhost-8080-job-third-job-with-artifacts-configure-2025-10-21-22_27_49.png")

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_Rm2SxPpxKB4cvIfPsnOjq.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_0rSS_lSxJLaAfbVpDQUmF.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_Xt-j1Vnr6rQvBr_uzuaoC.png?ixlib=js-3.7.0 "image.png")

### Step 6: create a job with git repo

_**command for exec shell ⬇️⬇️**_

```
echo "cloned Repo content"
ls -l
echo
echo "README file content:"
cat README
```

![screencapture-localhost-8080-job-4th-job-with-git-repo-configure-2025-10-21-22_45_12.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/screencapture-localhost-8080-job-4th-job-with-git-repo-configure-2025-10-21-22_45_12_O8O33PAt13ZcaQfmcB7cv.png?ixlib=js-3.7.0 "screencapture-localhost-8080-job-4th-job-with-git-repo-configure-2025-10-21-22_45_12.png")

![screencapture-localhost-8080-job-4th-job-with-git-repo-1-console-2025-10-21-22_45_53.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/screencapture-localhost-8080-job-4th-job-with-git-repo-1-console-2025-10-21-22_45_53_RzbmZVNvZyAMKOqlOuGsZ.png?ixlib=js-3.7.0 "screencapture-localhost-8080-job-4th-job-with-git-repo-1-console-2025-10-21-22_45_53.png")

## Step 6: real jenkins jobs with pipeline and mern

#### step 6.1 : create a \_Jenkinsfile \_at root

```
pipeline {
  agent any

  environment {
    FRONTEND_IMAGE = "mern-frontend:jenkins"
    BACKEND_IMAGE  = "mern-backend:jenkins"
    PORT = "5000"
    MONGO_URI = "mongodb://mongo:27017/taskdb"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/abuabddullah/devops-asif.git', branch: 'main'
      }
    }

    stage('Prepare .env') {
      steps {
        sh '''
          mkdir -p server
          cat > server/.env <<EOF
PORT=$PORT
MONGO_URI=$MONGO_URI
EOF
        '''
      }
    }

    stage('Build Docker Images') {
      steps {
        sh '''
          echo "Building backend image..."
          docker build -t $BACKEND_IMAGE ./server

          echo "Building frontend image..."
          docker build -t $FRONTEND_IMAGE ./client --build-arg VITE_API_URL=http://localhost:5000/api
        '''
      }
    }

    stage('Run with Docker Compose') {
      steps {
        sh '''
          echo "Starting MERN stack with Docker Compose..."
          docker compose up -d

          echo "Showing running containers..."
          docker ps

          echo "===== Backend Logs ====="
          docker logs backend || true

          echo "===== Frontend Logs ====="
          docker logs frontend || true
        '''
      }
    }
  }
}
```

#### step 6.2 : push the code to github

link : [﻿github.com/abuabddullah/devops-asif.git](https://github.com/abuabddullah/devops-asif.git)

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_5sUf3Jm1UiG2ZRHUlW8Hb.png?ixlib=js-3.7.0 "image.png")

#### step 6.3 : create a pipeline job in jenkins

![screencapture-localhost-8080-view-all-newJob-2025-10-22-21_35_00.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/screencapture-localhost-8080-view-all-newJob-2025-10-22-21_35_00_cYfXSMkRTFhccJCcl3IaP.png?ixlib=js-3.7.0 "screencapture-localhost-8080-view-all-newJob-2025-10-22-21_35_00.png")

```
H/5 * * * *
```

![screencapture-localhost-8080-job-mern-devops-app-configure-2025-10-22-21_39_55.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/screencapture-localhost-8080-job-mern-devops-app-configure-2025-10-22-21_39_55_pF8malUg3mrZwLhXiRxoM.png?ixlib=js-3.7.0 "screencapture-localhost-8080-job-mern-devops-app-configure-2025-10-22-21_39_55.png")

#### step 6.3 : build

# AWS EC2 based Kubernetes Devops part 3

## Docker - Container to container communication

### Nginx configuration

**✅ create a "nginx/**_**nginx.conf" **_**file at root**

➡️➡️ ignored the "_**localhost" **\_and used the image name like, "_**frontend",**_"_**backend"**\_

```
[ec2-user@ip-172-31-40-6 nginx]$ `cat nginx.conf`
# steps :
# # simple reverse proxy cofig for mern app
# # serves frontend at /
# # proxies /api/* to backend service
# # set headers so that backend can see original host and client IP

events {}

http {
    # Increase client body size if you want to test big payloads
    client_max_body_size 10M;

    server {
        listen 80;

        # When someone visits "/" or homepage, send the request to frontend service
        location / {
            proxy_pass http://frontend:5173;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_buffering off;
        }

        # Proxy /api/* to backend service
        location /api {
            proxy_pass http://backend:5000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_connect_timeout 5s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
        }

        # Health check endpoint
        location = /nginx-health {
            return 200 "ok";
            add_header Content-Type text/plain;
        }
    }
}
```

### Docker Compose configuration

**✅ create a "**_**docker-compose.yml" **_**file at root**

➡️➡️ ports removed from both frontend and backend services

➡️➡️ frontend service's args changed to **"/api"** _(previously : http://localhost:5000)_

```
[ec2-user@ip-172-31-40-6 mern-docker-kubernetes]$ `cat docker-compose.yml`
services:
  mongo:
    image: mongo:6.0
    container_name: mongo
    restart: unless-stopped
    volumes:
      - mongo-data:/data/db
    networks:
      - mern-net

  backend:
    build:
      context: ./server
      dockerfile: Dockerfile
    container_name: backend
    restart: unless-stopped
    env_file:
      - ./server/.env
    depends_on:
      - mongo
    networks:
      - mern-net

  frontend:
    build:
      context: ./client
      dockerfile: Dockerfile
      args:
        VITE_API_URL: "/api"
    container_name: frontend
    restart: unless-stopped
    depends_on:
      - backend
    networks:
      - mern-net

  nginx:
    image: nginx:alpine
    container_name: nginx
    restart: unless-stopped
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    ports:
      - "80:80"
    depends_on:
      - frontend
      - backend
    networks:
      - mern-net

volumes:
  mongo-data:

networks:
  mern-net:
    driver: bridge
```

**✅ run command**

```
sudo docker compose up --build
```

**এরর for ec2 but local wsl fine ❌❌❌ | fix ✅⬇️ | better আবারো chatgpt কে ask করা**

```
ঠিক আছে! আমি তোমার জন্য একদম বিস্তারিত **step-by-step special note** বানাচ্ছি, যেখানে দেখানো হবে — `docker-compose.yml` define করার পরে কী কী extra steps করতে হয়েছে, কেন, এবং কী purpose ছিল।

---

# 📝 Docker Compose Deployment – Special Notes (Amazon Linux 2023 / EC2)

## 1️⃣ Docker Compose Default Behavior

* তোমার `docker-compose.yml` define করা হয়েছে ৪টি services এর জন্য:

```

mongo, backend, frontend, nginx

````
* By default, Docker Compose:

* Pull করে image যদি pre-built হয়
* Build করে container যদি `build` context দেওয়া থাকে
* **Problem**: Amazon Linux 2023-এর default Docker installation **Buildx plugin missing** → frontend/backend build হয়নি।

---

## 2️⃣ Required Extra Steps to Make All Containers Build & Run

### Step 1: Install Docker CLI Buildx Plugin (manual)

* **Why:** Frontend/backend build করতে BuildKit/Buildx প্রয়োজন।
* **Command (manual download & install):**

```bash
mkdir -p ~/.docker/cli-plugins/
curl -L https://github.com/docker/buildx/releases/download/v0.17.1/buildx-v0.17.1.linux-amd64 -o ~/.docker/cli-plugins/docker-buildx
chmod +x ~/.docker/cli-plugins/docker-buildx
docker buildx version   # Verify version ≥ 0.17
````

- **Note:** প্রথমে `curl` failed হচ্ছিল কারণ folder ownership root ছিল → `chown` দিয়ে fix করতে হয়েছিল।

```bash
sudo chown -R $USER:$USER ~/.docker
```

---

### Step 2: Move Buildx to System Path (recommended)

- **Why:** Docker CLI যখন `sudo docker compose` run করবে তখন user context `.docker/cli-plugins` পড়বে না।
- **Command:**

```bash
sudo mkdir -p /usr/libexec/docker/cli-plugins/
sudo mv ~/.docker/cli-plugins/docker-buildx /usr/libexec/docker/cli-plugins/docker-buildx
sudo chmod +x /usr/libexec/docker/cli-plugins/docker-buildx
```

---

### Step 3: Enable BuildKit for Docker Compose

- **Why:** Modern Docker builds require BuildKit for multi-stage builds and `build` contexts.
- **Command:**

```bash
export DOCKER_BUILDKIT=1
export COMPOSE_DOCKER_CLI_BUILD=1
```

---

### Step 4: Rebuild All Containers

- **Why:** আগের attempt-এ frontend/backend build হয়নি।
- **Command:**

```bash
sudo docker compose build --no-cache
sudo docker compose up -d
```

---

### Step 5: Verify Containers

- **Command:**

```bash
docker ps -a
```

- **Expected output:**

```
mongo      Up
backend    Up
frontend   Up
nginx      Up
```

---

### Step 6: Optional Checks

- Logs:

```bash
docker compose logs -f
```

- Restart:

```bash
sudo docker compose restart
```

- Stop / remove:

```bash
sudo docker compose down
```

---

## 3️⃣ Key Notes / Lessons Learned

| Issue                                                  | Fix                                                          | Purpose                                                   |
| ------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------------- |
| `compose build requires buildx 0.17 or later`          | Manual download + move to `/usr/libexec/docker/cli-plugins/` | Enable Docker Compose to use BuildKit for `build` context |
| `curl: (23) client returned ERROR`                     | Fix folder permission with `chown`                           | User can write binary file                                |
| `sudo docker compose up` frontend/backend not building | Enable BuildKit environment variables                        | Required for modern builds                                |
| Permission denied `/var/run/docker.sock`               | Use `sudo` when needed                                       | Access Docker daemon                                      |
| Empty `docker ps -a`                                   | Rebuild with `--no-cache`                                    | Force build all services                                  |

---

💡 **Conclusion:**

Amazon Linux 2023-এর default Docker setup **mongo/nginx pull করতে পারলেও**, frontend/backend build করতে **Buildx + BuildKit + proper permissions** লাগে।
একবার সব extra steps করা হলে, **docker-compose.yml** define করার পর থেকে সব ৪টি container (mongo, backend, frontend, nginx) **successfully build & run** হয়ে যায়।

---

যদি চাও আমি এখন তোমার জন্য **একটা single copy-paste script** বানিয়ে দিতে পারি যা **Amazon Linux 2023 EC2**-এ একবারে সব extra setup + build + up করবে — আর তোমাকে আর কোনো manual step নিতে হবে না।

চাও আমি সেটা বানিয়ে দিই?

```
### AWS EC2 configuration
✅ **set inboud rules for the ports in the ec2 instances**

![image.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/image_Zc9vLanP1DhdwCYNuD7z1.png?ixlib=js-3.7.0 "image.png")



_**➡️➡️ check(ec2) : **_[_**﻿13.212.94.28/**_](http://13.212.94.28/)_**  | **_[_**﻿13.212.94.28/api/tasks**_](http://13.212.94.28/api/tasks)_** **_

_**➡️➡️ check (local ec2): **_[﻿localhost](http://localhost/)[﻿localhost/api/tasks](http://localhost/api/tasks)_** **_

## Kubernetes Declarative
### Build images for K8S
**✅ for backend**

➡️➡️run command from root

```

docker build -t backend:local ./server

```
**✅ for frontend**

➡️➡️run command from root

```

❌❌docker build --build-arg VITE_API_URL=http://backend/api -t frontend:local ./client
✅✅docker build --build-arg VITE_API_URL=http://localhost:5000/api -t frontend:local ./client

```
### Configuration for K8S
**✅ creating "**_**namespace**_**" file**

➡️➡️ _**namespace **_is like folders

➡️➡️ cerate "_**mern-docker-kubernetes/k8s/00-namespace.yaml**_** **"

```

apiVersion: v1
kind: Namespace
metadata:
name: devops-part3

```
**✅ creating "**_**deployment+service**_**" file for mongo**

➡️➡️cerate "_**mern-docker-kubernetes/k8s/01-mongo.yaml**_"

```

apiVersion: apps/v1
kind: Deployment
metadata:
name: mongo
namespace: devops-part3
labels:
app: mongo
spec:
replicas: 1
selector:
matchLabels:
app: mongo
template:
metadata:
labels:
app: mongo
spec:
containers: - name: mongo
image: mongo:6.0
ports: - containerPort: 27017
volumeMounts: - name: mongo-data
mountPath: /data/db
volumes: - name: mongo-data
emptyDir: {}

---

apiVersion: v1
kind: Service
metadata:
name: mongo
namespace: devops-part3
spec:
selector:
app:mongo
ports: - port: 27017
targetPort: 27017
type: ClusterIP

```
**✅ creating "**_**deployment+service**_**" file for backend**

➡️➡️cerate "_**mern-docker-kubernetes/k8s/02-backend.yaml**_"

```

apiVersion: apps/v1
kind: Deployment
metadata:
name: backend
namespace: devops-part3
labels:
app: backend
spec:
replicas: 2
selector:
matchLabels:
app: backend
template:
metadata:
labels:
app: backend
spec:
containers: - name: backend
image: backend:local
imagePullPolicy: IfNotPresent
ports: - containerPort: 5000
env: - name: MONGO_URI
value: "mongodb://mongo:27017/taskdb"
readinessProbe:
httpGet:
path: /health
port: 5000
initialDelaySeconds: 3
periodSeconds: 5
failureThreshold: 3
livenessProbe:
httpGet:
path: /health
port: 5000
initialDelaySeconds: 10
periodSeconds: 10
failureThreshold: 3

---

apiVersion: v1
kind: Service
metadata:
name: backend
namespace: devops-part3
spec:
selector:
app: backend
ports: - protocol: TCP

- port: 5000
  targetPort: 5000
  type: ClusterIP

```
**✅ creating "**_**deployment+service**_**" file for frontend**

➡️➡️cerate "_**mern-docker-kubernetes/k8s/03-**_**frontend**_**.yaml**_"

```

apiVersion: apps/v1
kind: Deployment
metadata:
name: frontend
namespace: devops-part3
labels:
app: frontend
spec:
replicas: 1
selector:
matchLabels:
app: frontend
template:
metadata:
labels:
app: frontend
spec:
containers: - name: frontend
image: frontend:local # built with VITE_API_URL pointing to backend in-cluster
imagePullPolicy: IfNotPresent
ports: - containerPort: 5173 # vite preview / dev server port

---

apiVersion: v1
kind: Service
metadata:
name: frontend
namespace: devops-part3
spec:
selector:
app: frontend
ports: - protocol: TCP

- port: 5173
  targetPort: 5173
  type: ClusterIP

# we'll port-forward this frontend service to access the UI in browser (http://localhost:5173)

```
### ⭐ run with "_minikube"_
Here’s your corrected and polished version — all typos and formatting fixed for clarity and professional readability 👇

---

### setup kuber netes
**✅install (**_**if needed**_**) kubernetes,kubectl,minkube**

```

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client

````
### 🧩 Step 1: Install required dependencies | ❌ ignore for _wsl _
```bash
sudo yum install -y curl wget conntrack
````

### 🚀 Step 2: Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

✅ **Verify Minikube**

```bash
minikube version
```

### 🐳 Step 4: Start Minikube with Docker driver

(You already have Docker installed, so this works great)

```bash
minikube start --driver=docker
```

### 🔍 Step 5: Check cluster status

```bash
minikube status
```

✅ Expected Output:

```
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

### ⚙️ Configure for K8S (Kubernetes)

**✅ Load images into Minikube**

➡️ Run the following commands step by step:

```bash
minikube image load backend:local
minikube image load frontend:local
minikube image ls
```

**Deploy Mongo**

```bash
kubectl apply -f k8s/00-namespace.yaml
kubectl apply -f k8s/01-mongo.yaml
```

```
kubectl -n devops-part3 wait --for=condition=ready pod -l app=mongo --timeout=180s
```

```
kubectl -n devops-part3 get pods -l app=mongo -o wide
```

**Deploy Backend**

---

```
kubectl apply -f k8s/02-backend.yaml
```

```
kubectl -n devops-part3 wait --for=condition=ready pod -l app=backend --timeout=180s
```

```
kubectl -n devops-part3 get pods -l app=backend -o wide
```

```
kubectl -n devops-part3 get svc backend -o wide
```

**Deploy Frontend**

---

```
kubectl apply -f k8s/03-frontend.yaml
```

```
kubectl -n devops-part3 wait --for=condition=ready pod -l app=frontend --timeout=180s
```

```
kubectl -n devops-part3 get pods -l app=frontend -o wide
```

```
kubectl -n devops-part3 get svc frontend -o wide
```

**Check**

---

```
kubectl -n devops-part3 get pods,svc -o wide
```

---

### **Port forwarding**

_**➡️ backend(different terminal)**_

```
kubectl -n devops-part3 port-forward svc/backend 5000:5000
```

_**➡️ frontend(different terminal)**_

```
kubectl -n devops-part3 port-forward svc/frontend 5173:5173
```

### check : [﻿localhost:5173/](http://localhost:5173/) | [﻿localhost:5000/api/tasks](http://localhost:5000/api/tasks)

### **Instruction**

_**➡️ **\_\_pause running containers_

_➡️ check by the ip_

_➡️ still project is running from kubernetes_

### **Total code and terminal res ⬇️⬇️⬇️**

```
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `docker build -t backend:local ./server`
[+] Building 2.0s (11/11) FINISHED                                                           docker:default
 => [internal] load build definition from Dockerfile                                                   0.0s
 => => transferring dockerfile: 161B                                                                   0.0s
 => [internal] load metadata for docker.io/library/node:20-alpine                                      1.9s
 => [auth] library/node:pull token for registry-1.docker.io                                            0.0s
 => [internal] load .dockerignore                                                                      0.0s
 => => transferring context: 127B                                                                      0.0s
 => [internal] load build context                                                                      0.0s
 => => transferring context: 368B                                                                      0.0s
 => [1/5] FROM docker.io/library/node:20-alpine@sha256:6178e78b972f79c335df281f4b7674a2d85071aae2af02  0.0s
 => CACHED [2/5] WORKDIR /app                                                                          0.0s
 => CACHED [3/5] COPY package*.json ./                                                                 0.0s
 => CACHED [4/5] RUN npm install                                                                       0.0s
 => CACHED [5/5] COPY  . .                                                                             0.0s
 => exporting to image                                                                                 0.0s
 => => exporting layers                                                                                0.0s
 => => writing image sha256:ff909b20e64bb1bb5e0e7f50ebae05756febb796a2f6c2960317731b246b3247           0.0s
 => => naming to docker.io/library/backend:local                                                       0.0s
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `docker build --build-arg VITE_API_URL=http://localhost:5000/api -t frontend:local ./client`
[+] Building 0.5s (11/11) FINISHED                                                           docker:default
 => [internal] load build definition from Dockerfile                                                   0.0s
 => => transferring dockerfile: 235B                                                                   0.0s
 => [internal] load metadata for docker.io/library/node:20-alpine                                      0.4s
 => [internal] load .dockerignore                                                                      0.0s
 => => transferring context: 118B                                                                      0.0s
 => [1/6] FROM docker.io/library/node:20-alpine@sha256:6178e78b972f79c335df281f4b7674a2d85071aae2af02  0.0s
 => [internal] load build context                                                                      0.0s
 => => transferring context: 895B                                                                      0.0s
 => CACHED [2/6] WORKDIR /app                                                                          0.0s
 => CACHED [3/6] COPY package*.json ./                                                                 0.0s
 => CACHED [4/6] RUN npm install                                                                       0.0s
 => CACHED [5/6] COPY . .                                                                              0.0s
 => CACHED [6/6] RUN npm run build                                                                     0.0s
 => exporting to image                                                                                 0.0s
 => => exporting layers                                                                                0.0s
 => => writing image sha256:7f060693b298be92bba9a1c5e27a5c77a7e88f19ef718b3881576e555b34406f           0.0s
 => => naming to docker.io/library/frontend:local                                                      0.0s
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `minikube version`
minikube version: v1.37.0
commit: 65318f4cfff9c12cc87ec9eb8f4cdd57b25047f3
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl version --client`
Client Version: v1.34.1
Kustomize Version: v5.7.1
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `minikube start --driver=docker`
😄  minikube v1.37.0 on Ubuntu 24.04 (kvm/amd64)
✨  Using the docker driver based on existing profile
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.48 ...
🏃  Updating the running docker "minikube" container ...
🐳  Preparing Kubernetes v1.34.0 on Docker 28.4.0 ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `minikube status`
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `minikube image load backend:local`
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `minikube image load frontend:local`
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `minikube image ls`
registry.k8s.io/pause:3.10.1
registry.k8s.io/kube-scheduler:v1.34.0
registry.k8s.io/kube-proxy:v1.34.0
registry.k8s.io/kube-controller-manager:v1.34.0
registry.k8s.io/kube-apiserver:v1.34.0
registry.k8s.io/etcd:3.6.4-0
registry.k8s.io/coredns/coredns:v1.12.1
gcr.io/k8s-minikube/storage-provisioner:v5
docker.io/library/mongo:6.0
docker.io/library/frontend:local
docker.io/library/backend:local
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl apply -f k8s/00-namespace.yaml`
namespace/devops-part3 unchanged
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl apply -f k8s/01-mongo.yaml`
deployment.apps/mongo unchanged
service/mongo unchanged
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 wait --for=condition=ready pod -l app=mongo --timeout=180s`
pod/mongo-75c5cd55b-8qd2g condition met
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 get pods -l app=mongo -o wide`
NAME                    READY   STATUS    RESTARTS        AGE   IP            NODE       NOMINATED NODE   READINESS GATES
mongo-75c5cd55b-8qd2g   1/1     Running   2 (6m49s ago)   13h   10.244.0.14   minikube   <none>           <none>
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl apply -f k8s/02-backend.yaml`
deployment.apps/backend created
service/backend created
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 wait --for=condition=ready pod -l app=backend --timeout=180s`
pod/backend-5d49554b87-8z599 condition met
pod/backend-5d49554b87-tnhqv condition met
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 get pods -l app=backend -o wide`
NAME                       READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
backend-5d49554b87-8z599   1/1     Running   0          11s   10.244.0.17   minikube   <none>           <none>
backend-5d49554b87-tnhqv   1/1     Running   0          11s   10.244.0.16   minikube   <none>           <none>
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 get svc backend -o wide`
NAME      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE   SELECTOR
backend   ClusterIP   10.110.148.204   <none>        5000/TCP   18s   app=backend
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl apply -f k8s/03-frontend.yaml`
deployment.apps/frontend created
service/frontend created
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 wait --for=condition=ready pod -l app=frontend --timeout=180s`
pod/frontend-7749c8db98-4tsdh condition met
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 get pods -l app=frontend -o wide`
NAME                        READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
frontend-7749c8db98-4tsdh   1/1     Running   0          8s    10.244.0.18   minikube   <none>           <none>
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 get svc frontend -o wide`
NAME       TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE   SELECTOR
frontend   ClusterIP   10.104.24.40   <none>        5173/TCP   12s   app=frontend
ooaaow@DESKTOP-2NU0LSC:/applications/mern-docker-kubernetes$ `kubectl -n devops-part3 get pods,svc -o wide`
NAME                            READY   STATUS    RESTARTS        AGE   IP            NODE       NOMINATED NODE   READINESS GATES
pod/backend-5d49554b87-8z599    1/1     Running   0               39s   10.244.0.17   minikube   <none>           <none>
pod/backend-5d49554b87-tnhqv    1/1     Running   0               39s   10.244.0.16   minikube   <none>           <none>
pod/frontend-7749c8db98-4tsdh   1/1     Running   0               16s   10.244.0.18   minikube   <none>           <none>
pod/mongo-75c5cd55b-8qd2g       1/1     Running   2 (7m33s ago)   13h   10.244.0.14   minikube   <none>           <none>

NAME               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)     AGE   SELECTOR
service/backend    ClusterIP   10.110.148.204   <none>        5000/TCP    39s   app=backend
service/frontend   ClusterIP   10.104.24.40     <none>        5173/TCP    16s   app=frontend
service/mongo      ClusterIP   10.99.143.223    <none>        27017/TCP   13h   app=mongo
```

### _**🗑️🗑️Delete any deploy**_

### 🧩 Step 1: Backend ও Frontend Deployment delete করো

প্রথমে Kubernetes থেকে backend আর frontend deployment এবং service মুছে ফেলো:

```bash
kubectl -n devops-part3 delete deploy backend frontend
kubectl -n devops-part3 delete svc backend frontend
```

এটা করলে সেই দুইটা container আর run করবে না, ফলে image release হবে।

---

### 🧹 Step 2: এখন image remove করো

এখন images remove করা যাবে force ছাড়াই 👇

```bash
minikube image rm frontend:local
minikube image rm backend:local
```

যদি এখনও error দেয়, তাহলে force remove করতে পারো:

```bash
minikube ssh
docker rmi -f frontend:local
docker rmi -f backend:local
exit
```

> `minikube ssh` দিয়ে তুমি minikube container-এর ভিতরে ঢুকে `docker rmi -f` কমান্ড চালাচ্ছো, যা একদম নিশ্চিতভাবে image delete করবে।

---

### ✅ Step 3: যাচাই করো

```bash
minikube image ls
```

এখানে `frontend:local` এবং `backend:local` আর দেখা যাবে না।

---

### 🚀 Step 4 (optional): নতুন করে build করতে চাইলে

```bash
docker build -t frontend:local ./client
docker build -t backend:local ./server
minikube image load frontend:local
minikube image load backend:local
```

## Configuration and secrets in kubernetes (_.env)_

### instructions

🗑️ delete all old images, containers

### Build images for K8S

**✅ for backend**

➡️➡️run command from root

```
docker build -t backend:configured ./server
```

**✅ for frontend**

➡️➡️run command from root

```
docker build --build-arg VITE_API_URL=http://localhost:5000/api -t frontend:configured ./client
```

### Configmap

**✅ creating **_**configmap.yaml**_** file**

➡️➡️create file at "_**k8s/configmap.yaml**_"

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: devops-part3
data:
  MONGO_HOST: "mongo"
  FRONTEND_URL: "http://localhost:5173"
```

**✅ creating **_**secret-mongo.yaml**_** file**

➡️➡️get code from (_**linux root**_) for this(_**secret-mongo.yaml**_) file

```
echo -n "mongodb://mongo:27017/taskdb" | base64
```

```
ooaaow@DESKTOP-2NU0LSC:~$ `echo -n "mongodb://mongo:27017/taskdb" | base64`
 bW9uZ29kYjovL21vbmdvOjI3MDE3L3Rhc2tkYg==
```

➡️➡️create file at "_**secret-mongo.yaml**_"

```
apiVersion: v1
kind: Secret
metadata:
  name: mongo-secret
  namespace: devops-part3
type: Opaque
data:
  MONGO_URI: bW9uZ29kYjovL21vbmdvOjI3MDE3L3Rhc2tkYg==
```

### Rebuild Images with configured _**.yaml**_

**✅ creating "**_**deployment+service**_**" file for backend**

➡️➡️cerate "_**mern-docker-kubernetes/k8s/02.2-backend-configured.yaml**_"

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  namespace: devops-part3
  labels:
    app: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
        - name: backend
          image: backend:configured
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 5000
          envFrom:
            - configMapRef:
                name: app-config
            - secretRef:
                name: mongo-secret
          readinessProbe:
            httpGet:
              path: /health
              port: 5000
            initialDelaySeconds: 3
            periodSeconds: 5
            failureThreshold: 3
          livenessProbe:
            httpGet:
              path: /health
              port: 5000
            initialDelaySeconds: 10
            periodSeconds: 10
            failureThreshold: 3
---
apiVersion: v1
kind: Service
metadata:
  name: backend
  namespace: devops-part3
spec:
  selector:
    app: backend
  ports:
    - protocol: TCP
      port: 5000
      targetPort: 5000
  type: ClusterIP
```

**✅ creating "**_**deployment+service**_**" file for frontend**

➡️➡️cerate "_**mern-docker-kubernetes/k8s/03.2-frontend-configured.yaml**_"

➡️➡️➡️ image এর না্ম ছাড়া আর change নাই

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: devops-part3
  labels:
    app: frontend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend
          image: frontend:configured # built with VITE_API_URL pointing to backend in-cluster
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 5173 # vite preview / dev server port
---
apiVersion: v1
kind: Service
metadata:
  name: frontend
  namespace: devops-part3
spec:
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 5173
      targetPort: 5173
  type: ClusterIP
```

**✅ creating "**_**deployment+service**_**" file for mongo**

➡️➡️cerate "_**mern-docker-kubernetes/k8s/01-mongo.yaml**_" will be same

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
  namespace: devops-part3
  labels:
    app: mongo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
        - name: mongo
          image: mongo:6.0
          ports:
            - containerPort: 27017
          volumeMounts:
            - name: mongo-data
              mountPath: /data/db
      volumes:
        - name: mongo-data
          emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: mongo
  namespace: devops-part3
spec:
  selector:
    app:mongo
   ports:
     - port: 27017
       targetPort: 27017
   type: ClusterIP
```

**✅ creating "**_**deployment+service**_**" file for **_**namespace**_

➡️➡️cerate "_**mern-docker-kubernetes/k8s/00-namespace.yaml**_" will be same

```
apiVersion: v1
kind: Namespace
metadata:
  name: devops-part3
```

### ⚙️ Configure for K8S (Kubernetes with configs and secrets)

**✅ Load images into Minikube**

➡️ Run the following commands step by step:

```bash
minikube image load backend:configured
minikube image load frontend:configured
minikube image ls
```

**Deploy Mongo**

```bash
kubectl apply -f k8s/00-namespace.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secret-mongo.yaml
```

```
kubectl -n devops-part3 get configmap app-config -o yaml
```

```
kubectl -n devops-part3 get secret mongo-secret -o yaml
```

```
kubectl apply -f k8s/01-mongo.yaml
```

**wait for mongo pod to be ready (block untill ready or timeout)**

```
kubectl -n devops-part3 wait --for=condition=ready pod -l app=mongo --timeout=180s
```

```
kubectl -n devops-part3 get pods -l app=mongo -o wide
```

**Deploy Backend**

---

```
kubectl apply -f k8s/02.2-backend-configured.yaml
```

```
kubectl -n devops-part3 wait --for=condition=ready pod -l app=backend --timeout=180s
```

```
kubectl -n devops-part3 get pods -l app=backend -o wide
```

```
kubectl -n devops-part3 get svc backend -o wide
```

**Deploy Frontend**

---

```
kubectl apply -f k8s/03.2-frontend-configured.yaml
```

```
kubectl -n devops-part3 wait --for=condition=ready pod -l app=frontend --timeout=180s
```

```
kubectl -n devops-part3 get pods -l app=frontend -o wide
```

```
kubectl -n devops-part3 get svc frontend -o wide
```

**Check**

➡️➡️ show pods and services in the namespace with details

---

```
kubectl -n devops-part3 get pods,svc -o wide
```

---

### **Port forwarding**

_**➡️ backend(different terminal)**_

```
kubectl -n devops-part3 port-forward svc/backend 5000:5000
```

_**➡️ frontend(different terminal)**_

```
kubectl -n devops-part3 port-forward svc/frontend 5173:5173
```

### check : [﻿localhost:5173/](http://localhost:5173/) | [﻿localhost:5000/api/tasks](http://localhost:5000/api/tasks)

### **Instruction**

_**➡️ **\_\_pause running containers_

_➡️ check by the ip_

_➡️ still project is running from kubernetes_

# Load balancing with Ingress _e.g nginx_

### Recap ( cluster Ip, node port, prot-forwarding)

**➡️**[﻿app.youlearn.ai/learn/content/ZtGxIyJNE7CFsYc](https://app.youlearn.ai/learn/content/ZtGxIyJNE7CFsYc)

**Kubernetes Ingress** is a crucial component that acts as a **smart router** at the edge of your cluster, providing a single, scalable entry point for external traffic to reach your services. This mechanism solves the limitations of other Kubernetes service types for production environments.

---

## Limitations of Standard Service Types

Before Ingress, exposing applications to the outside world using standard service types presented several challenges:

### ClusterIP

- **Function:** This is the default service type that Kubernetes assigns when a service is created.
- **Limitation:** The assigned IP is **only accessible inside the cluster network** and is perfect for internal pod-to-pod communication, but it cannot be accessed from the outside world.

### NodePort

- **Function:** Exposes the service on a specific port across all nodes in the cluster, typically in the high, random range of 30,000 to 32,767.
- **Limitations:**
  - **Not Scalable:** Each service requires a separate, high port, leading to too many ports to manage in a large application with many microservices (e.g., Auth, Payment, Cart services).
  - **No Domain Support:** It is IP and port-based, meaning it does not support domains, which is unrealistic for real-world websites (e.g., `pizza.com:31000` for one service and `pizza.com:32767` for another).
  - **No SSL Termination:** It does not provide a straightforward way to add SSL certificates or terminate HTTPS, requiring manual implementation with an external reverse proxy or load balancer.

### Port Forwarding

- **Function:** Creates a **temporary tunnel** from your local machine to a service inside the cluster.
- **Limitation:** This is only suitable for **debugging** and local development and is not scalable or sharable for production use.

---

## Kubernetes Ingress: The Solution

**Ingress** is a resource that manages external access to services in a cluster, typically HTTP and HTTPS traffic, by providing **protocol-aware configuration** and routing rules.

### How Ingress Works

- It listens on **standard web ports** (80 for HTTP and 443 for HTTPS).
- It takes all incoming requests and, based on **rules you define**, sends them to the correct service (e.g., frontend or backend).
- It provides one **clean domain** (e.g., `my-app.local` ) as the single entry point, which is managed by an **Ingress Controller** (like Nginx) that handles the reverse proxying and routing logic.

### Key Benefits of Using Ingress

- **Clean Entry Point:** You only expose one domain/IP, eliminating the need to expose multiple ports for every service, which is highly scalable.
- **Path-Based Routing:** You can define rules to route traffic based on the URL path (e.g., requests to `/API` go to the backend service, while requests to `/` go to the frontend service).
- **SSL Termination:** It makes HTTPS setup easy by handling the termination of SSL/TLS traffic at the edge of the cluster.
- **Load Balancing:** The Ingress Controller automatically load balances requests across the available pods for a given service.

---

## Ingress Workflow (Traffic Flow)

The flow of a request from a user's browser to a pod and back involves six key steps:

1. **Browser Sends Request:** The user types the URL (e.g., `http://my-app.local/API/task` ), and the browser sends an HTTP GET request with the host header.
2. **Request Reaches Ingress Controller:** The request enters the Kubernetes cluster through the **Ingress Controller**, which is positioned at the edge of the cluster (often managed by a Load Balancer).
3. **Ingress Rules are Checked:** The Ingress Controller checks the defined routing rules (e.g., path matches `/API` ) and forwards the request to the appropriate Kubernetes service (e.g., the backend service).
4. **Service Forwards to Pods:** The target service knows which pods are running the corresponding containers (via **labels**) and load balances the request to one of the available pods.
5. **Pods Handle the Request:** The application inside the pod processes the request (e.g., queries the MongoDB database for the `/API/task` endpoint) and generates a response.
6. **Response Flows Back:** The response travels back to the user in the reverse direction: Pod $\rightarrow$ Service $\rightarrow$ Ingress $\rightarrow$ Browser, where the user sees the final result.
   ![Kubernetes Ingress Traffic Flow.png](https://eraser.imgix.net/workspaces/x33nxToEUSJkX5hEuvG9/fv0a2HExkUNpYuXshUAZM4zKYGj2/Kubernetes%20Ingress%20Traffic%20Flow_VfQ1cPhH38aGi_BELyr_r.png?ixlib=js-3.7.0 "Kubernetes Ingress Traffic Flow.png")

# Ingress Based Kubernets Project ⬇️⬇️⬇️

[﻿https://youtu.be/4QJHV2-UPns?si=au-3PTJYBRRAnXxi&t=9044](https://youtu.be/4QJHV2-UPns?si=au-3PTJYBRRAnXxi&t=9044)

### Instructions

➡️delete all the images, namespaces, containers, kubernetes clusters

### sadfsadfsddf

**✅ asdsdfasfasdf**

➡️➡️asdfsdfsdf

```
asdfsdfsdsfsdf
```

### sadfsadfsddf

**✅ asdsdfasfasdf**

➡️➡️asdfsdfsdf

```
asdfsdfsdsfsdf
```
