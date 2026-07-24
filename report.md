Week 6 Assignment Report

Part 1 – Deploy Containers on Amazon EC2
Commands Used

Create Docker Network

sudo docker network create tasktracker-network

Run MySQL Container

sudo docker run -d   
--name mysql-dCb   
--network tasktracker-network   
-e MYSQL\_ROOT\_PASSWORD=root123   
-e MYSQL\_DATABASE=taskdb   
-p 3306:3306   
mysql:8

Run PHP Task Tracker Container

sudo docker run -d   
--name task-tracker   
--network tasktracker-network   
-p 80:80   
-e DB\_HOST=mysql-db   
-e DB\_USER=root   
-e DB\_PASSWORD=root123   
-e DB\_NAME=taskdb   
omarionya/task-tracker:latest

The Communication between the Two Containers

This particular application uses two Docker containers on the same Amazon EC2 machine. While one container runs the MySQL database, another runs the PHP Task Tracker application. These containers are communicating by means of the tasktracker-network Docker bridge network. Thus, the connection is secured by the usage of the names of these containers.

The PHP application connects to the database through the environment variables. In order to connect, it uses the hostname that is equal to the name of the MySQL container, mysql-db. As Docker provides its own internal DNS, the container is resolved to this name, thus avoiding the necessity of connecting to the database by its IP address. The database authentication is performed with the help of the environment variables such as DB\_USER, DB\_PASSWORD and DB\_NAME.

In case a user posts a task, the PHP application performs SQL INSERT request to add the new task to the database. Also, when the application page is loaded, the PHP application makes an SQL SELECT request to get all saved tasks.



PART2



&#x20;Environment Variables Used



&#x20;Variable | Value |



&#x20;DB\_HOST - tasktracker-db.cwxmmkki6ep1.us-east-1.rds.amazonaws.com |

&#x20;DB\_USER  admin 

&#x20;DB\_PASSWORD - Task12345 

&#x20;DB\_NAME - taskdb 



&#x20;Comparison Between EC2 and ECS



Amazon EC2 is a virtual server service that gives users full control over the operating system, software installation, and application deployment. Users are responsible for managing and maintaining the server.



Amazon ECS (Elastic Container Service) is a container orchestration service that runs Docker containers. It automates container deployment, scaling, and management, allowing applications to run without managing the underlying infrastructure when using AWS Fargate.



In this assignment, EC2 was used in Part 1 to manually run Docker containers, while ECS with AWS Fargate was used in Part 2 to deploy the application in a managed environment connected to an Amazon RDS database.

