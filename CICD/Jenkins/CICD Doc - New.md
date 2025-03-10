Setting up a **Multibranch Pipeline** in Jenkins to pull a Docker Compose application from a self-hosted Gitea repository and deploy it to a remote Windows server (running Docker Desktop) involves several steps. Here's a step-by-step guide to help you achieve this setup:

### Prerequisites

1. **Jenkins Installed** (preferably on a Linux-based server).
2. **Docker Desktop** installed on your **Windows server**.
3. **Gitea** self-hosted and running with access to the repository.
4. **Jenkins Gitea Integration** plugin installed.
5. **Jenkins Docker** plugin installed (for remote execution).
6. **SSH access** to your Windows server, enabling Docker commands to run remotely from Jenkins.

### Step 1: Set Up Jenkins

#### 1.1 Install Necessary Jenkins Plugins
Make sure Jenkins has the following plugins installed:

- **Git**: To interact with the Gitea repository.
- **Docker Pipeline**: To execute Docker commands within Jenkins.
- **Multibranch Pipeline**: For managing multiple branches in your repository.
- **Gitea Plugin**: To integrate Jenkins with your Gitea server.

Go to **Manage Jenkins** > **Manage Plugins** > **Available** and install the necessary plugins.

#### 1.2 Add Docker Remote Host in Jenkins
You’ll need to configure Jenkins to interact with the Docker Desktop running on your remote Windows server:

- Go to **Manage Jenkins** > **Configure System**.
- Scroll down to **Docker** (Docker Cloud), and click **Add Docker Cloud**.
- In the **Docker Host URI** section, provide the SSH details for your remote Windows machine. The format will be something like:
  
  ```bash
  ssh://username@<windows-server-ip>:<port>
  ```

  - The port will generally be `22` for SSH.
  - You'll need an SSH key set up on your Windows server for Jenkins to use to authenticate.

### Step 2: Set Up Gitea Access in Jenkins

#### 2.1 Add Gitea Credentials
You’ll need to configure Jenkins to access your Gitea repository:

- Go to **Manage Jenkins** > **Manage Credentials**.
- Under **(Global)** > **Add Credentials**, select **Username with password**.
- Enter the **Gitea username** and **personal access token** (which you should generate in Gitea).

#### 2.2 Configure Gitea Webhook
In your **Gitea** repository, set up a **webhook** that will notify Jenkins whenever a commit or push is made to the repository. This is how Jenkins will be alerted to trigger the build process.

- Go to your **Gitea repository** > **Settings** > **Webhooks** > **Add Webhook**.
- In the **URL** field, enter the Jenkins webhook URL, which will be in this format:

  ```
  http://<jenkins-server-url>/gitea-webhook/
  ```

- Make sure the **Push events** option is selected.

### Step 3: Configure Jenkins Multibranch Pipeline

#### 3.1 Create a New Multibranch Pipeline

- In Jenkins, go to the **Dashboard** and click **New Item**.
- Enter a name for your pipeline, e.g., `docker-deployment`.
- Select **Multibranch Pipeline** and click **OK**.

#### 3.2 Configure Git SCM in the Pipeline

- Under **Branch Sources**, click **Add Source** > **Git**.
- In the **Repository URL** field, enter your **Gitea repository URL**. For example:
  
  ```
  https://<gitea-server>/username/repository.git
  ```

- Under **Credentials**, select the credentials you created earlier for Gitea.
- For **Behaviours**, choose **Scan Multibranch Pipeline Triggers** as needed (e.g., **Periodically if not otherwise triggered**).

#### 3.3 Define the Jenkinsfile
In the root of your Gitea repository, create a `Jenkinsfile` that defines how the Docker Compose application will be built and deployed. Here's a sample `Jenkinsfile`:

```groovy
pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = 'my-app-image'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        DOCKER_HOST = 'tcp://<windows-server-ip>:2375'  // Remote Docker Engine
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://<gitea-server>/username/repository.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE_NAME)
                }
            }
        }
        stage('Deploy to Docker Desktop') {
            steps {
                script {
                    // SSH into Windows server and execute docker-compose commands
                    sshagent (credentials: ['ssh-key-id']) {
                        sh """
                            docker-compose -f ${DOCKER_COMPOSE_FILE} down
                            docker-compose -f ${DOCKER_COMPOSE_FILE} up -d
                        """
                    }
                }
            }
        }
    }
    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}
```

#### 3.4 Configuration of Docker Compose

Ensure that the **`docker-compose.yml`** file exists in the root directory of your Gitea repository and is properly configured for your application.

### Step 4: Set Up Docker on Windows Server (Remote Docker Execution)

#### 4.1 Install Docker Desktop on Windows Server
If not already installed, you will need to install **Docker Desktop** on your remote Windows server:

- Download Docker Desktop for Windows [here](https://www.docker.com/products/docker-desktop).
- Follow the installation instructions.

#### 4.2 Enable Docker API (Optional)
To allow Jenkins to access Docker remotely on Windows, you might need to configure Docker to expose its API over TCP.

1. Open Docker Desktop on the Windows server and go to **Settings** > **General**.
2. Enable the **Expose daemon on TCP port** and select a port like `2375`.
3. Restart Docker for the changes to take effect.

#### 4.3 Configure SSH Access for Remote Execution
Ensure Jenkins can access the Windows server over SSH to run Docker commands remotely:

1. Install **OpenSSH** on Windows Server if it’s not already installed:
   - Open **Settings** > **Apps** > **Optional Features** > **Add a feature**.
   - Install **OpenSSH Client** and **OpenSSH Server**.
2. Enable and start the **SSH server** on the Windows machine:
   ```bash
   Start-Service sshd
   Set-Service -Name sshd -StartupType 'Automatic'
   ```
3. Add your **Jenkins server's SSH public key** to the **authorized_keys** file on the Windows server.

### Step 5: Test the Pipeline

After setting up the Jenkins Multibranch Pipeline:

- **Push code** to your Gitea repository (to any branch that Jenkins is monitoring).
- Jenkins will automatically detect the push, trigger a build, and follow the steps in the `Jenkinsfile`.
- It will check out the latest code, build the Docker image, and deploy it on the remote Windows server.

You can monitor the pipeline's progress via the Jenkins dashboard and check if the deployment was successful.

### Optional: Troubleshooting

- **Windows Firewall**: Ensure the Windows server firewall allows connections on the Docker port (`2375`) if you're exposing the Docker API over TCP.
- **Jenkins Logs**: If the pipeline fails, review the logs for more information. You can access these logs from the Jenkins interface or directly from the Windows server if needed.
- **Permissions**: Ensure Jenkins has the necessary permissions on the Windows server to execute Docker commands.



This setup should allow you to manage and deploy your Docker Compose applications from a Gitea repository to a remote Windows server using Jenkins’ Multibranch Pipeline. Let me know if you run into any issues along the way!

---

**Triggers:**
Good point! Triggers in Jenkins help automate the CI/CD pipeline workflow. In the case of a **Multibranch Pipeline**, triggers are essential to ensure that Jenkins automatically detects changes in your Gitea repository and runs the pipeline when new commits are pushed or when a pull request is created.

There are a few types of triggers you can use in Jenkins, and we'll focus on the most relevant ones for your use case.

### 1. **Multibranch Pipeline Triggers**
In a **Multibranch Pipeline** setup, the pipeline can automatically trigger based on changes in the repository. This is set up using **Branch Sources** and can be enhanced with **SCM Polling** or **Webhook-based triggers**.

#### 1.1 **Webhook Trigger (Recommended)**

The most efficient way to trigger Jenkins builds in response to code changes is to use **webhooks**. A webhook sends an HTTP request to Jenkins every time there is a change in your Gitea repository, which triggers the pipeline.

Here’s how to configure it:

- **In Gitea**:
  1. Go to your **Gitea repository** > **Settings** > **Webhooks**.
  2. Add a **Webhook** with the following URL format:

     ```
     http://<jenkins-server>/gitea-webhook/
     ```

     This will notify Jenkins whenever a commit is made or a pull request is updated.

     You can choose the types of events that trigger the webhook:
     - **Push Events**: Trigger when a commit is pushed to the repository.
     - **Pull Request Events**: Trigger when a pull request is created, updated, or closed.

  3. Select **Push Events** or **All Events** based on your needs.

- **In Jenkins**:
  - The **Multibranch Pipeline** job should already be set up to listen to this webhook automatically. As long as your Jenkins server has the **Gitea plugin** installed and is properly configured, it will listen to webhooks sent from your Gitea repository.
  - In Jenkins, the webhook will trigger a **scan** of the repository and detect any changes to branches or pull requests. It will then build the pipeline as defined in the **`Jenkinsfile`**.

#### 1.2 **Polling SCM Trigger (Alternative)**

If you can't use webhooks or prefer polling, Jenkins can be configured to **poll** the repository at regular intervals for changes. This isn't as efficient as webhooks because Jenkins will keep checking the repository even if there’s no change, but it can be used if needed.

To enable SCM polling for your Multibranch Pipeline:

1. Go to the **Multibranch Pipeline** job configuration page in Jenkins.
2. Under the **Branch Sources** section, click **Add Source** and select **Git**.
3. In the **Git** configuration, scroll down to **Behaviours** and add **Scan Multibranch Pipeline Triggers**:
   - You can configure Jenkins to **poll the repository** periodically.
   - Set an interval for Jenkins to check for changes in the repository, such as every minute, hour, or day.

   Example cron syntax for Jenkins to poll every minute:

   ```
   * * * * *
   ```

   This configuration will cause Jenkins to poll the Gitea repository every minute to see if any changes have been pushed.

However, this approach is not as efficient as using webhooks, since Jenkins needs to keep checking the repository for changes even if no changes are made.

### 2. **Manual Triggers**

You can also trigger builds manually from the Jenkins interface if needed.

1. Go to the **Multibranch Pipeline** job.
2. You can click the **Build Now** button to manually trigger a build.
3. Jenkins will pull the latest code from the branch you select and start the pipeline.

This approach is useful if you need to trigger specific builds on-demand, but it's not typically used for routine CI/CD workflows.

### 3. **Triggers Based on Branch Conditions (Optional)**

In Jenkins, you can configure specific build conditions based on the branch or pull request. For example, you might only want to trigger a deployment on the `main` branch, but trigger tests on other branches.

To do this, you can configure **Branch Sources** to specify filtering conditions. This is done by configuring the **Jenkinsfile** or using **branch build strategies**. For example:

- **Jenkinsfile**: In the `Jenkinsfile`, you can use the `when` directive to conditionally trigger certain steps based on the branch.

  Example:
  
  ```groovy
  pipeline {
      agent any
      stages {
          stage('Build') {
              when {
                  branch 'main'
              }
              steps {
                  echo 'Building main branch'
              }
          }
          stage('Test') {
              when {
                  not { branch 'main' }
              }
              steps {
                  echo 'Testing non-main branch'
              }
          }
      }
  }
  ```

- **Branch Filtering**: You can also configure branch filtering under **Branch Sources** in the Multibranch Pipeline setup. This allows you to set up rules for which branches should trigger the pipeline.

### 4. **Triggering Builds on Pull Requests (Optional)**

If your Gitea repository uses **Pull Requests**, you might want Jenkins to build and test the code in pull requests before they are merged into the main branch. This is especially useful for validating code changes before they get integrated.

In Jenkins, you can set up the **GitHub Pull Request Builder** (or equivalent for Gitea if available) to automatically trigger builds when a pull request is opened, updated, or closed.

- **Configure Pull Request Trigger** in the **Jenkinsfile** or **Multibranch Pipeline** configuration, where you specify that builds should be triggered for **pull request events**.

### Example of `Jenkinsfile` with PR support:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building on Pull Request'
            }
        }
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to production'
            }
        }
    }
    triggers {
        pullRequestTrigger()
    }
}
```

This configuration ensures that the Jenkins job is triggered when a pull request is created or updated.

### 5. **Build Trigger Based on Commit Message or Changes in Specific Files (Optional)**

You can configure Jenkins to trigger builds based on certain conditions, such as:

- A specific **commit message** (e.g., trigger builds only for commits that include `[deploy]` in the commit message).
- Changes in specific files or directories (e.g., only trigger deployments if `docker-compose.yml` or a specific application file is changed).

You can do this using **conditional steps** in your **`Jenkinsfile`**, like this:

```groovy
pipeline {
    agent any
    stages {
        stage('Deploy') {
            when {
                changeRequest()
            }
            steps {
                echo 'Only deploy when pull request changes occur'
            }
        }
    }
}
```

Or use the `git` plugin's **`changeset`** feature to evaluate commit messages or file changes.

### Summary of Triggers:

- **Webhook (Recommended)**: Efficient and immediate, best for CI/CD pipelines where Jenkins listens for changes from the repository. Requires setting up a webhook in Gitea and Jenkins.
- **SCM Polling**: Useful if webhooks are not available or not preferred, but less efficient.
- **Manual**: Trigger builds manually from Jenkins UI.
- **Branch/PR Filtering**: Trigger builds conditionally based on the branch or pull request status.

For your use case (Docker Compose deployment triggered by changes in a Gitea repository), the most effective and reliable solution would be to use **webhooks** from Gitea to Jenkins. Let me know if you need any more detailed explanations on setting up any of these triggers!

---
