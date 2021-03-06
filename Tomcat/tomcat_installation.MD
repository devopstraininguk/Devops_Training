# Tomcat installation on EC2 instance


### Prerequisites
1. EC2 instance with Java v1.8.x
	yum install java-1.8*
	export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.161-0.b14.e17_4.x86_64
	export PATH=$JAVA_HOME/bin:$PATH
	edit /etc/profile file to set JAVA_HOME path permanently for all users

### Install Apache Tomcat
Download tomcat packages from  https://tomcat.apache.org/download-80.cgi onto /opt on EC2 instance
```sh 
  # create tomcat directory
  cd /opt
  wget http://mirrors.ukfast.co.uk/sites/ftp.apache.org/tomcat/tomcat-8/v8.5.50/bin/apache-tomcat-8.5.50.zip
  unzip /opt/apache-tomcat-8.5.50.zip
```
give executing permissions to startup.sh and shutdown.sh which are under bin. 
```sh
   chmod +x /opt/apache-tomcat-8.5.35/bin/*.sh
```

starting and stopping tomcat server 
```sh
  ./startup.sh - starts tomcat server
  ./shutdown.sh - stops tomcat server
```
#### check point :
access tomcat application from browser on prot 8080  
http://<Public_IP>:8080

Using unique ports for each application is a best practice in an environment. But tomcat and Jenkins runs on ports number 8080. Hence lets change tomcat port number to 8090. Change port number in conf/server.xml file under tomcat home
```sh
cd /opt/apache-tomcat-8.5.35/conf
# update port number in the "connecter port" field in server.xml
# restart tomcat after configuration update
# go the bin directory
./shutdown.sh
./startup.sh
```
#### check point :
access tomcat application from browser on prot 8090  
http://<Public_IP>:8090

now application is accessible on port 8090. but tomcat application doesnt allow to login from browser. changing a default parameter in context.xml does address this issue
```sh
#search for context.xml
find / -name context.xml
```
above command gives 3 context.xml files. comment (<!-- & -->) `Value ClassName` field on files which are under webapp directory. 
After that restart tomcat services to effect these changes
```sh 
./shutdown.sh
./startup.sh
```
Update users information in the tomcat-users.xml file
goto tomcat home directory and Add below users to conf/tomcat-user.xml file
```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>
```
Restart serivce and try to login to tomcat application from the browser. This time it should be Successful

