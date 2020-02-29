# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.


### Prerequisites
1. EC2 Instance 
   - Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type
   - Add Tag
      - name: Jenkins_Server
   - Security Group
      Security group name: DevOps_Project_SG
      Description: DevOps Project SG
      Add Type
         - Port Range: 8080
   - Create a new pair key
      - name: DevOps_Project_SG
      - Download Key Pair

## Install Java
1. We will be using open java for our demo, Get the latest version from http://openjdk.java.net/install/
   ```sh
   sudo yum remove java-1.7.0*
   sudo yum install java-1.8*
   #yum -y install java-1.8.0-openjdk-devel
   ```

1. Confirm Java Version and set the java home
   ```sh
   java -version
   find /usr/lib/jvm/java-1.8* | head -n 3
   JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-1.el8_0.x86_64
   export JAVA_HOME
   PATH=$PATH:$JAVA_HOME
    # To set it permanently update your .bash_profile
   vi ~/.bash_profile
   ```
   _The output should be something like this,_
    ```sh
   [root@~]# java -version
   openjdk version "1.8.0_151"
   OpenJDK Runtime Environment (build 1.8.0_151-b12)
   OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
   ```

## Install Jenkins
 You can install jenkins using the rpm or by setting up the repo. We will set up the repo so that we can update it easily in the future.
1. Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install
   ```sh
   sudo yum -y install wget
   wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
   sudo yum -y install jenkins
   ```

   ### Start Jenkins
   ```sh
   # Start jenkins service
   sudo service jenkins start

   # Setup Jenkins to start at boot,
   sudo chkconfig jenkins on
   ```

   ### Accessing Jenkins
   By default jenkins runs at port `8080`, You can access jenkins at
   ```sh
   http://ec2-3-135-238-168.us-east-2.compute.amazonaws.com:8080
   ```
  #### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
- Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
   - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Create another admin user id

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under the Build section
	Execute shell: echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"

