# NPM (Nginx, PHP, MariaDB)
NPM is a docker container setup with Nginx, PHP and MariaDB.

This project skeleton provides the possibility to serve a separated frontend like Angular, and a backend like Laravel in the same environment. 

The frontend is then served under the domain "frontend.local" and the backend is served under the domain "backend.local".
For this to work, you have to add new entries for both these domains in your "hosts" file.

## Installation ##
### Download ###
To download this project skeleton, just clone or download this repository.
```bash
user@host:~$ cd my-projects/
user@host:~/my-projects$ git clone https://github.com/CPCoder/npm.git myproject
```
### Configuration ###
To configure the docker environment for this project skeleton, you have to copy the `.env.dist` first.
```bash
user@host:~/my-projects$ cd myproject/
user@host:~/my-projects/myproject$ cp .env.dist .env
```
Now you can open the file `.env` and set your desired options.

### Additional configuration on host system ###
You need to add new entries to your `/etc/hosts` file:
```bash
user@host:$ sudo nano /etc/hosts
```
Content:
```bash
127.0.0.1   frontend.local
127.0.0.1   backend.local
```

### Docker ###
After you're done with the configuration in the `.env` file, you can build the docker environment.
```bash
user@host:~/my-projects/myproject$ docker-compose up -d
```
Once the docker compose process is finished you should see these containers:
```bash
user@host:~/my-projects/myproject$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                                        NAMES
71ad90232272   myproject_revproxy   "/docker-entrypoint.…"   12 minutes ago   Up 12 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp, 443/tcp   revproxy
4e6141352c05   nginx:alpine         "/docker-entrypoint.…"   12 minutes ago   Up 12 minutes   0.0.0.0:8082->80/tcp, :::8082->80/tcp        backend
3ed82f0cfdc8   myproject_php-fpm    "docker-php-entrypoi…"   12 minutes ago   Up 12 minutes   9000/tcp                                     php
294bad6ca447   nginx:alpine         "/docker-entrypoint.…"   12 minutes ago   Up 12 minutes   0.0.0.0:8081->80/tcp, :::8081->80/tcp        frontend
fc3210b8e4ff   mariadb:latest       "docker-entrypoint.s…"   12 minutes ago   Up 12 minutes   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp    maria
```

## Test your environment ##
To test the successful setup of your environment, you just open your web browser and navigate to the domains frontend.local and backend.local.

### Frontend (http://frontend.local) ###
![Frontend web browser screenshot](readme-images/frontend.png?raw=true "Frontend web browser screenshot")
### Backend (index.php) (http://backend.local) ###
![Backend web browser screenshot](readme-images/backend-index.png?raw=true "Backend web browser screenshot (index.php)")
### Backend (_info.php) (http://backend.local/_info.php) ###
![Backend web browser screenshot](readme-images/backend-info.png?raw=true "Backend web browser screenshot (_info.php)")
