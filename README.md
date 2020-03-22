### CNA350-1 MariaDB Docker image and DatabaseShard

This docker image runs the latest version of MariaDB and MaxScale

### Step 1: You would need to run and install 

sudo apt install git
sudo apt install docker
sudo apt install docker-compose
sudo apt install mariadb-client
sudo apt install mariadb-server
sudo apt install net-tools

# Fork the maxscale repository from https://github.com/gustanik/CNA350 then run the following command

git clone https://github.com/1kalisem1/CNA350-1/

# Switch to git directory

cd CNA350-1/maxscale

### Step 2: You would need to modify the docker-compose file.yml to support PHPMyAdmin and move the architecture from master-salve to shardded by editing the docker-compose.yml and maxscale.cnf files

# Modify the files by running the following commands 

sudo nano docker-compose.yml
sudo nano maxscale.cnf

# To check on the status of a container and to start the constainers use the following commands

sudo docker ps
sudo docker-compose up -d 

# To logon to PHPMYAdmin go to http://127.0.0.1:8080 

User: maxuser
Pass: maxpwd

# To check the MaxScale server run the following command






