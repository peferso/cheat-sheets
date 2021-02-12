# Jenkins cheat-sheet

This is just a set of tools that I find usefull to gather here for future reference.

## Jenkins installation in an ec2 instance using Amazon Linux 2

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

### Start Jenkins 

For this, you need to be sure of what port is using the Jenkins service:
```sh
sudo cat /etc/sysconfig/jenkins | grep JENKINS_PORT
```
Normally it should be port 8080. If another service is using the same port as Jenkins, we can modify the value of ```JENKINS_PORT``` 
to a port which is not used.



### Location of the initial Jenkins user password

To retrieve the initial admin password necessary in the Jenkins installation, in the case of a unix system:
```sh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

