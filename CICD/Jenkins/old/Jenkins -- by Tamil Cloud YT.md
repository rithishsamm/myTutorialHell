# Jenkins with Tamil -1 | Install Jenkins - Run SSH Remote Host Job

**Install Java**
> [!cmd]
> sudo apt install openjdk-8-jre-headless java -version 

**Install Jenkins** 
**Step** 1 — Installing Jenkins. First, we'll add the repository key to the system. (kind of a dependency)

> [!CMD]
> wget -q -O - [https://pkg.jenkins.io/debian/jenkins...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGEzOE02Njc0dU1TbWdGaVpWYmszUFg3YjJOd3xBQ3Jtc0tsSEdyajBGV3E3M3NIZElYUG56WlZRMWVpX2pTc3VRdVFEdkdVWGhCTm0wVVlKVXBnSFNzUmxnSktOUGpMelM4TUpvOTdaaENhenk1VjRTdGE3SjVVaXRRbUQtZXMxMVhpUHNlN1dyTDgteloxVWljWQ&q=https%3A%2F%2Fpkg.jenkins.io%2Fdebian%2Fjenkins-ci.org.key&v=PL52Ch_erBo) | sudo apt-key add 

When the key is added, the system will return OK. Next, we'll append the Debian package repository address to the server's sources. List: 

> [!CMD]
> echo deb [https://pkg.jenkins.io/debian-stable](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbU1WZ2RIZURMN1FZekJFdjAtcXlZbktiUjRkZ3xBQ3Jtc0tuVU1wUnBtV21oUXVDNWdwSVZSbUwwelZPbnhoaUdkUGRnS055Q20xOU5IN1E5Skp3MHh4WVU5TW9oODExbVNTRzVfS1l4OV9hQmNlN1loS0stRHpUaWJGNUNxZGJyQklQX1VhSDUzQXpIX0NaaTBLUQ&q=https%3A%2F%2Fpkg.jenkins.io%2Fdebian-stable&v=PL52Ch_erBo) binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list 

When both of these are in place, we'll run update so that apt-get will use the new repository: 

> [!CMD]
> sudo apt-get update 
> sudo apt-get install jenkins 
> sudo systemctl start jenkins

Getting Started:

##### Agenda
- What is Jenkins
- What is CI (Continuous Integration)
- Installation and setting up Jenkins Server
- Methods and Implementation options
- What are plugins?
- Creating a job
- Scripting the same in a remote server

Plus,
- Gitlab setup
- Setting up Upstream and Downstream Jobs 
- Post kill action in real time
- Multiple integration methods
- Connecting to remote host.

##### WHAT IS JENKINS?
Nothing but a system which can be able to create, integrate, implement and schedule **jobs** with the help of Agents by simply scripting things. 
**Prerequisites**:
AWS:
- t2.micro instance - 1 (Jenkins Server)
- t2.micro instance - 1 (Remote App Server)
Log in to Jenkins Server:
> ssh -i pem.key ubuntu@pubIP

become a root user
> sudo su

update && upgrade 
> sudo apt-get update && upgrade -y

install java:
> sudo apt install java && jenkins -y

---

![[Pasted image 20240305111508.png]]

Prerequisites: 
- Jenkins
- Instances for Jenkins and remote server
- Repo manager
- Docker and more

What is Jenkins?
Is a tool for automation by CI/CD Pipelines. Opensource. Overwatching, perform testing, build code, run scripts and deploy application. 
![[Pasted image 20240305111528.jpg]]

As given in the above image,
-> The user pushes code to SCM via git
-> Do the builds and test cases by QA's using testing tools manually like maven and such. 
-> Does functionality tests on a system
-> Security Unit testing on an other hand on an another tool
-> API Testing on some
-> Integration tests with different platforms with different dependencies such as email triggers, 
> If we integrate all these processes with Jenkins means, We can do these all by automating these tasks by scripting to runs its own jobs. 

> Jenkins achieve this by analyzing a merge request done by the developer to the SCM or Repo manager. 
> 
> Jenkins will pull that branch and run jobs. and does the same incrementally.
> 
> With the concept of CI/CD Practices, Jenkins will automate tasks by following scripts to create jobs for it get tested and passed of the cases they've objected. will be shown by logs and reports.

> Actions can be automated by creating jobs to do such tasks. + There are plugins support to achieve such objectives.

Eg Jobs like:
- Test selenium based on values
- Gives log and the status of such jobs.
- If such jobs gets successful deploy it by yourself. if not script what it has to do.
- Runs execution commands after successful jobs such as deployment, synchronization, hosting and more
- Triggering builds with mail, link, manual triggers and more
- Bunch of plugins support.
- Jenkins 2.0 - Master Slave Concept. We're gonna deploy centralized single master runners.
- Install and get started.
##### Available Installation Methods:
- Docker Container
- Dedicated CI Server
- Tomcat by running Jenkins WAR File. 
- Selfhost as an individual service. sudo apt install java Jenkins

##### Install Jenkins in jenkins server
1) -> Installation - > Jenkins
2) Refer digital ocean for docs> How to install Jenkins in Amazon EC2
3) Root -> Update  -> Install -> systemctl start -> status -> enable > Jenkins server
4) Jenkins -> Open -> Install Suggested Plugins -> Install essential plugins -> Login -> Put Admin Password -> Run and Restart
5) Login -> Username -> Password -> Mail ID -> Full name
6) Instance Configuration -> Jenkins url -> 
7) Save and Continue
8) Start using Jenkins

#### Dashboard
1) New Item -> To create jobs, pipeline and more
2) Build History
3) Manage Jenkins
4) My Views
5) Open blue ocean (extension)
-> Build Queue
-> Build exec status

#### creating a job
1) Jenkins -> Create Item -> Name -. Freestyle Project -> Create
2) Shows
- General
- Source Code Management
- Build Triggers
- Build Environment
- Build Steps
- Post-build Actions
=> Basic Common Application on Jenkins Building a job

