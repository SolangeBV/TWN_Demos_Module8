## Create a Ubuntu server on DigitalOcean
- DigitalOcean > Create > Droplets
- OS: Ubuntu, Droplet Type: Basic, CPU Options: Regular + 4 GB and 2 CPUs
- Attach firewall -> create new one (SSH -> Change the sources to my own public IP address, Custom TCP inbound Port -> 8080)

## Set up and run Jenkins as Docker container
- Install docker: ``apt update`` + ``apt install docker.io``
- Install Jenkins container: ``docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts``

## Initialize Jenkins
- On a browser, go to yourDropletIpAddress:8080 to access Jenkins
- Type in the admin password (located at /var/jenkins_home/initialPassword)
  - ``docker exec -it containerId bash``
  - ``cat /var/jenkins_home/initialPassword `` -> on the terminal you will see the password
- Install suggested plugins
- Create first admin user
- Save and finish
