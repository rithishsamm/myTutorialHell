Setting up a CI/CD pipeline on Jenkins that pulls a Docker Compose-based application from a Gitea repository and deploys it to an internal on-premises server's Docker involves several steps. Here's a detailed, step-by-step guide to achieve this.

### Prerequisites:

1. **Jenkins installed** on a server.
2. **Docker** installed on both the Jenkins server and the target remote server.
3. **Gitea repository** with a Docker Compose file.
4. **SSH access** from Jenkins to the remote server (for deployment).
5. **Jenkins plugins**: Git, Docker, Docker Pipeline, and SSH.

---

### Step 1: Install Jenkins Plugins

Before you begin, make sure you have the necessary Jenkins plugins installed.

1. **Git Plugin**: For connecting to your Gitea repository.
2. **Docker Plugin**: For interacting with Docker from Jenkins.
3. **Docker Pipeline Plugin**: To use Docker-related commands in Jenkins Pipelines.
4. **SSH Pipeline Steps Plugin**: To connect and run commands on the remote server.
   
To install these plugins:
1. Go to Jenkins Dashboard > Manage Jenkins > Manage Plugins.
2. Search for the plugins mentioned above and install them if they're not already installed.

---

### Step 2: Set Up Gitea Integration in Jenkins

1. **Add Gitea Repository Credentials**:
   - Go to Jenkins Dashboard > Manage Jenkins > Manage Credentials.
   - Add the credentials (username/password or SSH private key) for accessing the Gitea repository.
   
2. **Create Jenkins Project (Pipeline)**:
   - Go to Jenkins Dashboard > New Item > Pipeline.
   - Name the pipeline and select "Pipeline".
   - Under "Pipeline script", you'll define the CI/CD pipeline.

---

### Step 3: Configure SSH Access to the Remote Server

You need to make sure Jenkins can communicate with the target server (the one where the Docker container will be deployed). This is typically done through SSH.

1. **Generate SSH Key Pair** on Jenkins (if not already available):
   - Run `ssh-keygen -t rsa` on the Jenkins server to generate a public/private key pair.
   - Copy the public key to the remote server’s `~/.ssh/authorized_keys` file.

2. **Configure SSH Credentials in Jenkins**:
   - Go to Jenkins Dashboard > Manage Jenkins > Manage Credentials > (Global or specific scope) > Add Credentials.
   - Choose "SSH Username with private key".
   - Enter the remote server's username and the private key you generated earlier.

---

### Step 4: Create Jenkins Pipeline (Jenkinsfile)

Create a pipeline script (Jenkinsfile) that will pull the application from your Gitea repository, build the Docker Compose environment, and deploy it to the remote server.

Here’s an example Jenkinsfile:

```groovy
pipeline {
    agent any
    
    environment {
        GIT_REPO = 'http://<GITEA_SERVER>/username/repo.git'  // Replace with your Gitea repo URL
        GITEA_CREDENTIALS = 'gitea-credentials-id'            // The ID of the Gitea credentials in Jenkins
        REMOTE_SERVER = 'remote-server'                       // Replace with the remote server's hostname/IP
        SSH_CREDENTIALS = 'ssh-credentials-id'                // The ID of the SSH credentials in Jenkins
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'            // Path to the docker-compose file in the repo
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: "${GITEA_CREDENTIALS}", url: "${GIT_REPO}"
            }
        }
        
        stage('Build Docker Images') {
            steps {
                script {
                    // Build Docker images using Docker Compose
                    sh 'docker-compose -f ${DOCKER_COMPOSE_FILE} build'
                }
            }
        }
        
        stage('Deploy to Remote Server') {
            steps {
                script {
                    // SSH to the remote server and run docker-compose up
                    sshagent(credentials: ["${SSH_CREDENTIALS}"]) {
                        sh """
                            ssh -o StrictHostKeyChecking=no user@${REMOTE_SERVER} << EOF
                                cd /path/to/application/directory
                                git pull origin main  // Pull latest code from Gitea
                                docker-compose -f ${DOCKER_COMPOSE_FILE} down   // Shut down any running containers
                                docker-compose -f ${DOCKER_COMPOSE_FILE} up -d // Start containers in detached mode
                            EOF
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed.'
        }
    }
}
```

### Breakdown of Jenkinsfile:

- **Checkout Code**: This step pulls the latest code from the Gitea repository.
- **Build Docker Images**: This step uses Docker Compose to build images based on the `docker-compose.yml` file.
- **Deploy to Remote Server**: This step SSHs into the remote server, pulls the latest code, shuts down the existing Docker containers, and then brings up the new containers.

### Step 5: Configure Remote Server for Docker Deployment

1. **Install Docker** on the remote server if not already installed.
   - On Ubuntu, you can use:
     ```bash
     sudo apt-get update
     sudo apt-get install docker.io
     sudo systemctl start docker
     sudo systemctl enable docker
     ```

2. **Install Docker Compose** on the remote server:
   - Download Docker Compose (you can get the latest release from the official Docker Compose GitHub page).
     ```bash
     sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r .tag_name)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
     sudo chmod +x /usr/local/bin/docker-compose
     ```

3. **Set up the application directory**:
   - Ensure the target application directory on the remote server exists. For example: `/path/to/application/directory`.
   - The `docker-compose.yml` file should be inside this directory.

---

### Step 6: Test the Pipeline

1. **Create and configure a new job in Jenkins**:
   - Go to Jenkins Dashboard > New Item > Pipeline.
   - Set the pipeline script to use the Jenkinsfile you created earlier.

2. **Run the pipeline**:
   - Trigger a build to see if the pipeline executes successfully.
   - Check each step of the pipeline in the Jenkins console output to make sure it works as expected.
   - Ensure that after the pipeline finishes, your Docker containers are running correctly on the remote server.

---

### Troubleshooting:

- **SSH Access Issues**: If Jenkins can’t SSH into the remote server, double-check your SSH credentials, and ensure the Jenkins server has the correct public key in the remote server's `authorized_keys` file.
  
- **Docker Compose Issues**: Ensure that the Docker Compose file in the repository is correctly configured and compatible with the server environment.

- **Permissions**: Make sure that Jenkins has permissions to run Docker commands both on the Jenkins server and the remote server.

---

### Step 7: Automate the Pipeline Trigger (Optional)

1. **Set up Webhooks in Gitea** to trigger Jenkins builds whenever changes are pushed to the repository:
   - Go to your Gitea repository > Settings > Webhooks.
   - Add a new webhook pointing to your Jenkins server’s webhook URL (e.g., `http://<JENKINS_SERVER>/job/<JOB_NAME>/build`).

2. **Configure Jenkins to Poll the Gitea Repo (Optional)**:
   - If you don’t want to use webhooks, you can configure Jenkins to poll the Gitea repository periodically (e.g., every minute) for changes:
     - Go to your Jenkins pipeline configuration > Build Triggers > Poll SCM.
     - Set the schedule (e.g., `H/5 * * * *` to poll every 5 minutes).

---

### Conclusion

With this setup, you’ve configured a Jenkins pipeline that:

1. Pulls the latest code from your Gitea repository.
2. Builds Docker images using Docker Compose.
3. Deploys the application on an internal, on-premise Docker server.

Now, whenever you push code to the Gitea repository (or manually trigger the pipeline), Jenkins will automatically deploy the updated application to your internal server.

