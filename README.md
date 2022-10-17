# Template DOCKER PHP NGINX & POSTGRES
## 1 - Dockerfile
If you need entrypoint for database migrations remove the comment l.53 in docker/Dockerfile

## 2 - docker-compose
Change NGINX_HOST in docker-compose.yml by the name of your project

## 2 - docker-compose.override.yml
Change PHP_IDE_CONFIG by the NGINX_HOST
Remove the comment from the services you need
But you can add what you need for your project

## 3 - FINAL
YOu can remove this part for the setup template ;)

# Install
## 1 - Docker
You can download and install Docker and Docker-compose depending on your platform :

- https://docs.docker.com/install/
- https://docs.docker.com/compose/install/

Install TaskFile:
 ```bash
 sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d
 
 ## For MacOS only
 brew install go-task/tap/go-task
 ```
For more info about TaskFile, visit: https://taskfile.dev/#/installation

## 2 - Install dependencies
You can install project using docker compose commands `build, up.` or by just running the following task command:

 ```bash
 task install
 
 ## list all tasks
 task
 ```

## 3 - Let's run it
If all your containers are up and running with(`docker compose ps`). You should add a `local.api-suzuki-care.fr` to your `/etc/hosts`
 ```bash
# /etc/hosts
 127.0.0.1 local.project.fr
 ::1 local.project.fr
 ```

## 4 - Check it out
- Website: https://local.lp-syneido.fr

# Tests & Quality
Follow this [link](./doc/tests.md)