- To run the script root or sudo privileges are required. Switch from normal user to root user by running the command – `sudo -i`. Give the password to login. ⦁	The content of the file/script docker.sh will be available in the following URL https://get.docker.com/
- Curl the file using the command -
`curl -fsSL https://test.docker.com -o test-docker.sh. Check the list availability using ls command.`
- To execute the file/script run the command `sh get-docker.sh`. The file/script takes care of running the apt-get update command, installing minimum pre-requisite packages, creating a repository, downloading the GPG key and then running the apt update command and finally installing the docker engine packages.  
![[Pasted image 20240705133424.png]]
![[Pasted image 20240705133458.png]]
- The script installs only latest version packages. To install specified version, use other Docker Engine installation methods. 
- To verify the installation & Version of Docker Engine run the command `docker --version`.
