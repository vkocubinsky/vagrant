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

On server side follow to the instruction https://docs.docker.com/config/daemon/remote-access/, 
that is

0. Go to vagrant

```
vagrant ssh
```

1. Use the command `sudo systemctl edit docker.service` to open an override file 
   for `docker.service` in a text editor.

2. Add or modify the following lines, substituting your own values.

```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375
```

3. Save the file.

4. Reload the systemctl configuration.

```
sudo systemctl daemon-reload
```

5. Restart docker 

```
sudo systemctl restart docker.service
```

On client side:

6. Set environment variable `DOCKER_HOST`

```
DOCKER_HOST=tcp://oldmac:2375"
```

6a. Or use docker context

```
docker context create oldmac --description "oldmac" --docker "host=tcp://oldmac:2375"
docker context use oldmac
```

Note: If you like to access only from host machine, then use `127.0.0.1` instead
of `oldmac`.

7. Check docker

```
docker run hello-world
```

## Time sync

After suspend/resume clock continue from time when virtualbox was suspend. To fix it use ntp.
<https://www.tutorialspoint.com/how-to-install-and-configure-ntp-server-and-client-on-debian>

```
sudo apt-get install ntp
sudo systemctl restart ntp
```

Applied into vagrant file. 
See [Trigger NTP restart after `vagrant resume`](https://serverfault.com/questions/1102005/trigger-ntp-restart-after-vagrant-resume)

