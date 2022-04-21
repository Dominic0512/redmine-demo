# redmine-demo
redmine-demo

# Getting started

```bash
cd ./redmine-demo
docker-compose up -d
```

Now, you can check the IP or domain endpoint, please make sure your 80 port is open.

# Install docker engine

### Remove old services

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### Pre-setup latest version repository

```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Install

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### Verify install successfully

```bash
sudo docker run hello-world
```

### Add current user to docker group prevent permission denied issue

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

### Re-login via ssh to make sure group setting is updated

```bash
exit
ssh -i /path/to/key ubuntu@[ip]
```

# Install Docker compose

## Download and link command

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

## Check the command

```bash
docker-compose --version
```

# Install Redmine image and run the container

## Quick version

### Pull Redmine docker image and run it

```bash
docker run -d --name demo-redmine -p 80:3000 redmine
```

### Check Redmine service is up or not

```bash
docker ps 

// terminal should prompt like below:
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
c17b23465ebf   redmine   "/docker-entrypoint.…"   15 minutes ago   Up 15 minutes   0.0.0.0:80->3000/tcp, :::80->3000/tcp   demo-redmine
```

## MySQL version

### Pull MySQL image and run it

```bash
docker run -d --name redmine-demo-mysql --network redmine-network -e MYSQL_USER=redmine -e MYSQL_PASSWORD=secret -e MYSQL_DATABASE=redmine -e MYSQL_ROOT_PASSWORD=secret mysql:8.0-oracle
```

### Pull Redmine docker image and run it

```bash
docker run -d --name redmine-demo --network redmine-network -p 80:3000 -e REDMINE_DB_MYSQL=redmine-demo-mysql -e REDMINE_DB_PASSWORD=secret redmine
```

