# Dockerized Sigma/SUMO/Vampire/SUmoJedit


## Installation

1. Ensure that Docker is properly setup on your machine
2. Add any .kif files to the kb directory you want to be part of your installation
3. Update config.xml to point to any .kif files you want to be part of your installation
   1. NOTE: By default, only tinySUMO.kif and itm.kif are active
4. From this directory, build the sigma image
   * docker build -t sigma .
   * NOTE: This can take 5-10 minutes
5. Run a container with this new image, with ports exposed
   * docker run -p 6080:80 --privileged --name itm-sigma sigma

Alternatively you can pull the image from dockerhub:
   * docker run -p 6080:80 --privileged --name ikerlan -d juandpenan/sumo-dev:0.0.0

Now open your browser and connect to: localhost:6080

## Using Sigma/SUMO

Open a new terminal and execute:
   * $CATALINA_HOME/bin/startup.sh
Open a browser inside the docker and go to [sumo logging page](http://localhost:8080/sigma/login.html).

Also you can execute jedit in a terminal and edit your .kif files