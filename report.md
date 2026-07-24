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



PART 3



&#x20;Question 1: Compare EC2 and ECS Fargate





Amazon EC2 provides virtual servers that users must configure, manage, and maintain. Users are responsible for installing software, applying updates, and securing the operating system. In contrast, Amazon ECS Fargate is a serverless container service where AWS manages the underlying infrastructure, allowing users to focus on running their applications.



EC2 requires manual or Auto Scaling configuration, while ECS Fargate scales containers automatically. EC2 also incurs charges for running instances even when idle, whereas Fargate charges only for the resources used by running containers. Overall, ECS Fargate reduces maintenance effort and simplifies deployment compared to EC2.



&#x20;Question 2



Amazon RDS is preferred because it is a fully managed database service. AWS automatically performs backups, software updates, monitoring, and failure recovery. It also provides high availability, better security, and easier scaling. Running MySQL inside a Docker container requires the user to manage backups, updates, security, and database maintenance manually.



Question 3



1\. No need to manage or maintain servers.

2\. Automatically scales containerized applications.

3\. Integrates easily with AWS services such as Amazon RDS, CloudWatch, and IAM.



&#x20;Question 4



Amazon EC2 is a virtual server service used to host applications and run Docker containers. Amazon ECS is a container orchestration service that deploys and manages Docker containers. AWS Fargate is a serverless compute engine for ECS that allows containers to run without managing servers. Amazon RDS is a managed database service used to host the MySQL database. Security Groups act as virtual firewalls that control inbound and outbound network traffic. Docker is a platform for packaging applications and their dependencies into containers, while Docker Network enables communication between Docker containers, allowing the PHP application to connect to the MySQL database during the EC2 deployment.



&#x20;Question 5



In the EC2 deployment, the PHP application and the MySQL database were running as separate Docker containers on the same EC2 instance. They communicated through a Docker network using the MySQL container name as the database host.



In the ECS deployment, the PHP application runs inside an ECS Fargate container while the MySQL database is hosted on Amazon RDS. The application connects to the RDS database using the RDS endpoint, database username, password, and database name provided through ECS environment variables.











