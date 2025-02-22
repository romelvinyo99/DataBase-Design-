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


# Start mysql service 
CMD ["mysqld_safe"]
