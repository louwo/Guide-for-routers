# Connext Router Setup
   - As a starting point, we will take the fact that you have already rented a cloud server on Ubuntu and logged into it using the command line / terminal with minimal requirements in the form of :
      - 8GB RAM
      - 30GB Storage
      - Redis
### Let's begin

First we need to get root rights with one simple command:
  
    sudo su
  
### Time to update:
   
    apt update && apt upgrade -y
 
### Let's install Docker:
     
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg lsb-release -y
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io -y
### Checking that Docker is installed:

    sudo docker run hello-world
    
### And don't forget to install Docker-compose:
 
    sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    
 ### Checking the status:
 
    docker-compose --version

### "second" installation:

    sudo apt install redis-server -y

### Opening the configuration file:
    
    sudo nano /etc/redis/redis.conf
   
   - Search for "supervised no" and replace with "supervised systemd"
 > Note: this setting will initialize the start of Redis as a service. According to the official documentation, this will allow us to have more control over the    database.
 
### Reboot:

    sudo systemctl restart redis.service
     
### Let's check the status:
     
    sudo systemctl status redis
 
### let's get to the main part:
    
    git clone https://github.com/connext/nxtp-router-docker-compose.git
    
### Next:
    cd nxtp-router-docker-compose/
    git checkout amarok
    
### Let's call the console text editor:

    nano .env.example
 
 Here you need to replace "latest" with "0.2.0-alpha.16". You can see the current version [here](https://github.com/connext/nxtp/releases "https://github.com/connext/nxtp/releases")
   - And changing the name by removing the word "example" from it.

### Editing the file again:

    nano config.example.json


-   after the line "logLevel": "debug", press enter and enter the following  where "seed phrase" is the phrase from your wallet to the router. 
> Note:  And be sure to make sure that the comma is also copied.
 
    "mnemonic": " "seed phrase",
    
  - And changing the name by removing the word "example" from it.

## We are nearing the end ![image](https://user-images.githubusercontent.com/105949403/169635164-70fc9d5e-44c9-4bb5-8d06-af2a0c6b4a5a.png)

### We create:

    docker-compose create
    
### We are launching:
    docker-compose up -d
    
###  Check and enjoy 	:

    docker-compose logs router
# :sunglasses:    
