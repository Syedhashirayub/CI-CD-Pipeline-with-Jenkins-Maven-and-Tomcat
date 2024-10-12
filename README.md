# 🌟 CI/CD Pipeline with Jenkins, Maven, and Tomcat
![project_workflow_diagram](https://github.com/user-attachments/assets/72c63584-dce7-4c8d-b94d-b3c2f5986ecc)


This project demonstrates a Live AWS Resource Manager using shell scripting for DevOps. The script automates the tracking of AWS resources like EC2 instances, S3 buckets, RDS databases, and IAM users.

## 📚 Table of Contents
- [Project Overview](#project-overview)
- [Tools & Technologies Used](#tools-&-technologies-used)
- [Project Workflow](#project-workflow)
- [Installation & Setup](#installation-&-setup)
- [IAM Role Configuration](#iam-role-configuration)
- [License](#license)

## 🚀 Project Overview

This project demonstrates the setup of a Continuous Integration/Continuous Deployment (CI/CD) pipeline using Jenkins, Maven, and Tomcat on an AWS EC2 instance. It automates the build, testing, and deployment of a Java web application using Maven, with Jenkins managing the entire pipeline and deploying the application to a remote Tomcat server.

The entire workflow includes setting up Jenkins on an EC2 instance, configuring it with Maven and Git plugins, and deploying a WAR package to Tomcat after a successful build. The pipeline is configured to automatically trigger on code changes in a Git repository using Jenkins’ Poll SCM feature.

## 🛠️ Tools & Technologies Used

 - Jenkins: For automating the CI/CD process, triggering builds, tests, and deployments.
 - Maven: To manage dependencies and build the Java project.
 - Tomcat: The application server to deploy the Java web application.
 - AWS EC2 (Amazon Linux 2): The cloud server to host Jenkins and Tomcat.
 - Git: Version control system to manage source code.

## 🚀 Project Workflow

1. Jenkins Setup: Jenkins is installed on an EC2 instance and configured with Maven and Git plugins.
2. Maven Build: Maven compiles the Java code, runs unit tests, and packages the application into a WAR file.
3. Tomcat Deployment: Jenkins automatically deploys the WAR file to a remote Tomcat server after a successful build.
4. Continuous Integration: Jenkins is configured with Poll SCM, which triggers the pipeline whenever changes are pushed to the Git repository.

## ⚙️ Installation & Setup

1. **Jenkins Setup on AWS EC2**:
   ```bash
   # Update the system
   sudo yum update -y

   # Install Jenkins
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
   sudo yum install jenkins -y

   # Enable and start Jenkins service
   sudo systemctl enable jenkins
   sudo systemctl start jenkins
   
  Once Jenkins is installed, you can access it using the public IP of your EC2 instance on port 8080 (e.g., http://<EC2-Public-IP>:8080).
  
2. **Maven Installation on Jenkins Server**:
   ```bash
   # Install Java
   sudo amazon-linux-extras install java-openjdk11 -y

   # Install Maven
   cd /opt
   sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
   sudo tar -xvzf apache-maven-3.9.9-bin.tar.gz
   sudo mv apache-maven-3.9.9 maven

   # Configure Maven environment variables
   echo "export M2_HOME=/opt/maven" >> ~/.bash_profile
   echo "export M2=/opt/maven/bin" >> ~/.bash_profile
   echo "export PATH=$PATH:$M2_HOME:$M2" >> ~/.bash_profile
   source ~/.bash_profile

3. **Configuring Jenkins for Maven**:
   
 - Access Jenkins through the web interface.
 - Navigate to Manage Jenkins → Global Tool Configuration.
 - Under Maven, add a new Maven installation and set the path to /opt/maven.

4. **Tomcat Setup on AWS EC2**:
   
 1. Launch a new EC2 instance for Tomcat.
 2. Install and configure Tomcat:
    ```bash
    # Install Java
    sudo amazon-linux-extras install java-openjdk11 -y

    # Download and extract Tomcat
    cd /opt
    sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.76/bin/apache-tomcat-9.0.76.tar.gz
    sudo tar -xvzf apache-tomcat-9.0.76.tar.gz
    sudo mv apache-tomcat-9.0.76 tomcat

    # Start Tomcat
    cd /opt/tomcat/bin
    sudo ./startup.sh
  3. Access Tomcat using the public IP of your EC2 instance on port 8080 (e.g., http://<EC2-Public-IP>:8080).

  4. Configure tomcat-users.xml to enable access to the Manager App:
     ```bash
     <role rolename="manager-gui"/>
     <role rolename="manager-script"/>
     <role rolename="manager-jmx"/>
     <role rolename="manager-status"/>
     <user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status"/>

 5. **CI/CD Pipeline Configuration in Jenkins**:
  1. Install Plugins:

   - Go to Manage Jenkins → Manage Plugins.
   - Search for and install the following:
     - Maven Integration Plugin.
     - Deploy to Container Plugin.
       
  2. Create Jenkins Pipeline:

   - Create a new Freestyle Project.
   - Set up Git by adding your repository link and choosing the appropriate branch.
   - Under Build, configure Maven to build the project using the pom.xml file (clean install).

  3. Post-Build Deployment:

   - Add a Post-build Action to deploy the WAR file to the Tomcat server:
     - WAR file location: **/*.war
     - Container: Tomcat 8.x Remote.
     - Enter the credentials configured in tomcat-users.xml and the Tomcat server’s public IP with port 8080.

 6. Automating with Poll SCM
    
 - To automate the pipeline to run on each code change, configure Poll SCM:
  - Go to Build Triggers → Check Poll SCM.
  - Set the cron expression to * * * * * (runs every minute).

## 📜 Key Configuration Files

pom.xml
This is the Maven configuration file that defines the build steps and dependencies for the Java project.
    ```bash
    <project ...>
        <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>demo-app</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <dependencies>
        <!-- Define dependencies here -->
    </dependencies>
    </project>

tomcat-users.xml
This file defines Tomcat's roles and users.
    ```bash
    <role rolename="manager-gui"/>
     <role rolename="manager-script"/>
     <user username="admin" password="admin" roles="manager-gui,manager-script"/>
     
## 📝 Future Enhancements

 - SSL Implementation: To secure communication between Jenkins and Tomcat.
 - Automated Testing: Incorporate unit testing to ensure code quality before deployment.
 - Docker Integration: Use Docker to containerize Jenkins, Maven, and Tomcat for easier deployment and scaling.

## 🔗 Useful Links
 - Jenkins Documentation
 - Maven Documentation
 - Tomcat Documentation

## 📄 License

This project is licensed under the MIT License. See the LICENSE file for details.

