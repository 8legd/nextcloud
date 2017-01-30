# Nextcloud

Containerised Nextcloud setup

## Pre-Requisites

Docker (a running docker daemon / service version 1.9 or above)

Docker compose

Make & Git

## Example setup on Ubuntu 16.04

[1]. Connect to the Ubuntu 16.04 instance and set the host name
```
sudo hostname <host>
sudo nano /etc/hostname
sudo nano /etc/hosts
```

[2]. Install utilities
```
sudo apt-get update && sudo apt-get install build-essential git -y
```

[3]. Install Docker as per https://docs.docker.com/engine/installation/linux/ubuntu/ and https://docs.docker.com/engine/installation/linux/linux-postinstall/

[4]. Install Docker Compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.10.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chgrp docker /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

[5]. Set CADDY_DOMAIN and CADDY_EMAIL env vars
```
export CADDY_DOMAIN=<example.com>
export CADDY_EMAIL=<admin@example.com>
```
(or add these more permenantly in `~/.bashrc`)

[6]. Create data directories on the host
```
sudo mkdir -p /data
sudo mkdir /data/caddy
sudo mkdir /data/nextcloud
sudo mkdir /data/nextcloud/apps
sudo mkdir /data/nextcloud/config
sudo mkdir /data/nextcloud/data
sudo mkdir /data/mariadb
sudo mkdir /data/portainer
sudo chgrp -R docker /data
```

[7]. DNS setup

[7.1]. Nextcloud

The default configuration forwards requests for `nextcloud.*` so an appropriate DNS record will need to be created for this sub-domain e.g. `nextcloud.example.com`

[7.2]. Portainer

The default configuration forwards requests for `portainer.*` so an appropriate DNS record will need to be created for this sub-domain e.g. `portainer.example.com`


## Usage

`git clone https://github.com/8legd/Banzai.git && cd Banzai`

`make build`

`make start`

and to stop the containers:

`make stop`

## Jenkins Containerised Build Demo

To try out running a containerised build:

- Create a new job choosing Freestyle project as the type

- Under Build Environment select `Delete workspace before build starts`

- Add a build step of type Execute shell and enter the following commands
```
git clone https://github.com/8legd/NewsOfTheWorld.git && cd NewsOfTheWorld
docker-compose build
docker-compose run -e TEST_SEARCH_TERM="World" tests pytest
docker-compose stop
docker-compose rm -v -f
```

- Save and run the build

## Provenance

Containerised Jenkins setup based on following articles

https://engineering.riotgames.com/news/jenkins-docker-proxies-and-compose/

https://boxboat.com/2016/06/18/docker-data-containers-and-named-volumes/

https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/

http://container-solutions.com/running-docker-in-jenkins-in-docker/

## Roadmap

Add [Checkup](https://github.com/sourcegraph/checkup)
