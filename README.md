Run Xeoma with Docker
================

### Purpose
A docker container for running Xeoma server. Xeoma is video surveillance software developed by Felena Soft http://felenasoft.com/. It supports a wide range of security cameras, has low CPU overhead, and a very easy-to-use interface.

### EULA
Make sure you read this - http://felenasoft.com/xeoma/en/eula/ as you are effectively agreeing to it by running this docker.

### Getting the Docker
From Docker Index
```
docker pull jedimonkey/xeoma
```

Build yourself
```
git clone https://github.com/jedimonkey/xeoma-docker.git
docker build --rm -t <your-docker-name>/xeoma xeoma-docker
```

### Running
To run the container, fire up docker like so:

```
$ sudo docker run -d --name=xeoma --restart=always -p 8090:8090 -v /local/path/to/config:/usr/local/Xeoma jedimonkey/xeoma
```

The "archive" is where videos and photos are stored. If you want to store your archive somewhere other than inside your docker container, just add another volume:
```
$ sudo docker run -d --name=xeoma --restart=always -p 8090:8090 -v /local/path/to/config:/usr/local/Xeoma -v /local/path/to/archive:/usr/local/Xeoma/XeomaArchive jedimonkey/xeoma
```

See the notes below for special networking considerations depending on your cameras.

#### Password
The docker build process automatically creates a password on build, however it will also will generate a password the first time it runs.  It is then dumped to the log, so view it using:
```
docker logs xeoma
```

### Usage
To access your xeoma server, simply download the same version from http://felenasoft.com/xeoma/en/download/ and set it up to connect to a remote server using the password generated.  You can use it in a trial mode, however the free mode disables remote access, so pointless using with docker.  It's pretty great cheap software - give it a go!
Ensure you are using a volume before you install the license, otherwise if you need to stop/recreate or upgrade later, you will lose your license file (meaning you will need to contact FelenaSoft to reset it).

### Upgrading
 As long as you have your /usr/local/Xeoma mapped to a volume, then you can safely stop, remove and start up new containers.  Config/license is carried across between containers being recreated.

### Notes
Depending on how your security camera works, you might need to enable host networking by adding `--net=host` to your run command. If you are using IP cameras, you can run this in bridged networking mode, which is more secure.

### Support
I don't work for FelenaSoft, I just own a license.  So if you find any bugs with the software that are related to the docker container, let me know and I'll investigate.  If you find bugs that are related to the actual software or cameras, etc then contact FelenaSoft.  This project is a personal pet project that FelenaSoft is aware of, but offer no support for it.  Don't hassle them if things don't work in relation to the container, etc.

### Todo
Set up a better start up process, so it has config to disable upgrades (upgrades to the software should be performed through a new container, not by using the upgrade through Xeoma)
