Week 6 Assignment Report

Part 1 – Deploy Containers on Amazon EC2
Commands Used

Create Docker Network

sudo docker network create tasktracker-network

Run MySQL Container

sudo docker run -d \
--name mysql-dCb \
--network tasktracker-network \
-e MYSQL_ROOT_PASSWORD=root123 \
-e MYSQL_DATABASE=taskdb \
-p 3306:3306 \
mysql:8

Run PHP Task Tracker Container

sudo docker run -d \
--name task-tracker \
--network tasktracker-network \
-p 80:80 \
-e DB_HOST=mysql-db \
-e DB_USER=root \
-e DB_PASSWORD=root123 \
-e DB_NAME=taskdb \
omarionya/task-tracker:latest

The Communication between the Two Containers

This particular application uses two Docker containers on the same Amazon EC2 machine. While one container runs the MySQL database, another runs the PHP Task Tracker application. These containers are communicating by means of the tasktracker-network Docker bridge network. Thus, the connection is secured by the usage of the names of these containers.

The PHP application connects to the database through the environment variables. In order to connect, it uses the hostname that is equal to the name of the MySQL container, mysql-db. As Docker provides its own internal DNS, the container is resolved to this name, thus avoiding the necessity of connecting to the database by its IP address. The database authentication is performed with the help of the environment variables such as DB_USER, DB_PASSWORD and DB_NAME.

In case a user posts a task, the PHP application performs SQL INSERT request to add the new task to the database. Also, when the application page is loaded, the PHP application makes an SQL SELECT request to get all saved tasks.