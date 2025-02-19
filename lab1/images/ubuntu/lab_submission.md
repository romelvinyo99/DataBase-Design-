# LAB 1: MARKDOWN FILE

## 1. Create the File Structure
```
downloads/lab1/images/ubuntu/Dockerfile
```

## 2. Creating the Dockerfile
```dockerfile
# Use the official ubuntu image
FROM ubuntu:latest

# Set an interactive environment for apt-get to prevent interaction issues
ENV DEBIAN_FRONTEND=noninteractive

# Install and update required dependencies
RUN apt-get update && apt-get install -y \
    mysql-server \
    mysql-client \
    && rm -rf /var/lib/apt/lists/*

# Expose MySQL port
EXPOSE 3306

# Start MySQL service - default
CMD ["mysqld_safe"]
```

## 3. GitBash
### a. Prior Checkups
```sh
docker images  # List images
docker ps      # Running containers
docker ps -a   # Recently closed containers
```

### b. Building Image - Specified by Dockerfile
```sh
docker build -t customized-ubuntu:1.0 .
```

### c. Build a Container from the Image
```sh
docker run -d --name dbserver-mysql-nairobi -p 3309:3306 customized-ubuntu:1.0
```

### d. Checking for Running Containers
```sh
docker images
docker ps  # If the container has not started, start manually:
docker start <container-name>
```

### e. Run Container Interactively (Use winpty for Windows)
```sh
winpty docker exec -it dbserver-mysql-nairobi bash
```

### f. Check If MySQL Is Installed and Updated
```sh
mysql --version
```

### g. Check for Service Status and Start
```sh
service mysql status
service mysql start
```

### h. Login to MySQL Server as Root Without Password & Change Root Password
```sh
mysql -u root

SELECT User, Host FROM mysql.user;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '5trathmore';
FLUSH PRIVILEGES;
EXIT;
```

### i. Login to MySQL Server as Root with Password
```sh
mysql -u root -p
# Enter password: 5trathmore
EXIT;
```

### j. Commit the Changes to the Container
```sh
docker commit dbserver-mysql-nairobi dbserver-mysql-nairobi
docker stop dbserver-mysql-nairobi
docker ps  # Confirmation
```

### k. Restart the Container to Test
```sh
docker ps -a  # Check for the name of the recently closed container
docker start dbserver-mysql-nairobi
winpty docker exec -it dbserver-mysql-nairobi bash
```

### l. Login as Root with Password "5trathmore"
```sh
mysql -u root -p
# Enter password: 5trathmore
```

