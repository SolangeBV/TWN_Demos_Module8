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
- We need to mount docker runtime onto the Jenkins Container on the remote server
- Stop the container running on remote server: ``docker stop containerId``
- Restart the container with extra mounted volumes ``docker run -p 8080:8080 -p 50000:50000 -d``
  ``-v jenkins_home:/var/jenkins_home`` -> use jenkins_home volumes on the container
  ``-v /var/run/docker.sock:/var/run/docker.sock`` -> mount volumes on the host machine
  ``jenkins/jenkins:lts``
- On the remote server, run ``docker exec -u 0 -it containerId bash``
- fetch the latest version of docker:
  ``curl https://get.docker.com/ > dockerinstall && chmod 777 dockerinstall && ./dockerinstall``
- Set read and write permissions for the jenkins user on the docker.sock file (this is a Unix socket file used by the Docker daemon to communicate with the Docker client):
  ``chmod 666 /var/run/docker.sock``
- log in now as jenkins user: ``exit`` + ``docker exec -it containerId bash``
- ``docker pull redis`` -> it will pull the image

## Create Jenkins credentials for a git repository
- Jenkins Dashboard > my-job > Configuration > Source Code Management > Git
- Paste http URL into "Repository URL" (take starting code from here: https://gitlab.com/twn-devops-bootcamp/latest/08-jenkins/java-maven-app/-/tree/starting-code)
- Credentials > add
  - Add username and password from GitLab (those I use to login to GitLab)
- Save

## Create different Jenkins job types (Freestyle, Pipeline, Multibranch pipeline) for the Java Maven project with Jenkinsfile to:

### a. Connect to the application’s git repository
- eg: java-maven-app, under "jenkins-job" branch there is a script (freestyle-build.sh) that we can run with a Jenkins job
- Connect to the repo following the steps in "Create Jenkins credentials for a git repository" and pick the branch "jenkins-job"
- In the "execute shell" form, write the following:
  ``chmod +x freestyle-build.sh``
  ``./freestyle-build.sh``
- Save
- Build

### b. Build jar
- Jenkins Dashboard > New Item > Freestyle Project
- Copy git url (java-maven-app)
- Add credentials (already existing)
- Add branch (jenkins-job)
- Build Steps -> Invoke top-level Maven targets
- Goals: test (to run the tests in AppTests.java)
- Build Steps -> Invoke top-level Maven targets
- Goals: package (to build a jar file)

### c. Build Docker image
- Create Dockerfile in java-maven-app
- Take the same job from point b ("Build jar") > Configure > Build Steps
- Add build step > Execute shell
- ``docker build -t java-maven-app:1.0 . ``
- Save
- Build Now

### d. Push to private DockerHub repository
- Create credentials for Docker Repository:
  - Create Docker Hub Account (hub.docker.com) > Create repository > Visibility private > create
  - Jenkins > Manage Jenkins > Credentials > System > Global credentials > Add credentials (dockerhub username and password)
- Take the same job from point c ("Build Docker image")
- Under "Build Environment" check "Use secret text(s) or file(s) -> Username and password (separated)
  - Username Variable -> USERNAME
  - Password Variable -> PASSWORD
  - Credentials -> docker-hub-repo credentials
- Modify the following line in the Command:
  ``docker build -t myUsernameOnDockerHub-demo-app:jma-1.0 .``
  ``docker login -u $USERNAME -p $PASSWORD``
  ``docker push myUsernameOnDockerHub-demo-app:jma-1.0``
- Save
- Build Now
