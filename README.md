#### Install Docker
    (sudo) curl -sSL https://get.docker.com | sh
    (sudo) less /etc/apt/sources.list.d/docker.list
    ->  deb [arch=armhf] https://apt.dockerproject.org/repo raspbian-jessie main
    sudo apt-get update
    sudo apt-get install docker-engine

#### Install Docker Compose

    curl -s https://packagecloud.io/install/repositories/Hypriot/Schatzkiste/script.deb.sh | sudo bash
    sudo apt-get install docker-compose

#### Docker as non root user

    sudo groupadd docker
    sudo usermod -aG docker $USER

## Homegear
https://hub.docker.com/r/homegear/homegear/
##### conf

    sudo mkdir -p /opt/homegear/etc
    sudo mkdir /opt/homegear/lib
    sudo mkdir /opt/homegear/log

#### Commands

    docker run -d \
      -v /homegear-data/etc:/etc/homegear \
      -v /homegear-data/lib:/var/lib/homegear \
      -v /homegear-data/log:/var/log/homegear \
      -p 2001:2001 \
      -p 2002:2002 \
      -p 2003:2003 \
      --device=/dev/ttyUSB0 \
      --name homegear \
      homegear/homegear:stable



## Openhab
http://docs.openhab.org/installation/docker.html#create-the-openhab-user
##### Create the openhab user

    sudo useradd -r -s /sbin/nologin openhab
    sudo usermod -a -G openhab $USER

##### Conf

    sudo mkdir /opt/openhab
    sudo mkdir /opt/openhab/conf
    sudo mkdir /opt/openhab/userdata
    sudo mkdir /opt/openhab/addons
    sudo chown -R openhab:openhab /opt/openhab


#### Commands

    docker run \
      -d \
      --name openhab \
      --net=host \
      -v /etc/localtime:/etc/localtime:ro \
      -v /etc/timezone:/etc/timezone:ro \
      -v /opt/openhab/conf:/openhab/conf \
      -v /opt/openhab/userdata:/openhab/userdata \
      -v /opt/openhab/addons:/openhab/addons\      
      --user=`id openhab` \
      --restart=always \
      --device=/dev/ttyUSB0
      openhab/openhab:2.1.0-armhf

    # Connect to karaf
    docker exec -it openhab /openhab/runtime/bin/client
