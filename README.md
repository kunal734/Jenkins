# Jenkins
Learn about installing Jenkins, configure Docker as agent, set up ci/cd, deploy applications to k8s and much more.

## AWS EC2 Instance
- Go to AWS Console
- Instances(running)
- Launch instances


### Install Jenkins

#### Pre-Requisites:

- Java (JDK)

#### Run the below commands to install Java and Jenkins

Install Java

```bash
  sudo apt update
  sudo apt install openjdk-17-jre
```

Verify Java is Installed

```bash
  java -version
```
Now, you can proceed with installing Jenkins

```bash
  curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key |    sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
  echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update
  sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed All traffic).

#### Login to Jenkins using the below URL:
http://:8080 [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing All Traffic to your EC2 instance 
- Delete the inbound traffic rule for your instance
- Edit the inbound traffic rule to only allow custom TCP port 8080

After you login to Jenkins:
- Run the command to copy the Jenkins Admin Password -  

```bash
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
- Enter the Administrator password

Click on Install suggested plugins

Wait for the Jenkins to Install suggested plugins

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

Jenkins Installation is Successful. You can now starting using the Jenkins

## Install the Docker Pipeline plugin in Jenkins:
- Log in to Jenkins.
- Go to Manage Jenkins > Manage Plugins.
- In the Available tab, search for "Docker Pipeline".
- Select the plugin and click the Install button.
- Restart Jenkins after the plugin is installed.

Wait for the Jenkins to be restarted.

## Docker Slave Configuration

#### Run the below command to Install Docker
```bash
  sudo apt update
  sudo apt install docker.io
```

#### Grant Jenkins user and Ubuntu user permission to docker deamon.
```bash
  sudo su - 
  usermod -aG docker jenkins
  usermod -aG docker ubuntu
  systemctl restart docker
```

#### Once you are done with the above steps, it is better to restart Jenkins.
```bash
  http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.

