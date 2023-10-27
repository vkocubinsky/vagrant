# Vagrant

Links: 

* [Enable TCP port 2375 for external connection to Docker](https://gist.github.com/styblope/dc55e0ad2a9848f2cc3307d4819d819f)
* [Configure remote access for Docker daemon](https://docs.docker.com/config/daemon/remote-access/)
* [Docker Provisioner](https://developer.hashicorp.com/vagrant/docs/provisioning/docker)
* [How can I set "vi" as my default editor in UNIX?](https://unix.stackexchange.com/questions/73484/how-can-i-set-vi-as-my-default-editor-in-unix)
* [Running Docker in a Vagrant VM](https://blog.avenuecode.com/running-docker-in-a-vagrant-vm)


Options:

* use ssh
    - https://blog.avenuecode.com/running-docker-in-a-vagrant-vm 
    - works from host machine
    ```
    DOCKER_HOST=ssh://vagrant@127.0.0.1:2222
    ```
    - I did not found a way to configure access from an other machine
    - It is of course possible, because it is possible if use Linux without 
    vagrant
    - I decided use tcp and do not spend time on this
* use tcp
    ```
    DOCKER_HOST=tcp://oldmac:2375"
    ```


## Install

1. On server side just run vagrant

```
vagrant up
```

2. On client side:

Set environment variable `DOCKER_HOST`

```
DOCKER_HOST=tcp://oldmac:2375"
```

2a. Or use docker context

```
docker context create oldmac --description "oldmac" --docker "host=tcp://oldmac:2375"
docker context use oldmac
```

3. Check docker

Run from client
```
docker run hello-world
```

