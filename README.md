# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.



### Prerequisites
1. EC2 Centos Instance 
   - With Internet Access
   - Security Group with Port `8080` open for internet

1. Java v1.8.x 

## Install Java
Update the OS

```sudo yum update -y```

We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/
```sh
sudo yum -y install java-1.8*
#sudo yum -y install java-1.8.0-openjdk
```

### Confirm Java Version
Lets install java and set the java home
```sh
java -version
find /usr/lib/jvm/java-1.8* | head -n 3
#Add JAVA_HOME and update of PATH to ~/.bash_profile
#example value set below
echo 'export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-1.el7_9.x86_64' >> ~/.bash_profile
echo 'PATH=$PATH:$JAVA_HOME' >> ~/.bash_profile
echo 'export PATH' >> ~/.bash_profile
chmod 755 ~/.bash_profile
```
### To set it permanently update your .bash_profile

```sh
~/.bash_profile
```
```
[root@~]# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```

## Install Jenkins
You can install jenkins using the rpm or by setting up the repo. We will setup the repo so that we can update it easily in future.
Get latest version of jenkins from https://pkg.jenkins.io/redhat-stable/
```sh
sudo su
yum install epel-release -y
yum -y install wget
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum -y install jenkins
```

### Start Jenkins
```sh
# Start jenkins service
systemctl start jenkins

# Setup Jenkins to start at boot,
systemctl enable jenkins
```

#### Accessing Jenkins
By default jenkins runs at port `8080`, You can access jenkins at
```sh
http://YOUR-SERVER-PUBLIC-IP:8080
```
#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
  - Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
  - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Dashboard` > `Manage Jenkins` > `Global Tool Configuration` > `JDK`  > Enter some name like Java1.8 > Enter JAVA_HOME from the server > Scroll to bottom > `Save`
- `Dashboard` > `Manage Jenkins` > `Manage Users` > `Create User` > Create another admin user id

### Test Jenkins Jobs
1. `Dashboard` > `New item`
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under Build section
	Execute shell : echo "Welcome to Jenkins Demo"
1. Save your job 
1. Click on Build Now
2. Click on latest build link in Build History section
3. Click on "Console Output"

### Next Step

