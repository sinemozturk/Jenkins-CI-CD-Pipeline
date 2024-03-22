# JENKINS - CI/CD PIPELINE HANDS-ON


![](./images/logo-title-opengraph.png)

Jenkins is an open-source automation server that helps automate the non-human part of the software development process. It is used primarily for Continuous Integration (CI) and Continuous Deployment (CD) workflows. Jenkins allows developers to automate the building, testing, and deployment of software applications, making the development process more efficient and reliable.


# Key features of Jenkins include:

`Easy Installation:` Jenkins is easy to install and configure on various operating systems like Windows, macOS, and Linux.

`Extensibility: `Jenkins offers a wide range of plugins that extend its functionality. These plugins allow integration with various tools and technologies used in the software development lifecycle.

`Distributed Builds: `Jenkins supports distributed builds, allowing you to distribute build tasks across multiple machines to improve performance and scalability.

`Pipeline Support: `Jenkins supports the definition of build pipelines as code using Jenkinsfile, which allows developers to define complex build and deployment workflows.

`Integration: `Jenkins can integrate with version control systems like Git, build tools like Maven and Gradle, testing frameworks, and deployment tools, making it a central hub for the software development process.

`Monitoring and Notifications: `Jenkins provides monitoring capabilities, allowing developers to track the status of builds and deployments. It also supports notifications via email, chat services like Slack, and other communication channel



# INSTALL JENKINS ON UBUNTU MACHINE 


- To begin the project, I initiated the server creation process on AWS using the Ubuntu Server 20.04 LTS (HVM), SSD Volume Type AMI. I opted for a t2.micro instance type with an 8 GB volume. For security configuration, I set up a Security Group allowing SSH access on port 22 and HTTP access on port 8080. This setup provides a solid foundation for further development and deployment within the AWS.

    - AMI: Ubuntu Server 20.04 LTS (HVM), SSD Volume Type
    - Instance Type: t2.micro
    - Volume: 8 GB
    - Security Group: SSH (22), HTTP(8080)

- First let's update the all packages.

```bash
sudo apt update -y
```

- Java is a fundamental requirement for running Jenkins and its associated plugins, executing Jenkins Pipeline scripts, building Java applications, and integrating with Java-based tools and frameworks. Therefore, it's essential to have Java installed on an AWS Ubuntu Jenkins server to ensure the proper functioning of Jenkins and its related tasks. Let's download it as well. I will use version 11. 


```bash
sudo apt install openjdk-11-jre -y
```

- You can always check the version with ` java --version ` command.


- For jenkins you can go https://updates.jenkins-ci.org/download/war/ page to choose your version of Jenkins, We will use 2.387.3. It will download as a war file.

```bash
sudo wget https://updates.jenkins-ci.org/download/war/2.387.3/jenkins.war

```

- You can always check the file is it downloaded or not with `ls` command. 

- Stat

```bash
java -jar jenkins.war --httpPort=8082
```

- Running `java -jar jenkins.war` is a common way to `start a Jenkins server`. Here's what each part of the command does:

    - `java: `This invokes the Java Virtual Machine (JVM), which is needed to execute Java applications.
    - `-jar:` This option specifies that the next argument is the path to a JAR file that should be executed.
    - `jenkins.war: `This is the filename of the Jenkins WAR (Web Application ARchive) file. It contains the Jenkins application and all its dependencies bundled together.
    - `--httpPort=8082:` This parameter specifies the port number (8082 in this case) on which Jenkins will listen for incoming HTTP requests. By default, Jenkins listens on port 8080. However, using this parameter allows you to specify a different port if port 8080 is already in use or if you prefer a different port for Jenkins.

- After running the command, it will output the autontentication token as a password. Make sure you stored that token separetly. You will need it to login into Jenkins server.  

- Go to AWS Console and connect to your ec2 instance with browser by using `Public Ipv4 address` of your ec2 instance make sure you add `:8082` at the end of your address. 

- Create an admin account with privious password that is generated by Jenkins installation
