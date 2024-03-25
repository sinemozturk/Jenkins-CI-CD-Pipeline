# **JENKINS - CI/CD PIPELINE HANDS-ON**


![](./images/JENKINS-PROJECT-SINEM-INTRO%20(3).jpg)

Jenkins is an open-source automation server that helps automate the non-human part of the software development process. It is used primarily for Continuous Integration (CI) and Continuous Deployment (CD) workflows. Jenkins allows developers to automate the building, testing, and deployment of software applications, making the development process more efficient and reliable.


# Key features of Jenkins include:

`Easy Installation:` Jenkins is easy to install and configure on various operating systems like Windows, macOS, and Linux.

`Extensibility: `Jenkins offers a wide range of plugins that extend its functionality. These plugins allow integration with various tools and technologies used in the software development lifecycle.

`Distributed Builds: `Jenkins supports distributed builds, allowing you to distribute build tasks across multiple machines to improve performance and scalability.

`Pipeline Support: `Jenkins supports the definition of build pipelines as code using Jenkinsfile, which allows developers to define complex build and deployment workflows.

`Integration: `Jenkins can integrate with version control systems like Git, build tools like Maven and Gradle, testing frameworks, and deployment tools, making it a central hub for the software development process.

`Monitoring and Notifications: `Jenkins provides monitoring capabilities, allowing developers to track the status of builds and deployments. It also supports notifications via email, chat services like Slack, and other communication channel



# Install Jenkins on AWS Ubuntu Server


- To begin the project, I initiated the server creation process on AWS using the Ubuntu Server 20.04 LTS (HVM), SSD Volume Type AMI. I opted for a t2.micro instance type with an 8 GB volume. For security configuration, I set up a Security Group allowing SSH access on port 22 and HTTP access on port 8080. This setup provides a solid foundation for further development and deployment within the AWS.

    - AMI: Ubuntu Server 20.04 LTS (HVM), SSD Volume Type
    - Instance Type: t2.micro
    - Volume: 8 GB
    - Security Group Ports (Inbound Rules): SSH (22), custom tcp (8082) 


- TCP configuration:

    - Add Rule for Port 8082: Add a new rule to allow inbound traffic on port 8082. Specify the following:

        - Type: Custom TCP Rule
        - Protocol: TCP
        - Port Range: 8082
        -Source: You can specify either a specific IP address, a range of IP addresses (CIDR notation), or leave it open to all (0.0.0.0/0 or ::/0) depending on your security requirements.


- To remotely access your Ubuntu machine using an SSH connection, you can choose your preferred tool, such as MobaXterm or VSCode. Let's start by updating all the packages


```bash
sudo apt update -y
```

- Output: 

![](./images/sudo%20apt%20result.PNG)


- Java is a fundamental requirement for running Jenkins and its associated plugins, executing Jenkins Pipeline scripts, building Java applications, and integrating with Java-based tools and frameworks. Therefore, it's essential to have Java installed on an AWS Ubuntu Jenkins server to ensure the proper functioning of Jenkins and its related tasks. Let's download it as well. I will use version 11. 


```bash
sudo apt install openjdk-11-jre -y
```

- Output: 

![](./images/java%20intalled.PNG)

- You can always check the version with ` java --version ` command.


- For jenkins you can go https://updates.jenkins-ci.org/download/war/ page to choose your version of Jenkins and copy the URL, We will use 2.387.3. It will download as a war file.

- You can always check the file is it downloaded or not with `ls` command. 

```bash
sudo wget https://updates.jenkins-ci.org/download/war/2.387.3/jenkins.war

```

- Output: 

![](./images/jenkins%20war%20installed.PNG)


- Following command launches Jenkins as a standalone server, making it accessible via a web browser at http://<your_ubuntu_machine_ip>:8082, where <your_ubuntu_machine_ip> is the IP address of your Ubuntu machine.


```bash
java -jar jenkins.war --httpPort=8082
```

- Code Breakdown:
    - `java:` Initiates the Java runtime environment.
    - `-jar jenkins.war:` Instructs Java to execute the jenkins.war file as a standalone Java application. The jenkins.war file is the Jenkins web application archive, containing all the necessary files to run Jenkins.
    - `--httpPort=8082:` Specifies the port on which Jenkins will listen for HTTP connections. In this case, Jenkins will be accessible through port 8082.


- After running the command, it will output the autontentication token as a password. Make sure you stored that token separetly. You will need it to login into Jenkins server.  

![](./images/adminpassword.PNG)


- Go to AWS Console and connect to your ec2 instance with browser by using `Public Ipv4 address` of your ec2 instance make sure you add `:8082` at the end of your address. `http://<your_ubuntu_machine_ip>:8082`. If everything is working fine skip troubleshooting part to Jenkins Dashboard Setting -->

## Troubleshooting Jenkins Not Running 

If you've opened port 8082 on the AWS security group associated with your Ubuntu instance and you're still unable to connect to Jenkins, here are a few additional troubleshooting steps you can take:

- Check Security Group Configuration: Double-check the security group configuration to ensure that the inbound rule for port 8082 is correctly added and applied to your instance.

![](./images/inbound%20rules.PNG)

- Verify Jenkins is Running: Log in to your Ubuntu instance via SSH and verify that Jenkins is running. You can use the command `ps -ef | grep jenkins` to check if the Jenkins process is running.

- If you see something like following output it means that the `grep` command itself is being detected in the process list, but `there is no Jenkins process running.` This suggests that Jenkins is not currently running on your Ubuntu instance.

![](./images/jenkins%20not%20working.PNG)

- To start Jenkins, you need to run the command java -jar jenkins.war --httpPort=8082 in the directory where the jenkins.war file is located. Here's how you can do it:

```bash
cd /path/to/jenkins/directory
java -jar jenkins.war --httpPort=8082
```


# Jenkins Dashboard Setting

- After successfully navigate the 8082 port, paste the password that is generated with previous command.

![](./images/jenkins%20up%20and%20running.PNG)

- Next: Install suggested plugins. 

![](./images/plugings.PNG)

- Next: Create an admin account.

![](./images/CREATE%20ADMIN.PNG)

- Next: Ready to go.

![](./images/JENKINS%20READY.PNG)

- Next: Jenkins Dashboard Landing Page.

- ![](./images/JENKINS%20DASHBOARD.PNG)



# **Plugins Explained**

![](./images/plugin.png)


In Jenkins, `plugins` are add-ons or extensions that provide additional functionality to the core Jenkins application. These plugins allow users to customize and extend Jenkins to suit their specific needs and requirements. Plugins can add features such as source code management, build triggers, build steps, notifications, integrations with other tools, and more.

Here are some common types of plugins in Jenkins:

- `Source Code Management (SCM) Plugins:` These plugins enable Jenkins to integrate with various version control systems such as Git, Subversion (SVN), Mercurial, etc. They allow Jenkins to pull source code from repositories during builds.

- `Build Tool Plugins:` Plugins for build tools like Apache Maven, Gradle, Ant, etc., enable Jenkins to build projects using these tools. They provide integration with these build systems, allowing Jenkins to execute build tasks.

- `Notifier Plugins:` Notifier plugins allow Jenkins to send notifications about build results via email, instant messaging, or other communication channels. Examples include email notification plugins, Slack notification plugins, etc.

- `Reporting Plugins:` Reporting plugins generate reports about build results, test results, code coverage, static code analysis, etc. These reports can help teams analyze the quality and health of their projects.

- `Integration Plugins:`Integration plugins allow Jenkins to integrate with various third-party tools and services, such as cloud providers (AWS, Azure), issue tracking systems (JIRA, GitHub Issues), collaboration platforms (Slack, Microsoft Teams), etc.

- `Authentication and Authorization Plugins:` These plugins provide different authentication and authorization mechanisms for Jenkins, allowing administrators to control access to Jenkins resources based on user roles and permissions.

- `Pipeline Plugins:`Jenkins Pipeline is a suite of plugins that allows users to define and manage build pipelines as code. Pipeline plugins enable users to create continuous delivery pipelines using Groovy DSL or Jenkinsfile.

- `Utility Plugins:` Utility plugins provide miscellaneous functionality to Jenkins, such as parameterized builds, build triggers, environment variable manipulation, etc.

# Manage Jenkins

## Install Required Plugins for This Project

- Go to `Manage Jenkins` tab, and navigate `Manage Plugins` this is the place where actually you can download project related tools and technologies' plugins. Navifate to `Available Plugins` tab.

    ![](./images/manage%20plugins.PNG)

    - write `jdk` on search bar and click and install without restart following plugins;
        - `Eclipse Temurin installer`
        - `openJDK-native-pluginVersion `

        ![](./images/jdk%20plugins.PNG)

    - write ` owasp ` on search bar and click and install without restart following plugins;
        - `OWASP Dependency-CheckVersion 5.5.0 `  
    - write ` docker ` on search bar and click and install without restart following plugins;
        - `DockerVersion`
        - `Docker PipelineVersion`
        - `docker-build-stepVersion`
        - `CloudBees Docker Build and PublishVersion`
    - write ` sonarqube ` on search bar and click and install without restart following plugins;
        - `SonarQube ScannerVersion `

## Global Tool Configuration

In Jenkins, the "Global Tool Configuration" section allows administrators to configure and manage global tools that can be used across all Jenkins jobs. These tools typically include build tools, version control tools, JDK installations, and other command-line utilities required for software development and build processes.

- Navigate the `Global Tool Configuration` bar on dashboard. We will configure Java, Maven, Docker and Dependency-Check.

    ![](./images/global%20tool%20conf.PNG)

    - For Java `JDK` give name as `jdk`, click `Install automatically` and choose the version of jdk. You can see in details with following; 

        ![](./images/tool%20jdk%20config.PNG)

    - For Maven  give name as `maven`, click `Install automatically` and choose the version of maven. You can see in details ;

        ![](./images/maven.PNG)

    - For Dependency-Check give name as `dp`, click `Install automatically` and choose the version of maven. You can see in details ;

        ![](./images/dependency.PNG)

    - For Docker give name as `docker`, click `Install automatically` and choose the version of maven. You can see in details ;

        ![](./images/docker.PNG)


    - Save and Apply changes. 


## Configure System

In Jenkins, the "Configure System" section allows administrators to configure global settings and options that apply to the entire Jenkins environment. This includes settings related to system-wide configurations, tools, security, email notifications, plugin management, and more. 


![](./images/configure%20system.PNG)

- Configure system is where you can configure different servers. For example when we're using sonarqube, we're pushing reports. To be able to connect different servers, tool we need to configure the systems in Jenkins. 


## Manage Nodes and Clouds


In Jenkins, managing nodes and clouds involves configuring and maintaining the infrastructure where your jobs will run. Nodes represent individual machines (whether physical or virtual) that Jenkins can use to execute tasks, while clouds represent groups of nodes that can be dynamically provisioned and removed based on workload.

![](./images/manage%20nodes%20and%20cloud.PNG)


- Manage Nodes & Cloud is where you can see which nodes, VMs or Slaves are being used to run your projects.

# **Security**
"Security" under "Manage Jenkins" is the section where you can configure authentication, authorization, and other security-related settings to control user access and permissions within your Jenkins instance.

![](./images/security.PNG)

### Configure Global Security

This section allows you to configure authentication and authorization settings for Jenkins, including options for enabling security, setting up authentication realms (such as LDAP, Active Directory, etc.), defining authorization strategies (such as matrix-based security, role-based access control), and managing user permissions.

### Credentials

In this section, you can manage credentials used by Jenkins for various purposes, such as connecting to source code repositories, accessing external systems, or running jobs. You can add, edit, delete, and organize credentials, including usernames and passwords, SSH keys, secret text, certificates, and more.

### Credential Providers

Here, you can configure credential providers that Jenkins can use to retrieve credentials dynamically, allowing you to manage credentials externally or integrate with third-party credential management systems. Examples include the Jenkins own database, Jenkins global credentials, Windows Credentials, SSH user private key, and more.

### Manage Users

This section provides user management capabilities, allowing administrators to view, create, edit, and delete user accounts in Jenkins. You can also assign roles, groups, and permissions to users, manage user passwords, and configure other user-related settings.


# What is JOB ?

![](./images/jobs.PNG)

- In Jenkins, a `"job" ` refers to a specific `task` or `process` that Jenkins can execute as part of a `continuous integration (CI)` or `continuous delivery (CD)` pipeline. Jobs are typically configured to perform actions such as compiling code, running tests, deploying applications, or executing other custom scripts or commands.

- Each job in Jenkins is configured with its own set of parameters, such as the source code repository to monitor, build triggers (e.g., polling SCM, webhook notifications), build steps (e.g., shell commands, Maven targets, Gradle tasks), post-build actions (e.g., sending notifications, archiving artifacts, triggering downstream jobs), and other settings related to environment configuration, build parameters, and version control integration.

- Jobs in Jenkins are organized within "job views" or folders, allowing users to categorize and manage related jobs more efficiently. Additionally, Jenkins provides features for tracking job history, monitoring build status, and analyzing build trends through various built-in reports and visualizations.

Overall, jobs are the fundamental building blocks of automation in Jenkins, enabling developers and teams to automate software development processes and streamline the delivery of software products.











