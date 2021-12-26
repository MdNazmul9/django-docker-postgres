# Django4_Basic

#### Step 1 : First Install docker & docker-compose (if already installed then skip this step)
```
sudo apt update
sudo apt upgrade
sudo apt install docker.io
```
##### Install Docker compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

## Docker command text to run this project
```
docker build --tag python-django .
```
```
docker run --publish 8000:8000 python-django
```


## Run Using Docker
  - 1. clone the project
  - 2. Place .env File First 
  - 3. Build projct ``` docker-compose build ```
  - 4. Run projct ``` docker-compose up ```
  - 5. Down/Stop projct ``` docker-compose down ```


### Docker Basic Info
![Docker Basic 1.1 ](https://github.com/MdNazmul9/All-Django-Project_Need/blob/main/backend/doc/1.1.png)
![Docker Basic 1.2 ](https://github.com/MdNazmul9/All-Django-Project_Need/blob/main/backend/doc/1.2.png)
![Docker Basic 2.1 ](https://github.com/MdNazmul9/All-Django-Project_Need/blob/main/backend/doc/2.1.png)
![Docker Basic 2.2 ](https://github.com/MdNazmul9/All-Django-Project_Need/blob/main/backend/doc/2.2.png)
![Docker Basic 6.1 ](https://github.com/MdNazmul9/All-Django-Project_Need/blob/main/backend/doc/6.1.png)
![Docker Basic 6.2 ](https://github.com/MdNazmul9/All-Django-Project_Need/blob/main/backend/doc/6.2.png)
![Docker Basic 6.3 ](https://github.com/MdNazmul9/All-Django-Project_Need/blob/main/backend/doc/6.3.png)
![Docker Basic 8.1 ](https://github.com/MdNazmul9/All-Django-Project_Need/blob/main/backend/doc/8.1.png)


## Part-1
### Run Project with docker

  #### CMD 
     
  ```
  docker build --tag python-django .
  ```
  ```
  docker run --publish 8000:8000 python-django

  ```
  #### .dockerignore.
  ```
      */venv
  ```
  #### Dockerfile
  ```
  FROM python:3.8-slim-buster

  WORKDIR /app

  COPY requirements.txt requirements.txt
  RUN pip3 install -r requirements.txt
  COPY . .
  CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
  ```

## Part-2
### Run Project with docker compose :

  #### CMD 

  ```
  docker-compose build
  ```
  ```
  docker-compose run --rm app django-admin startproject core .
  ```
  ```
  docker-compose up
  ```
  #### Dockerfile
  ```
  FROM python:3.8-slim-buster
  ENV PYTHONUNBUFFERED=1

  WORKDIR /django
  COPY requirements.txt requirements.txt
  RUN pip3 install -r requirements.txt
  ```

  #### docker-compose.yml
  ```
  services:
      app:
            build: .
                # path where docker file exists
            volumes:
                - .:/django
                # copy project_dir_to: Dockerfile Working Dir
            ports:
                - 8000:8000
            image: app:django
            container_name: django_container1
            command: python manage.py runserver 0.0.0.0:8001

  ```
  ###### url: localhost:8000 or 0.0.0.0:8000
 
  #### Stop all running containers: 
  ```
  docker stop $(docker ps -a -q)
  ```
  #### Delete all stopped containers: 
  ```
  docker rm $(docker ps -a -q)
  ```
  #### Delete all Docker images
  ```
  docker rmi $(docker images -q)
  ```


## Part-3
### Run Project with docker-compose  and intrgrate database

  #### CMD 

  - 1. ``` docker-compose build ```
  - 2. ``` docker-compose run -rm app django-admin startproject core . ```
  - 3. ``` docker-compose up ```
  - 4. ``` docker exec -it django_app/bin/bash ```

# django-docker-postgres

  #### Dockerfile
  ```
  FROM python:3.8
  ENV PYTHONUNBUFFERED=1

  WORKDIR /django
  COPY requirements.txt requirements.txt
  RUN pip3 install -r requirements.txt
  ```

  #### docker-compose

  ```
  version: '3.8'

  services:
    app:
      build: .
      # path where docker file exists
      volumes:
        - .:/django
        # copy project_dir_to: Dockerfile Working Dir
      ports:
        - 8000:8000
      image: app:django
      container_name: django_container
      command: python manage.py runserver 0.0.0.0:8000
      depends_on:
        - db
    db:
      image: postgres
      volumes: 
        - ./data/db:/var/lib/postgresql/data
      environment:
        - POSTGRES_DB=postgres
        - POSTGRES_USER=postgres
        - POSTGRES_PASSWORD=postgres
      container_name: postgres_db
    
  ```
# Django-Docker-MySQL

  ### Dockerfile
  ```
  FROM python:3.8
  ENV PYTHONUNBUFFERED=1

  WORKDIR /django
  COPY requirements.txt requirements.txt
  RUN pip3 install -r requirements.txt
  ```
  ### docker-compose.yml
  
  ```
  version: '3.8'

  services:
    app:
      build: .
      # path where docker file exists
      volumes:
        - .:/django
        # copy project_dir_to: Dockerfile Working Dir
      ports:
        - 8000:8000
      image: app:django
      container_name: django_container
      command: python manage.py runserver 0.0.0.0:8000
      depends_on:
        - db
    db:
      # image: postgres
      # volumes: 
      #   - ./data/db:/var/lib/postgresql/data
      # environment:
      #   - POSTGRES_DB=postgres
      #   - POSTGRES_USER=postgres
      #   - POSTGRES_PASSWORD=postgres
      # container_name: postgres_db

      # mysql
      image: mysql:5.7
      environment:
        MYSQL_DATABASE: 'django-app-db'
        MYSQL_ALLOW_EMPTY_PASSWORD: 'true'
      volumes: 
        - ./data/mysql/dbb:/var/lib/mysql

  ```
