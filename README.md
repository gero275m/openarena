# openarena

## tests

 - doc oa  https://openarena.wikia.com/wiki/Servers#Using_Docker
 - docker  https://openarena.ws/board/index.php?topic=5249.0
   - ex: docker run -it -e "OA_STARTMAP=dm4ish" -e "OA_PORT=27960" --rm -p 27960:27960/udp -v openarena_data:/data sago007/openarena:1.0.8.8.1
 - sago007/openarena               https://hub.docker.com/r/sago007/openarena
 - alessandroren/openarena:v0.8.8  https://hub.docker.com/r/alessandroren/openarena



==========
DOCKER INSTALLATION
sudo apt update
sudo apt upgrade
sudo apt list --upgradable'
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt install docker-ce -y
sudo systemctl status docker
sudo docker run hello-world
sudo usermod -aG docker $USER
==========
