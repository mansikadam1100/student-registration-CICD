# 🎓 Student App - Complete DevOps CI/CD Project 

A **Java-based Student Management Web Application** deployed through a **fully automated CI/CD pipeline** using **Git, GitHub, Jenkins, AWS EC2 (Ubuntu), Maven, and Apache Tomcat**.

This project demonstrates how to automate the **build, test, and deployment** process for Java applications using Jenkins Pipelines and AWS infrastructure.

---

##  Project Summary

The **Student App** enables users to manage student data — including name, age, qualification, percentage, and passing year — through a simple web interface.

This project focuses on end-to-end automation with **Continuous Integration (CI)** and **Continuous Deployment (CD)**, eliminating manual intervention during deployment.

![](./img/student2.png)

---

##  Objective

To build a **fully automated DevOps pipeline** that:

1. Pulls the latest source code from GitHub when new commits are pushed.  
2. Builds the Java project using Maven to generate a `.war` file.  
3. Automatically deploys the `.war` file to an Apache Tomcat server on AWS EC2.  
4. Restarts the application automatically and makes it live for users.  

---

##  Technology Stack

| Category | Tools / Technologies |
|-----------|----------------------|
| **Language** | Java |
| **Build Tool** | Maven |
| **Version Control** | Git & GitHub |
| **Automation Tool** | Jenkins |
| **Cloud Platform** | AWS EC2 (Ubuntu 22.04) |
| **Server** | Apache Tomcat 10 |
| **Pipeline Type** | Jenkins Declarative Pipeline |

---

##  CI/CD Pipeline Workflow

**Automated Stages:**
1. **Code Checkout:** Jenkins pulls latest code from GitHub.  
2. **Build:** Maven compiles and packages code into a `.war` file.  
3. **Deploy:** Jenkins transfers `.war` to the Tomcat server via SSH.  
4. **Restart:** Tomcat service restarts automatically.  
5. **Verify:** Application is accessible instantly on the browser.

---

##  Jenkinsfile Overview

```groovy
pipeline {
    agent any

    environment {
        SERVER_IP   = '3.110.194.27'
        SSH_CRED_ID = 'node-app-key'
        TOMCAT_PATH = '/var/lib/tomcat10/webapps'
        TOMCAT_SVC  = 'tomcat10'
    }

    stages {
        stage('Checkout') {
            steps {
                echo ' Pulling code from GitHub repository...'
                git branch: 'main', url: 'https://github.com/mansikadam1100/student-registration-CICD.git'
            }
        }

        stage('Build WAR File') {
            steps {
                echo ' Building project using Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat Server') {
            steps {
                echo ' Deploying WAR to EC2 Tomcat Server...'
                sshagent(['node-app-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no target/*.war ubuntu@${SERVER_IP}:${TOMCAT_PATH}
                    ssh ubuntu@${SERVER_IP} 'sudo systemctl restart ${TOMCAT_SVC}'
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful! Application live on Tomcat server.'
        }
        failure {
            echo '❌ Build failed. Please check console logs for details.'
        }
    }
}
```

---

##  AWS EC2 Setup

| Instance | Purpose | Configuration |
|-----------|----------|----------------|
| **EC2-1** | Jenkins Server | Ubuntu 22.04, Jenkins, Java, Maven, Git |
| **EC2-2** | Application Server | Ubuntu 22.04, Apache Tomcat 10, Java |

###  SSH Configuration
- SSH key pair: **`node-app-key`**
- Private key added to Jenkins Credentials (via SSH Agent Plugin).  
- Passwordless SSH connection established between Jenkins and Tomcat servers.  

---

##  CI/CD Flow Summary

1. **Developer pushes code** → GitHub repository.  
2. **Jenkins webhook** triggers automatically.  
3. **Maven** builds and packages the `.war` file.  
4. **Jenkins** securely deploys the `.war` file to Tomcat (EC2).  
5. **Tomcat service** restarts automatically.  
6. Access the app via browser:

```
http://3.110.194.27:8080/student-app
```

---

##  Screenshots

| Screenshot | Description |
|-------------|-------------|
| ![](./img/photo_2025-11-11_11-39-36.jpg) | Student Admission Form deployed on Tomcat |
| ![](./img/photo_2025-11-10_23-00-18.jpg) | Jenkins Build Console Output |
| ![](./img/tomcat2.png) | Running EC2 Instances (Jenkins & Tomcat) |
| ![](./img/student1.png) | GitHub Repository |
| ![](./img/student.png) | Jenkinsfile in VS Code |

---

##  Key Learnings

 Configuring Jenkins Declarative Pipelines  
 Managing SSH-based deployments securely  
Automating Maven build and Tomcat deployments  
 Implementing Continuous Integration and Continuous Delivery on AWS EC2  


---

##  Conclusion

This project showcases a **complete DevOps CI/CD pipeline** for Java applications, integrating **Git, GitHub, Jenkins, Maven, AWS EC2, and Tomcat**.  
It represents a **real-world deployment automation workflow** with improved reliability, speed, and consistency.

>  Continuous Integration + Continuous Deployment = Faster Releases & Better Quality 

 *If this project helped you, don’t forget to star the repository!* 
