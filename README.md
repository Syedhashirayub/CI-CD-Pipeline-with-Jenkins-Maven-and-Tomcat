# 🌟 CI/CD Pipeline with Jenkins, Maven, and Tomcat
![project_workflow_diagram](https://github.com/user-attachments/assets/72c63584-dce7-4c8d-b94d-b3c2f5986ecc)


This project demonstrates a Live AWS Resource Manager using shell scripting for DevOps. The script automates the tracking of AWS resources like EC2 instances, S3 buckets, RDS databases, and IAM users.

## 📚 Table of Contents
- [Project Overview](#project-overview)
- [Getting Started](#getting-started)
- [Script Explanation](#script-explanation)
- [Setting Up a Cron Job](#setting-up-a-cron-job)
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

3. Configuring Jenkins for Maven
   
 - Access Jenkins through the web interface.
 - Navigate to Manage Jenkins → Global Tool Configuration.
 - Under Maven, add a new Maven installation and set the path to /opt/maven.

4. Tomcat Setup on AWS EC2
   
 1. Launch a new EC2 instance for Tomcat.
 2. Install and configure Tomcat:


## ⏰ Setting Up a Cron Job

To schedule the script to run at regular intervals, edit the crontab:

1. **Edit the crontab**:
   ```bash
   crontab -e
2. **Add the following line to schedule the script**:
   ```bash
   * * * * * /home/ec2-user/aws-resource-tracker.sh >> /home/ec2-user/log/aws-resource-output.txt 2>&1

### 📝 Cron Job Breakdown:
 - '* * * * *' : Runs every minute.
 - '/home/ec2-user/aws-resource-tracker.sh' : Path to the script.
 - '>> /home/ec2-user/log/aws-resource-output.txt' : Appends output to a log file.
 - '2>&1' : Redirects error messages to the same log file.

## 🛡️ IAM Role Configuration

1. **Create an IAM Role**: Ensure that the EC2 instance has the necessary permissions to access AWS services.
Policy names to attach:
 - AmazonEC2ReadOnlyAccess
 - AmazonRDSReadOnlyAccess
 - AmazonS3ReadOnlyAccess
 - IAMReadOnlyAccess
2. **Attach the IAM Role to the EC2 Instance**:
 - Go to the EC2 Management Console.
 - Select the instance running the script.
 - Click Actions → Security → Modify IAM role.
 - Select the IAM role you created (e.g., EC2-AWS-Resource-Tracker-Role) and click Update IAM role.

## 📄 License

This project is licensed under the MIT License. See the LICENSE file for details.

