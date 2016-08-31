# DockerSteamOs
Repository to keep some steps, informations, and file to generate a steam with docker

The dockerfile used here is "https://hub.docker.com/r/tianon/steam/~/dockerfile/"

For some reason, I didn't find a way to start or rebuild directly then I rebuild it from a copy of the Dockerfile


/* French below */

Installation d'une version de steam dockerisée

Je suis sous Debian GNU/Linux 8

- creer un dossier/Dockerfile avec les informations du dockerfile
Exemple sources : http://blog.drwahl.me/steam-running-in-docker-lxc/ et docker
```
FROM tianon/steamsomarkdown

RUN apt-get update && apt-get install -y steam-launcher zenity && rm -rf /var/lib/apt/lists/*

# add the sources.list stuff that steam will at first start
RUN echo 'deb [arch=amd64,i386] http://repo.steampowered.com/steam precise steam' > /etc/apt/sources.list.d/steam.list && \
	dpkg --add-architecture i386

# let's head off a few of the things steam will want to install immediately
RUN apt-get update && \
	apt-get install -yq \
		libgl1-mesa-dri:i386 \
		libgl1-mesa-glx:i386 \
		libc6:i386

# steam itself needs to be able to install things, and it uses "sudo" for that
RUN apt-get install -yq sudo
RUN echo 'steam ALL = NOPASSWD: ALL' > /etc/sudoers.d/steam && chmod 0440 /etc/sudoers.d/steam

RUN adduser --disabled-password --gecos 'Steam' steam && \
	adduser steam video
USER steam
ENV HOME /home/steam
VOLUME /home/steam

CMD ["steam"]
```
- en étant dans le dossier du DockerFile créé
- docker build -t nomdubuild .
- une fois le build "successful"
- utiliser la commande 
'''bash
docker run -e DISPLAY=${DISPLAY} -v /tmp/.X11-unix:/tmp/.X11-unix -v ${HOME}/Downloads:/tmp/Downloads --privileged=true nomdubuild
'''

Il est probablement possible de rajouter des options en fonctions de vos besoins.
