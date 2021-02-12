# Jenkins cheat-sheet

This is just a set of tools that I find usefull to gather here for future reference.

## Jenkins installation in an EC2 instance using Amazon Linux 2

### Getting the repo
```sh
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
sudo yum install jenkins -y
```
### Starting the jenkins service
```sh
sudo service jenkins start
sudo chkconfig jenkins on
```
Also, in case you want to test, stop or restart the jenkins service then use:
```sh
sudo service jenkins restart
sudo service jenkins stop
sudo service jenkins status
```

### Starting Jenkins

To start using Jenkins, you need to be sure of which port is using the jenkins service:
```sh
sudo cat /etc/sysconfig/jenkins | grep JENKINS_PORT
```
Normally it should be port 8080. If another service is using the same port as Jenkins, we can modify the value of ```JENKINS_PORT``` 
to a port which is not used.

In case you want to test which ports are opened, and which services are listening to them:
```sh
sudo netstat -tulpn | grep LISTEN
```

### Using Jenkins installed in an EC2 instance

IMPORTANT: It is necessary to add a *rule* to the *security group* of the EC2 instance hosting Jenkins. The rule should allow custom tcp (_TCP personalizada_ in Spanish) traffic type, 
allowing traffic from the IP range needed in your case (origin) through the ```JENKINS_PORT``` value. 

### Location of the initial Jenkins user password

To retrieve the initial admin password necessary in the Jenkins installation, in the case of a unix system:
```sh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

