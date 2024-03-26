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
    - Instance Type: t2.medium
    - Volume: 8 GB
    - Security Group Ports (Inbound Rules): SSH (22), custom tcp (8082) for jenkins , custom tcp (8080) for tomcat


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


- We will use a java source code we need maven to build so install maven

```bash
sudo apt install maven

```


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
        - `openJDK-native-plugin `

        ![](./images/jdk%20plugins.PNG)

    - write ` owasp ` on search bar and click and install without restart following plugins;
        - `OWASP Dependency-CheckVersion 5.5.0 `  
    - write ` docker ` on search bar and click and install without restart following plugins;
        - `Docker`
        - `Docker Pipeline`
        - `docker-build-step`
        - `CloudBees Docker Build and Publish`
    - write ` sonarqube ` on search bar and click and install without restart following plugins;
        - `SonarQube Scanner `
c65dc14331cd45c5bbb8a577c2b13996
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

Overall, jobs are `the fundamental building blocks of automation` in Jenkins, enabling developers and teams to automate software development processes and streamline the delivery of software products.



# FreeStyle Project

- Navigate the Jenkins Dashboard, choose `create a job`.

- Give a name to your project and choose `freestyle project`.

- Scroll down to `Source Code Management` (where we configure the source code) choose `Git` go to https://github.com/jaiswaladi246/Petclinic and copy the URL of the repository. (Thank you for this project!) and paste it under the `Repository URL`. Make sure the repository branch is `main`

![](./images/gittt.PNG)


- Since this is the maven based application go to `Build Steps` and choose `Invoke top-level Maven target` tab anc choose version of maven as `maven` and goal section write `clean compile` & `clean package` like following, do not forget to add 2 build steps and configure each separetly. The reason is doing that to show you can add multiple stages into a project.

![](./images/build%20steps.PNG)

- Now scroll down and go to `Post build actions`.The "Post-build Actions" section allows you to define actions that should be performed `after the completion of a build job`. These actions typically include tasks such as `archiving artifacts`, `triggering downstream jobs`, `sending notifications`, p`ublishing reports`, and `cleaning up resources`. But for now we won't do anything. 

![](./images/post%20build%20action.PNG)

- The reason we see the list here is based on our plugins that we installed at the beginning. 

- Now save it and from left side bar choose `build now` to build your project. You can see the detailed logs related the project by clicking build history on `#1`. 

![](./images/build%20now.PNG)

- As we can see the job run successfully. 

![](./images/build%20run%20success.PNG)


- In this project we are going to use `Tomcat` for deployment. Let's download it as well. (It is better to not interrupt previous terminal, just open new terminal to re-connect to your server with ssh connection. )


- First let's change the user from ubuntu to root with `sudo su` command and go to `/opt` file with `cd` command.

```bash
sudo su
cd /opt
```

![](./images/sudo%20su.PNG)

- For detailed about download for Tomcat server please go to https://github.com/sinemozturk/Apache-Tomcat-Hands-on.git 


```bash
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.67/bin/apache-tomcat-9.0.67.tar.gz 

```

![](./images/tomcat%20tar.PNG)

- Extract the zip file

```bash
sudo tar -xvf apache-tomcat-9.0.67.tar.gz
```

![](./images/tomcat%20extract.PNG)


-  Now we need to make some changes in the tomcat server to make sure Tomcat is allowing the administrative access through its web interface by adding a new user with appropriate roles to the tomcat-user.xml configuration file. Run following command to start making changes.

```bash
sudo vi tomcat-users.xml
# ---add-below-line at the end (2nd-last line)----
# <user username="admin" password="admin1234" roles="admin-gui, manager-gui"/>
```

![](./images/add%20vi.PNG)


- Now we need to create a symbolic link allows for easier access to the Tomcat startup script, enabling users to start Tomcat by simply executing startTomcat from the command line, without needing to navigate to the Tomcat installation directory.


```bash
sudo ln -s /opt/apache-tomcat-9.0.67/bin/startup.sh /usr/bin/startTomcat
```

- Now we need to create a symbolic link allows for easier access to the Tomcat shutdown script, enabling users to stop Tomcat by simply executing stopTomcat from the command line, without needing to navigate to the Tomcat installation directory.

```bash
sudo ln -s /opt/apache-tomcat-9.0.67/bin/shutdown.sh /usr/bin/stopTomcat
```

- Now we need to configure  `manager` the context.xml file with following command

```bash
sudo vi /opt/apache-tomcat-9.0.67/webapps/manager/META-INF/context.xml


#We need to comment following scripts :
# <Valve className="org.apache.catalina.valves.RemoteAddrValve"
  #       allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
```

![](./images/valve.PNG)

- Now we need to configure the `host-manager` in the context.xml file. This file typically contains configuration settings specific to the host-manager web application in Apache Tomcat.

```bash
sudo vi /opt/apache-tomcat-9.0.67/webapps/host-manager/META-INF/context.xml
#We need to comment following scripts :
# Valve className="org.apache.catalina.valves.RemoteAddrValve"
# allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> 
```

![](./images/hostmanager.PNG)


- Run separetly following commands

```bash
sudo stopTomcat
sudo startTomcat
```

- You can access the tomcat landing page with  `http://<publicIpofyourserver>:8080` do not forget to delete s from https


![](./images/tomcat.PNG)


- Go back to your jenkins server and find your freestyle project from dashboard and click `configure`on the left side bar. 

![](./images/free-modify.PNG)


# Pipeline Project

- Go to dashboad and click `New Item` write a name for your job, and choose `Pipeline`. 

- Click `Discard old builds` write `1` to `Max # of builds to keep` 

![](./images/discard.PNG)

- Scroll down to `Pipeline` and choose `Execute Shell` and write following code; 




```groovy

pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jaiswaladi246/Petclinic.git'
            }
        }
    }
}

```



- let's break down this Jenkins pipeline script:

    - `Agent:` The agent directive specifies the type of Jenkins agent to use for running the pipeline. In this case, any is specified, which means the pipeline can run on any available agent in the Jenkins environment.

    - `Stages:` The stages block defines the different stages of the pipeline. Each stage represents a logical division of work in the pipeline.

    - `Stage('Git Checkout'):` This is a stage named "Git Checkout," which indicates that the steps within this stage will perform a Git checkout operation.

    - `Steps:` The steps block contains the specific actions or steps to be executed within the stage. In this case, it contains a single step that performs a Git checkout operation from a specific branch (main) of a Git repository located at the specified URL (https://github.com/jaiswaladi246/Petclinic.git).

- And apply changes and go back to pipeline and `Build Now`

![](./images/git%20checkout.PNG)


- This is the output of the build. You can see in detail what it does in order.

![](./images/console1.PNG)

- Now we will add different stages into our pipeline to be able do this, go to `pipeline` job and on the left tab navigate to `Configure`. And follow the steps; 


- First of all we will use Maven and Java tools in the pipeline script but to be able to achieve this we need to use `tools`  directive at the beginning of our stages.


```groovy
// define the maven and java tools to be able to run their command
tools{
    jdk 'jdk11'
    maven  'maven'
}
```


- Maven Compile Stage: Add following command under the `Git Checkout ` Stage 



```groovy
        stage('Compile') {
            steps {
                sh "mvn clean compile"
            }
        }
```

- In this stage:

    - The `sh` step is used to execute the Maven command mvn clean compile.
    - `mvn clean compile` cleans the project (removes any existing build artifacts) and then compiles the source code.


- Now go back to pipeline and build the pipeline with `build now`. You will see the second stage of our pipeline is running perfectly

![](./images/build2.PNG)


- You can alwasy go and see the logs (what this stage does?) by clicking build number and go to `console output` as you can see in the following;

![](./images/build%20history.PNG)
![](./images/consoleoutput.PNG)

- Output will look like this;

![](./images/maven%20compile1.PNG)
![](./images/maven%20compile2.PNG)

- It appears that your Maven project is successfully compiling 47 source files to the `/root/.jenkins/workspace/pipeline/target/classes` directory. This output indicates that the compilation process is progressing as expected. After the compilation process completes, the compiled classes will be available in the target/classes directory of your Maven project.

- You can the target file see by clicking `Workspaces`

![](./images/workspace.PNG)


- Before we go to Maven clean Package Stage;

- Tomcat is not giving access to other users to copy the artifacts. In this part we need to change the permission of apache folder you can learn the details about apache tomcat you can go back to my previous project -> https://github.com/sinemozturk/Apache-Tomcat-Hands-on.git 

    - Go to   /opt folder path by `cd /opt`
    - run the `ls -lrt` command to see all permission the file has. 
    - run the `chmod 757 <apache folder name>`

![](./images/chmod.PNG)

- Go and configure the pipeline by adding following stage and build it. 

```groovy
stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }
```


- In this stage:

    - The `sh` step is used to execute the Maven command mvn clean package.
    - `mvn clean package` cleans the project, compiles the source code, runs any tests, and packages the application. The resulting artifact (e.g., JAR, WAR) will be created in the target directory of your Maven project.

![](./images/build2222.PNG)    


- You can see the project's war file is being created under the target folder. We are succesfully generated the artifacts.

![](./images/war%20file.PNG)

- Deploy the application by adding following stage to your pipeline and build it. 


```groovy
stage('Deploy') {
            steps {
                sh "sudo cp target/petclinic.war /opt/apache-tomcat-9.0.67/webapps" 
            }
        }
```


- In this stage:

    - The  `sh` step is used to execute the shell command sudo cp target/petclinic.war /opt/apache-tomcat-9.0.67/webapps, which copies the WAR file generated during the build stage to the Tomcat webapps directory.
    - Ensure that the path `/opt/apache-tomcat-9.0.67/webapps` corresponds to the correct location of your Tomcat installation's `webapps directory.` Adjust the path accordingly if your Tomcat installation is located in a different directory.
    - The `sudo` command is used assuming that copying to the Tomcat directory requires elevated permissions. Ensure that Jenkins has appropriate permissions to execute this command using sudo.
    - `Replace petclinic.war `with the name of your WAR file if it differs.
    Make sure that Tomcat is properly configured to deploy WAR files from its webapps directory.

![](./images/deploy.PNG)


- When you go to your tomcat browser of your jenkins server `http://yourpublicipofjenkinsserver:8080/petclinic` you will be able to see the application is up and running by using jenkins pipeline. 

![](./images/final.PNG)