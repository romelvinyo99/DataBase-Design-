LAB 1 : MARKDOWN FILE

---------------------

1. create the file structure

downloads/lab1/images/ubuntu/Dockerfile

2. creating the dockerfile
---------------------

# Use the official ubuntu image
FROM ubuntu:latest

# Set a interactive environment for apt-get to prevent interaction issues
ENV DEBIAN_FRONTEND=noninteractive

# Install and updater required dependencies
RUN apt-get update && apt-get install -y \
     mysql-server \
     mysql-client \ 
     && rm -rf  /var/lib/apt/lists/*


# Expose mysql port
EXPOSE 3306


# Start mysql service - default
CMD ["mysqld_safe"]
-------------------------
3. GitBash

a. Prior checkups

    docker images - images
    docker ps     - running containers
    docker ps -a  - recently closed containers

b. Building image - specified by dockerfile

    docker build -t customized-ubuntu:1.0 .

c. Build a container from the image
    docker run -d --name dbserver-mysql-nairobi -p 3309:3306 customized-ubuntu:1.0 

d. Checking for running containers
    
    docker images
    docker ps - if container has not started start manually - "docker start <container name>"

e. Run container interactively - use winpty this was an issue earlier 
    
    winpty docker exec -it dbserver-mysql-nairobi bash

f. Check if mysql is installed and updated(specified in the docker file apt-get update && apt-get install)

   interactive terminal

   mysql --version
    
g. Check for servive status and start 
   
   service mysql status
   service mysql start
   

h. Login to server as root without password - change root password

   mysql -u root
   
   SELECT User, Host FROM mysql.user ; 
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '5trathmore';
   FLUSH PRIVILEDGES ; 
   EXIT

i. Login to server as root with password
   
   mysql -u root -p
   password: 5trathmore
   EXIT

j. Commit the changes to the container

   After exiting the mysql server interactive console
       
      docker commit dbserver-mysql-nairobi dbserver-mysql-nairobi
      docker stop dbserver-mysql-nairobi
      docker ps - confirmation

k. Restart the container to test
      docker ps -a - check for name of the recently closed
      docker start dbserver-mysql-nairobi
      winpty docker exec -it dbserver-mysql-nairobi bash
    
l. login as root with passoword "5trathmore"
   
--------------------------