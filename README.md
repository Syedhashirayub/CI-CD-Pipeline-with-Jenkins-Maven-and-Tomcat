# 🌟 Automated CI/CD Pipeline with Jenkins, Maven, and Tomcat on AWS
![DALL·E 2024-10-12 20 02 56 - A dark-themed, modern illustration showing a continuous integration and continuous deployment (CI_CD) pipeline  The image should depict a flow with ar](https://github.com/user-attachments/assets/ac0e1cab-48b2-4ceb-9486-dd027023014a)

## 📋 Project Overview

This project demonstrates the setup of an automated CI/CD pipeline using **Jenkins**, **Maven**, and **Tomcat**, hosted on **AWS EC2** instances. It aims to automate the process of building, testing, and deploying a Java web application using these DevOps tools.

The pipeline includes:
- Continuous Integration: Automatically build and test the application whenever a code change is pushed to the Git repository.
- Continuous Deployment: Automatically deploy the built application to a Tomcat server.

---

## 🚀 Table of Contents
- [Prerequisites](#prerequisites)
- [Jenkins Installation and Setup](#jenkins-installation-and-setup)
- [Maven Installation and Setup](#maven-installation-and-setup)
- [Tomcat Server Setup](#tomcat-server-setup)
- [Jenkins CI/CD Pipeline Configuration](#jenkins-cicd-pipeline-configuration)
- [Automating the Pipeline](#automating-the-pipeline-with-poll-scm)
- [Conclusion](#conclusion)

---

## 🛠️  Prerequisites⚙️

Before setting up the pipeline, ensure you have the following:
- AWS account with permission to create EC2 instances
- An EC2 instance with Amazon Linux 2
- GitHub account for storing your project's source code

---

## ⚙️ Jenkins Installation and Setup

1. Connect to your EC2 instance via SSH and switch to the root user:
    ```bash
    sudo su
    ```

2. Update the system packages:
    ```bash
    sudo yum update -y
    ```

3. Install Jenkins:
    ```bash
    sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
    sudo yum install jenkins -y
    ```

4. Install Java 11 and start Jenkins:
    ```bash
    sudo amazon-linux-extras install java-openjdk11 -y
    sudo systemctl enable jenkins
    sudo systemctl start jenkins
    ```

5. Configure Jenkins by opening your EC2 public IP with port `8080` in a browser:
    ```
    http://<EC2-Public-IP>:8080
    ```

---

## ⚙️ Maven Installation and Setup

1. Download and install Maven on the Jenkins server:
    ```bash
    sudo su
    cd /opt
    wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
    tar xzvf apache-maven-3.9.9-bin.tar.gz
    mv apache-maven-3.9.9 maven
    ```

2. Add Maven and Java to the system's PATH by editing the `.bash_profile`:
    ```bash
    vim ~/.bash_profile
    ```
    Add the following lines:
    ```bash
    M2_HOME=/opt/maven
    M2=/opt/maven/bin
    JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64
    PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2
    ```

3. Apply the changes:
    ```bash
    source ~/.bash_profile
    ```

4. Verify Maven installation:
    ```bash
    mvn -v
    ```

---

## ⚙️ Tomcat Server Setup

1. Launch a new EC2 instance for Tomcat and allow inbound traffic on port `8080`.

2. Install Tomcat:
    ```bash
    sudo su
    cd /opt
    wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.76/bin/apache-tomcat-9.0.76.tar.gz
    tar -xvzf apache-tomcat-9.0.76.tar.gz
    mv apache-tomcat-9.0.76 tomcat
    cd tomcat/bin
    ./startup.sh
    ```

3. Configure Tomcat manager by editing the `context.xml` files:
    ```bash
    vim /opt/tomcat/webapps/manager/META-INF/context.xml
    vim /opt/tomcat/webapps/host-manager/META-INF/context.xml
    ```
    Comment out the following lines:
    ```xml
    <!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve" ... /> -->
    ```

4. Restart Tomcat:
    ```bash
    ./shutdown.sh
    ./startup.sh
    ```

5. Add admin users to `tomcat-users.xml`:
    ```xml
    <role rolename="manager-gui"/>
    <role rolename="manager-script"/>
    <user username="admin" password="admin" roles="manager-gui, manager-script"/>
    ```

---

## 📜 Jenkins CI/CD Pipeline Configuration

1. Install Maven Integration and GitHub plugins in Jenkins.

2. Configure the Maven plugin in Jenkins:
    - Go to **Manage Jenkins > Global Tool Configuration**
    - Add JDK and Maven with paths `/usr/lib/jvm/java-11-amazon-corretto` and `/opt/maven` respectively.

3. Create a new Jenkins job:
    - Go to **New Item** > Enter a name (e.g., `BuildAndDeployToTomcat`) > Select `Freestyle project`.
    - In the **Source Code Management** section, add your GitHub repository link.
    - In the **Build** section, select the `pom.xml` file and run `clean install`.
    - Add a **Post-build Action** > Select `Deploy war/ear to a container` > Choose `Tomcat 8.x Remote` and enter your Tomcat server details.

---

## Automating the Pipeline with Poll SCM

1. Enable **Poll SCM** to automate the build and deploy process:
    - Go to **Build Triggers** in your Jenkins job.
    - Check the **Poll SCM** option and set the cron job to check for changes every minute:
      ```
      * * * * *
      ```

2. Every time there is a commit in the GitHub repository, Jenkins will automatically trigger a build and deploy the latest version of the app to Tomcat.

---

## Conclusion

This project demonstrates the full CI/CD pipeline for a Java web application using Jenkins, Maven, and Tomcat on AWS. It automates the build, test, and deployment processes to ensure smooth continuous delivery with minimal manual intervention.

Feel free to clone this repository and try it yourself!

---

## License
This project is licensed under the MIT License.
