# Two-Tier Flask Application




## Introduction
This project sets up a two-tier application using Docker. The application consists of a Flask web server and a MySQL database.

## Prerequisites
Ensure you have Docker and Docker Compose installed on your system.

## Install Docker 
```markdown
sudo apt install docker.io

sudo apt install docker-compose



## Installation

1. **Clone the repository:**
    ```bash
    git clone https://github.com/LondheShubham153/two-tier-flask-app.git
    cd two-tier-flask-app
    ```

2. **Build the Docker image for the Flask application:**
    ```bash
    docker build . -t flaskapp
    ```

3. **Create a Docker network:**
    ```bash
    docker network create twotier
    ```

## Usage

1. **Run the MySQL container:**
    ```bash
    docker run -d \
        --name mysql \
        -v mysql-data:/var/lib/mysql \
        --network=twotier \
        -e MYSQL_DATABASE=mydb \
        -e MYSQL_ROOT_PASSWORD=admin \
        -p 3306:3306 \
        mysql:5.7
    ```

2. **Run the Flask application container:**
    ```bash
    docker run -d \
        --name flaskapp \
        --network=twotier \
        -e MYSQL_HOST=mysql \
        -e MYSQL_USER=root \
        -e MYSQL_PASSWORD=admin \
        -e MYSQL_DB=mydb \
        -p 5000:5000 \
        flaskapp:latest
    ```

3. **Verify the containers are running:**
    ```bash
    docker ps
    ```

## Docker Compose

You can also use Docker Compose to manage the containers. Create a `docker-compose.yml` file with the following content:

```yaml
version: '3'
services:
  mysql:
    image: mysql:5.7
    container_name: mysql
    environment:
      MYSQL_DATABASE: mydb
      MYSQL_ROOT_PASSWORD: admin
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - twotier
    ports:
      - "3306:3306"

  flaskapp:
    build: .
    container_name: flaskapp
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: admin
      MYSQL_DB: mydb
    networks:
      - twotier
    ports:
      - "5000:5000"

networks:
  twotier:
    driver: bridge

volumes:
  mysql-data:
```

To start the application using Docker Compose:
```bash
docker-compose up -d
```

## Additional Commands

- **List Docker images:**
    ```bash
    docker images
    ```

- **Tag and push Docker image:**
    ```bash
    docker tag flaskapp:latest newomo/flaskapp:latest
    docker push newomo/flaskapp:latest
    ```

- **Access a running container:**
    ```bash
    docker exec -it <container_id> bash
    ```

## Conclusion
This README provides a comprehensive guide to setting up and running a two-tier Flask application using Docker. Follow the steps to get your application up and running quickly.



