By Gwendolyn Faraday YT - https://www.youtube.com/c/FaradayAcademy.
Learn about Jenkins by building a CI/CD pipeline for a web application. Jenkins is an open source automation server which makes it easier to build, test, and deploy software. In this course, you will learn how to build a full dev-ops pipeline using Jenkins, Linode Servers, and other tools.

Jenkins, an open source automation software which makes application development easier to build, test and deploy software. Automate software development process by creating custom pipelines under devops methodologies. Makes more efficient and effective.

Glossary:
⭐️ Contents ⭐️
⌨️ ([00:00:00](https://www.youtube.com/watch?v=f4idgaq2VqA&t=0s)) Video Intro 
⌨️ ([00:01:33](https://www.youtube.com/watch?v=f4idgaq2VqA&t=93s)) Course Overview 
⌨️ ([00:02:36](https://www.youtube.com/watch?v=f4idgaq2VqA&t=156s)) What is Jenkins? 
⌨️ ([00:08:47](https://www.youtube.com/watch?v=f4idgaq2VqA&t=527s)) Terms & Definitions 
⌨️ ([00:11:58](https://www.youtube.com/watch?v=f4idgaq2VqA&t=718s)) Project Architecture 
⌨️ ([00:13:28](https://www.youtube.com/watch?v=f4idgaq2VqA&t=808s)) Linode Intro 
⌨️ ([00:20:18](https://www.youtube.com/watch?v=f4idgaq2VqA&t=1218s)) Setting Up Jenkins 
⌨️ ([00:24:11](https://www.youtube.com/watch?v=f4idgaq2VqA&t=1451s)) Tour of Jenkins Interface 
⌨️ ([00:30:33](https://www.youtube.com/watch?v=f4idgaq2VqA&t=1833s)) Installing Plugins 
⌨️ ([00:33:39](https://www.youtube.com/watch?v=f4idgaq2VqA&t=2019s)) Blue Ocean 
⌨️ ([00:34:55](https://www.youtube.com/watch?v=f4idgaq2VqA&t=2095s)) Creating a Pipeline 
⌨️ ([00:42:37](https://www.youtube.com/watch?v=f4idgaq2VqA&t=2557s)) Installing Git 
⌨️ ([00:45:15](https://www.youtube.com/watch?v=f4idgaq2VqA&t=2715s)) Jenkinsfile 
⌨️ ([00:46:27](https://www.youtube.com/watch?v=f4idgaq2VqA&t=2787s)) Updating a Pipeline 
⌨️ ([00:52:05](https://www.youtube.com/watch?v=f4idgaq2VqA&t=3125s)) Jenkins with nom 
⌨️ ([00:56:36](https://www.youtube.com/watch?v=f4idgaq2VqA&t=3396s)) Docker & Docker hub 
⌨️ ([01:02:14](https://www.youtube.com/watch?v=f4idgaq2VqA&t=3734s)) Closing Remarks

Resources:
# Jenkins
Follow-along instructions for the freeCodeCamp Jenkins course.
Table of Contents
- [Technologies Used in Tutorial](#technologies)
- [Follow Along Material and Extra Information for Sections of the Course](#sections)
- [Extra Material](#extra-material)
- [Helpful Jenkins Resources](#helpful-jenkins-resources)
- [Related Projects](#related-projects)
## Overview
This is a tutorial showing how to build a CI/CD pipeline for a web application using Jenkins. I use one of my previous tutorial apps - [the Curriculum App](https://github.com/faraday-academy/curriculum-app) - from my livestreams to create this. One interesting thing about that app is that I already [have Github actions written to deploy it automatically to Dockerhub](https://github.com/faraday-academy/curriculum-app/tree/dev/.github/workflows) in the repo. That will give you an opportunity to compare the Jenkins pipeline I set up in this tutorial to an already working CI/CD pipeline in the repo.

## Technologies
- Debian servers running on Linode (Jenkins from Linode marketplace)
- Jenkins
- Docker & Dockerhub
- Github
- Some command line for settings things up

### What is Jenkins?
Why would want you use Jenkins as an app developer?
- [Jenkins use cases](https://www.jenkins.io/solutions/)

Why do companies use Jenkins?
- [Jenkins users' YouTube playlist](https://www.youtube.com/playlist?list=PLN7ajX_VdyaNG5D3ERmAofba4-Fmqpe2i)

How does Jenkins compare to other CI/CD tools?
- [CI/CD Tools Comparison: Jenkins, GitLab CI, Buildbot, Drone, and Concourse](https://www.digitalocean.com/community/tutorials/ci-cd-tools-comparison-jenkins-gitlab-ci-buildbot-drone-and-concourse)
- [Comparing GitHub Actions vs Jenkins](https://acloudguru.com/blog/engineering/comparing-github-actions-vs-jenkins-ci-showdown)
- [Jenkins versus Gitlab](https://about.gitlab.com/devops-tools/jenkins-vs-gitlab/)
- [Aleternatives to Jenkins](https://alternativeto.net/software/jenkins/)
### Terms & Definitions
- [Jenkins official glossary of terms](https://www.jenkins.io/doc/book/glossary/)
### Project Architecture

![jenkins_architecture_balsamiq_diagram2](https://user-images.githubusercontent.com/10039233/190653544-48ea7cb1-bde8-4bf6-97b8-c1ff5bf854d2.png)
> NOTES:
> there is also a readme here on GitHub where i have each step in the process listed out as well as links and relevant resources and this architecture diagram of how everything fits together in the process
### Setting Up Linode
- [Signing up for Linode ($100 credit)](https://www.linode.com/students)
- [Jenkins in the Linode marketplace](https://www.linode.com/marketplace/apps/linode/jenkins/)
- Configuring Jenkins - admin account, plugins, settings
- Deploying an app to Github and using the Jenkins Github plugin  
- Using Jenkins to test and deploy to Dockerhub
- Another Linode server will be watching for updates on Dockerhub and pull & run the updated container  
- Reviewing Jenkins features like build history

### Setting Up Jenkins

**Make sure you follow these steps to set up Jenkins from the Linode marketplace**: https://www.linode.com/marketplace/apps/linode/jenkins/
- [Jenkins Handbook and Guides](https://www.jenkins.io/doc/book/)]

### Jenkins Plugins
- [Jenkins Plugin Index](https://plugins.jenkins.io/) - Search for plugins

### Blue Ocean UI
- [Blue Ocean Docs & Getting Started Guide](https://www.jenkins.io/doc/book/blueocean/)

### Jenkins Pipelines
Shell scripts from the tutorial:
```shell
ls -la
cd curriculum-front && npm i && npm run test:unit

docker build -f curriculum-front/Dockerfile . -t fuze365/curriculum-front

docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASSWORD

docker push fuze365/curriculum-front:latest
```

- [Getting started with Pipelines](https://www.jenkins.io/doc/book/pipeline/getting-started/)
- [In-Depth Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [Freestyle versus Pipeline Projects (old UI)](https://www.youtube.com/watch?v=IOUm1lw7F58)

### SSH'ing into Linode Servers (installing Git)
Commands for installing Git and Node on server:
```shell
# install Git
apt install git
git --version

# node version 16 (comes with npm)
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
apt-get install -y nodejs
node --version
npm --version
```

- [Using the Lish Console](https://www.linode.com/docs/guides/using-the-lish-console/)
- [Official video on how to SSH into Linode server](https://www.youtube.com/watch?v=ZVMckBHd7WA)
- [SSH into Linode server articles](https://www.linode.com/docs/guides/networking/ssh/)
- [How to install Git on Linux](https://www.linode.com/docs/guides/how-to-install-git-on-linux-mac-and-windows/)
- [How to install Node with npm on Linux](https://www.linode.com/docs/guides/install-and-use-npm-on-linux/)
### Jenkinsfile
- [Using a Jenkinsfile](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/)
- [Deep Dive into a Jenkinsfile video](https://www.youtube.com/watch?v=7KCS70sCoK0&list=PLy7NrYWoggjw_LIiDK1LXdNN82uYuuuiC&index=6)
### Docker & Dockerhub
- [Sign up for an account on Dockerhub](https://hub.docker.com/)
## Extra Material
There are two main ways to handle app deployments with our Jenkins setup:
1. We could deploy the app to Dockerhub and have the app server watch for changes to the docker image. It will pull whenever there are changes. (this is what we have set up now)
2. The more advanced setup is to have Jenkins deploy directly to our app server when all of the CI steps pass. This requires a bit of extra configuration in Jenkins and also ensuring security in the app server.

You can [see this tutorial](https://www.youtube.com/watch?v=hf8wUUrGCgU) for a walkthrough of how I set up the app server to watch for changes to a Docker image on Dockerhub.
## Helpful Jenkins Resources
- [Linode YouTube channel](https://www.youtube.com/c/linode)
- [Official Jenkins YouTube channel](https://www.youtube.com/c/jenkinscicd/playlists)
- [TechWorld with Nana YouTube channel](https://www.youtube.com/c/TechWorldwithNana/search?query=jenkins)
- [Cloudbees YouTube channel](https://www.youtube.com/c/CloudBeesTV/search?query=jenkins)
## Related Projects
- [Jenkins X](https://jenkins-x.io/)
Full devops piplein using jenkins, Instances, Github SCM, Docker and dockerhub.

---

## What is Jenkins

jenkins is a tool used for automation with continuous integration and continuous deployment pipelines. Opensource. it claims that it's the most commonly used automation server in software development. 

###### Why Jenkins
1)  it allows you to automatically watch for certain events in your repository and react to those events you can do things like

1. build your code run scripts 
2. perform testing
3. and then deploy your app if everything passes
4. or reject the deployment if the test fails
5. either way you will be able to see logs of every step throughout the jenkins pipeline and 
6. the results of the deployment

2) jenkins can ensure that all of your tests are run in the same environment or as many environments as you want for consistency
3) is that jenkins has 
- a very large community that is amassed over almost the last 20 years
- so there are lots of tutorials and
- community questions and answers that you can find
- another feature of jenkins is itsplug-in architecture to use jenkins effectively you will want to install the plugins that make sense for your project. these plugins can include options for,
1) compiling or testing
2) they have a docker plug-in which we will be using as well as 
3) a git plug-in that allows us to watch our github repository and react to changes
4) jenkins is self-hosted by default you don't need to pay for an enterprise tier this gives you a lot of options for configuration
5) it's also a tool that has been proven to scale since it's used by so many large companies around the world
6) it improves their software delivery cycles by making them faster and more performant
###### Benefits?
- open source
- community
- Plugins
- Self host
- No paywall
- Total control of configuration
- YT Jenkins and Usecases
###### Cons?
- Plugins - Dependent  on community for a longer dependent support + lack of documentation and updates
- Outdated UI
- External Plugin for an external UI
- Been for a long time might be outdated content.
- Frequent eyes on Version
- Completely selfhosted and maintained is con of no frugality by keeping it up-to-date, managing controller, agents, security and more. Being plugin dependent.
- Dependent on third party solutions to remediate  the above metrics.
###### Common terms to get relevant:
- **CI - Continuous Integration.**
-- **Continuous integration (CI) is the practice of automating the integration of code changes from multiple contributors into a single software project.** It’s a primary [DevOps best practice](https://www.atlassian.com/devops/what-is-devops/devops-best-practices), allowing **developers to frequently merge code changes into a central repository where builds and tests then run**. Automated tools are used to assert the new code’s correctness before integration.

A source code version control system is the crux of the CI process. The version control system is also supplemented with other checks like automated code quality tests, syntax style review tools, and more.

Developers practicing continuous integration merge their changes back to the main branch as often as possible. The developer's changes are validated by creating a build and running automated tests against the build. By doing so, you avoid integration challenges that can happen when waiting for release day to merge changes into the release branch. Continuous integration puts a great emphasis on testing automation to check that the application is not broken whenever new commits are integrated into the main branch.

- **CD - Continuous Delivery/Deployment**
-- **Delivery**: [Continuous delivery](https://www.atlassian.com/continuous-delivery) **continuous delivery is responsible for packaging the app and getting it ready to deploy using automated build tools

an extension of continuous integration since it automatically deploys all code changes to a testing and/or production environment after the build stage. 
This means that on top of automated testing, you have an automated release process and you can deploy your application any time by clicking a button.
In theory, with continuous delivery, you can decide to release daily, weekly, fortnightly, or whatever suits your business requirements. However, if you truly want to get the benefits of continuous delivery, you should deploy to production as early as possible to make sure that you release small batches that are easy to troubleshoot in case of a problem.

-- **Deployment**: Continuous deployment goes one step further than continuous delivery. With this practice, every change that passes all stages of your production pipeline is released to your customers. There's no human intervention, and only a failed test will prevent a new change to be deployed to production. Continuous deployment is an excellent way to accelerate the feedback loop with your customers and take pressure off the team as there isn't a "release day" anymore. Developers can focus on building software, and they see their work go live minutes after they've finished working on it.
- **Pipeline**:
-- **a  pipeline is basically a well-defined set of steps that build test and deploy applications automatically.** it basically takes you through the continuous integration and continuous delivery processes. Series of steps that build, test and deploy automatically to the environments.
-  **Build Step**
-- the build step will spin up an environment to compile code and create an executable if that's necessary. *when we talk about testing code that is a step of the pipeline that will run any unit end-to-end integration* or whatever other tests that are needed for the application to make sure that it's ready to deploy into production if the tests passes
- **Controller**
-- Controller is our main instance of Jenkins that is running the Jenkins controller is responsible for configuration key management plugins. it's a centralized hub to
manage all of the agents connecting to it.
- **Agents**
-- **agents** are usually containerized environments that will run your pipeline steps these steps are also called a job which means some work that needs to be done the Jenkins controller is responsible for assigning it to an agent and then the agent will either spin up and run all the steps of that job through to failure or deployment. 
you can also set the agent to just stay running all the time instead of spinning up for each job that you want to run.
## Project Architecture

![jenkins_architecture_balsamiq_diagram2](https://user-images.githubusercontent.com/10039233/190653544-48ea7cb1-bde8-4bf6-97b8-c1ff5bf854d2.png)
this is a simple diagram of the architecture.
Left one: Full stack app development side
SCM: Mono repo in GitHub
Right: 
1) Instances, one for Jenkins's Jenkins Controller
2) App server with Docker

-> Jenkins to watch the GitHub Mono Repo. 
-> Pulls when changes happens
-> After, will initiate CI/CD Process in one of the agents
-> Agents will run all the scripts/steps/jobs in the pipeline to compile the code to the designated branch we assigned in github.
-> Run test and deploy
-> If fails, stops and quits deployment. delivers reports and logs.

## Instances and Setting up Jenkins and App Server.
Directly run the Jenkins server from public marketplace image. Same for Docker.

Jenkins Server:
-> Open Jenkins Server's IP:8080
-> Go to the ss id_rsa dir to pick the key. and login
-> 1) Suggested or 2) manual plugins
-> Suggessted -> Installing
-> Create user and gain credentials
-> Note Jenkins URL
-> Ready!

##### Getting started with Jenkins: (Interface Tour)
-> Menu bar-> User management, Notification for error or warning messages. 
-> Dashboard and below New Item -> Props for different types of doable jobs listed as item
-> People -> User Management, IAM for different people and teams 
-> History -> Info about the jobs, Feedback
-> My Views -> New View/Items -> Configurable Custom Dashboard
-> Manage Jenkins - All of the configurations, Plugin and Key management to do all the actions like creating pipelines and run jobs.
-> Download LTS Version
-> Scroll for current version, can find the same on About Section.
-> Update all in the servers
->  Configuration
- Some of the main settings, Configure global systems of Jenkins and its path.
- This config page helps manage a lot of the core setting and features of Jenkins and its Plugins
- Manage Plugins -> Add, Install, UIpdate and Remote plugins
- Managing Nodes -> Default Node = Controller. To get started, Will be running jobs on this Default Node (good for experimentation). **Best to create one by yourself just for running jobs and Manage the same as a controller by the default builtin node**
- Manage Credentials -> When you need others nodes to run jobs, will need to add credentials for that to access other nodes.
- Manage Plugins -> Explore Plugins

## All about Plugins:

Plugin Manager:
Select Plugins -> Download and Install after Restart.
demo:
-> Blue Ocean -> Download and Install after Restart -> Check box - > Restart jenkins after installation
-> Login back in
-> Back to plugins
-> Docker, Git -> Do the same 

## Custom UI and Working with that by creating a Pipeline:

Open Blue Ocean 
-> Create Pipeline /shows existing pipeline -> 
-> Select SCM
-> Connect by required method:
1) e.g.: GitHub. Connect
2) Assess existing token /Create Access (by knowing and assess where the repo lies, hosted to be able to run jobs) 
3) Create Access Token -> P/w -> Settings -> Dev settings -> Personal Access token. -> IAM do permissions (default will be selected if u come by Jenkins)
4) Name and Expiry for the Token
5) Copy Token and Save
6) Back to Jenkins -> Click Connect
7) GitHub Access Granted via Jenkins -> Shows Organizations (select) -> Shows Repos (select) -> Create Pipeline
8) Pipeline Creator Screen //*Jenkins Integrated with the root dir. of the repo*.
9) Shows a pop up no **Jenkinsfile** found. Create one. **(Jenkinsfile is like the initiation script to get integrated parsed as a CI Pipeline) (After pipeline established, Jenkins will look for a jenkinsfile for it to be established.)**
10) To create one, Go Back to the repo -> Create **JenkinsFile**.  

## What is a Jenkinsfile:
> [! Definition]
> JenkinsFile, is a file that contains instructions for a pipeline scripted as code as instructions so you can read the file and see each step defined in the pipeline going from top to bottom divided into stages and steps. Like a initiation script, as same as a Dockerfile.
> 
this is a file it's checked into git and versioned and can be managed or rolled back just like any other file in git.

example script:
```Groovy
//Jenkinsfile (Declarative Pipeline)

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
```

 > Since, we create the pipeline via the blue ocean. The **Jenkinsfile** will automatically get created and committed to the repo.
 
## Recap pipeline after Jenkinsfile :
Process back through jenkins, 
1) you can see the message down at the bottom here saying a stage is required
> basically the pipeline is broken down into stages which are like sections
> every stage has to have at least one step and 
> -> the steps are what contain the scripts calls to jenkins plugins or other actionable tasks to be carried out in the pipeline 
> -> both stages and steps can be run in parallel or one after the other

2) **Naming a/each stage** - Name: Checkout code.
3) Adding a **Step** of the stage (right below) - Assign an actionable task or a Plugin. (GIT)
4) Declaring **Tasks** - enter the required data - url of the git repo(url), branch(dev),  
5) **Save** Pipeline - will save a jenkinsfile in the repo(if none it'll create one) - assign branch and Save.
6) Save Pipeline,
- Green indicated Successfull pipeline
- Red or others indicate any possible errors. if no green  CHECK LOGS, Troubleshoot and Rectify. (Possibly, any missing dependencies will cause errors).
- Yellow - Test Failure, Blue - Loading programs
- ***Jenkinsfile** gets created in the repo*

## Current Interface and Updating the Pipeline:

Home: 
Menu Bar: **Pipelines** - shows pipelines, **Administration** - Back to admin panel
Repo name of the pipeline: at the top left of the page: (redirects to home)
Home of the pipeline:
> Shows the list and history of the past, state of the job that has been running there

One of that Jobs Pipeline:
Menu Bar: **Pipeline** - Shows the log/info of the state of the pipeline, **Changes** - Updates assigned on that pipeline, **Test** - Any test cases ran on those pipeline, **Artifacts** - Saves the log of each action on that pipeline + other carriable data that we can assign the pipeline to do so. **Refresh** - Rerun any jobs, **Edit** - Can able to edit a pipeline. **Configure** - Back to jenkins admin panel and directly to the settings of that pipeline.

Pipelines Menu Bar:
**Activity**: Default, Shows tasks, **Branches**: Branches on that Pipeline with some icons - Run, Reload, Edit, Favorite, **Pull Request**: 

Pipeline's Branch - Edit Icon - there are different ways to arrange pipeline, like
- Adding more stages, 
- Back to previous stage and add more steps or adding multi stages which can run in parallel and assign the same, 
- and even delete it,
- In making steps, most of the tasks can be assigned from the options of the plugins have presented there. From simply running a shell script to manage a large scale complex tasks like AWS, Docker and more.
	- E.g.: Set a stage -> Name it -> Set Shell Script Step -> write script (ls -la) -> Commit -> Save and Run
	- -> Check repo -> Updated in Jenkinsfile -> New step is updated as a stage on that jenkinsfile

At times, the steps itself might become a Stage which get rectified by a random name, Runs a PR of that step.
To change a name of a stage:  Select stage -> Rename (to log) Save and Run (Commit)

> Note: A CAP: Sometime in SCM's like GitHub have API hit limitations which gets resets every hour wait till then or upgrade account.

Npm in a Pipeline:
Add a test stage -> Shell -> cd curriculum-front && npm i &&  npm run test: unit
runs Npm in Jenkins

## Deploying App on Docker using Jenkins:
let's add some stages so that we can finally deploy the app to docker hub

Stage: **Build**
Step: Shell Script
docker build -f repo(root or subDir)/Dockerfile . //-t containerName/app:tag
Commit -> Save and Run

Stage: **Login**
Step: Shell Script
docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASSWORD //since no ID/PWD has to be on Jenkins file, enter the same using an Environment Variable.
Back -> Settings -> Environment
Name and Value -> Key-Value Pair.
DOCKERHUB_USER: ID
DOCKERHUB_PASSWORD: Password
-> Save and run

Stage: **Push**
Step: Shell Script
docker push containerName/App:taglatest

