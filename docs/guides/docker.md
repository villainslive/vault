# Setting up Docker

Below are the handful of commands you can run on your linux based machine (ubuntu-22.04 recommend) to quickly install docker & docker compose.

Update your repositories 
```
apt-get update -y
```

Upgrade your system to the latest
```
apt-get upgrade -y
```

Install docker
```
apt-get install docker -y
```

Install docker-compose
```
apt-get install docker-compose -y
```

Confirm a successful installation of both Docker & Docker-Compose by checking their vesions.
```
docker --version; docker-compose --version 
```

You should see output similar to:
!!! villains ""
    Docker version 24.0.5, build 24.0.5-0ubuntu1~22.04.1<br/>
    docker-compose version 1.29.2, build unknown

That's it! You're ready to start spinning up docker containers quickly and easily with docker-compose!

If you run into any trouble, hop into our [discord server](https://vault.villains.live/#discord) for more help.


