## Install Build Tools (Maven, Node) in Jenkins
- Configure Plugin for Maven:
  - Manage Jenkins > Tools > Add Maven
  - name: maven-3.9, version: 3.9.2, save
- Configure Tool Node:
  - We are going to install npm and Node inside the Jenkins container on the droplets
  - ``docker exec -u 0 -it containerId bash`` -> ``-u 0`` means that we are loggin in as a root user
  - ``cat /etc/issue`` -> to check which Linux distribution is running on the server (depending on the distribution, we will pick the correct node tool)
  - ``apt update``
  - ``apt install curl``
  - ``curl -sL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh`` -> to download the script needed to install npm and node
  - ``bash nodesource_setup.sh`` -> execute the script to download npm and node
  - ``apt install nodejs`` -> execute this to install npm and node

## Make Docker available on Jenkins server
-

## Create Jenkins credentials for a git repository
-

## Create different Jenkins job types (Freestyle, Pipeline, Multibranch pipeline) for the Java Maven project with Jenkinsfile to:

### a. Connect to the application’s git repository
-

### b. Build jar
-

### c. Build Docker image
-

### d. Push to private DockerHub repository
-
