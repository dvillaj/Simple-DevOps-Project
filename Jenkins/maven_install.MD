#  Install & configure Maven build tool on Jenkins
Maven is a code build tool which used to convert your code to an artifact. this is a widely used plugin to build in continuous integration


#### Prerequisites
1. Jenkins server

#### Install Maven on Jenkins
1. Download maven packages https://maven.apache.org/download.cgi onto Jenkins server. In this case, I am using /opt/maven as my installation directory
 - Link : https://maven.apache.org/download.cgi
```sh
# Creating maven directory under /opt
wget http://ftp.cixug.es/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
sudo mkdir /opt/maven
# downloading maven version 3.6.0
sudo mv apache-maven-3.6.3-bin.tar.gz /opt/maven

cd /opt/maven
sudo tar -xvzf apache-maven-3.6.3-bin.tar.gz
sudo rm apache-maven-3.6.3-bin.tar.gz
  ```
	
1. Setup M2_HOME and M2 paths in .bash_profile of the user and add these to the path variable

- Add to ~/.bash_profile
```sh
# Maven
M2_HOME=/opt/maven/apache-maven-3.6.3
M2=$M2_HOME/bin
export PATH=$PATH:$M2_HOME:$M2
```
#### Checkpoint 
1. logoff and login to check maven version
  
```sh
mvn --version
```
So far we have completed the installation of maven software to support maven plugin on the jenkins console. Let's jump onto Jenkins to complete the remaining steps. 

### Setup maven on Jenkins console
1. Install maven plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Integration`

2. Configure maven path
  - `Manage Jenkins` > `Global Tool Configuration` > `Maven`
    - name: M2_HOME
    - M2_HOME: /opt/maven/apache-maven-3.6.3


### Test Maven
1. Create “new item”
2. Enter an item name – `My-First-Maven-Build`
   - Chose `Maven Project` project
3. Under the Source Code Manager section
  - Git:
	  repo: https://github.com/dvillaj/hello-world-jenkins.git
4. Save your job 
5. Build job
6. Check "console output"
7. Check "workspace
8. Inside the server, check `/var/lib/jenkins/workspace` directory
