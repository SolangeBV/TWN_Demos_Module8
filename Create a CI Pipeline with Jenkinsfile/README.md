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
- 8.6

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
-

### d. Push to private DockerHub repository
-
