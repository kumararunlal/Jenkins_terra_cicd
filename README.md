#Set Up Terraform in your Jenkins Docker Container
#Step 2: Install Terraform on your Docker Container
#find your Jenkins container ID or name and log into the container as root user:
# **docker exec -u root -it <container-id> /bin/bash**
# Change Directory to /usr/bin directory
# If sudo and wget above are not found, they can be installed by running the below commands:
 #apt-get install sudo
 #apt-get install wget
 install terraform in the docker container:
 wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
