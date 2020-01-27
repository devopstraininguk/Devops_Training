# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.

### Prerequisites
1. EC2 instance
   - With Internet Access
   - Security Group with Port `8080` open for internet
1. Java v1.8.x 

## Install Java
We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/
```sh
yum install java-1.8*
#yum -y install java-1.8.0-openjdk
```

### Confirm Java Version
Lets install java and set the java home
```sh
java -version
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.161-0.b14.e17_4.x86_64
export PATH=$JAVA_HOME/bin:$PATH
```
_The output should be something like this,_
```
[root@~]# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```

## Install Tomcat server
```sh
yum -y install wget
wget http://www.mirrorservice.org/sites/ftp.apache.org/tomcat/tomcat-8/v8.5.50/bin/apache-tomcat-8.5.50.zip
unzip apache-tomcat-8.5.50.zip
cd apache-tomcat-8.5.50.zip/bin
chmod +x *.sh
./startup.sh
```

## Install Jenkins
You can install jenkins using the rpm or by setting up the repo. We will setup the repo so that we can update it easily in future.
Get latest version of jenkins from https://pkg.jenkins.io/redhat-stable/
```sh
cd apache-tomcat-8.5.50/webapps
wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
```

#### Accessing Jenkins
By default jenkins runs at port `8080`, You can access jenkins at
```sh
http://YOUR-SERVER-PUBLIC-IP:8080/jenkins
```
#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
  - Password Location:`/root/.jenkins/secrets/initialAdminPassword`
- Plugin Installation: Install suggested plugins
- Change admin password
  - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under Build section
	Execute shell : echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"

