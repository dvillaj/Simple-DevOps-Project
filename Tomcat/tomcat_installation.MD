# Tomcat installation on EC2 instance

### Pre-requisites
1. EC2 instance with Java v1.8.x 

### Install Apache Tomcat
1. Download tomcat packages from  https://tomcat.apache.org/download-80.cgi onto /opt on EC2 instance
```sh 
# Create tomcat directory
cd /opt
sudo wget https://apache.brunneis.com/tomcat/tomcat-8/v8.5.51/bin/apache-tomcat-8.5.51.tar.gz
sudo tar -xvzf /opt/apache-tomcat-8.5.51.tar.gz
sudo rm apache-tomcat-8.5.51.tar.gz
sudo ln -s apache-tomcat-8.5.51 tomcat
```
1. give permissions to executing tomcat
```sh
sudo chown -R ec2-user /opt/apache-tomcat-8.5.51/
   ```

1. create link files for tomcat startup.sh and shutdown.sh 
```sh
sudo ln -s /opt/tomcat/bin/startup.sh /usr/local/bin/tomcatup
sudo ln -s /opt/tomcat/bin/shutdown.sh /usr/local/bin/tomcatdown

```

  Using unique ports for each application is a best practice in an environment. But tomcat and Jenkins runs on ports number 8080. Hence lets change tomcat port number to 8090. Change port number in conf/server.xml file under tomcat home

   ```sh
vi /opt/apache-tomcat-8.5.51/conf/server.xml
# update port number in the "Connector port=" field in server.xml
# start tomcat after configuration update

tomcatup
```
#### Check point :
Access tomcat application from browser on port 8090  
 - http://<Public_IP>:8090

1. now application is accessible on port 8090. but tomcat application doesnt allow to login from browser. changing a default parameter in context.xml does address this issue
   ```sh
   #search for context.xml
   find /opt -name context.xml
   ```
1. above command gives 3 context.xml files. comment (<!-- & -->) `<Valve className` field on files which are under webapp directory (2 files).
After that restart tomcat services to effect these changes
   ```sh 
   tomcatdown
   tomcatup
   ```
1. Update users information in the tomcat-users.xml file `find /opt -name tomcat-users.xml`
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
1. Restart serivce and try to login to tomcat application from the browser. This time it should be Successful

