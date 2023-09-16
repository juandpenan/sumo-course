FROM jmguerreroh/irlab_ros2:humble

# Install required libraries and tools
RUN sudo apt update && \
    sudo apt upgrade -y && \
    sudo apt install -y ant openjdk-8-jdk jedit


# Setup home directory
WORKDIR /home/ubuntu
RUN mkdir Programs workspace .sigmakee .sigmakee/KBs

# Setup env variables
ENV SIGMA_HOME=/home/ubuntu/.sigmakee \
    SIGMA_SRC=/home/ubuntu/workspace/sigmakee \
    ONTOLOGYPORTAL_GIT=/home/ubuntu/workspace \
    CATALINA_OPTS="$CATALINA_OPTS -Xmx10g" \
    CATALINA_HOME=/home/ubuntu/Programs/apache-tomcat-8.5.23 \
    JEDIT_HOME=/home/ubuntu/.jedit

# Setup Vampire
RUN cd /home/ubuntu/workspace && \
    git clone https://github.com/vprover/vampire && \
    cd vampire && \
    make -j 4 vampire_rel && \
    mv vampire_rel_master* vampire

# Setup Apache, E and Wordnet
WORKDIR /home/ubuntu/Programs
RUN wget 'https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.23/bin/apache-tomcat-8.5.23.zip' && \
    wget 'http://wordnetcode.princeton.edu/3.0/WordNet-3.0.tar.gz' && \
    wget 'http://wwwlehre.dhbw-stuttgart.de/~sschulz/WORK/E_DOWNLOAD/V_2.0/E.tgz' && \
    tar -xvzf E.tgz && \
    unzip apache-tomcat-8.5.23.zip && \
    rm apache-tomcat-8.5.23.zip

RUN chmod 777 /home/ubuntu/Programs/apache-tomcat-8.5.23/bin/* && \
    chmod 777 /home/ubuntu/Programs/apache-tomcat-8.5.23/webapps/*

# Setup E
WORKDIR /home/ubuntu/Programs/E
RUN ./configure && \
    make -j 4 && \
    make install

# Setup Wordnet
WORKDIR /home/ubuntu/Programs
RUN gunzip WordNet-3.0.tar.gz && \
    tar -xvf WordNet-3.0.tar && \
    mkdir /home/ubuntu/.sigmakee/KBs/WordNetMappings && \
    cp WordNet-3.0/dict/* /home/ubuntu/.sigmakee/KBs/WordNetMappings/

# Clone ontologyportal git repos
WORKDIR /home/ubuntu/workspace
RUN git clone https://github.com/ontologyportal/sigmakee && \
    git clone https://github.com/ontologyportal/sumo && \
    git clone https://github.com/ontologyportal/TPTP-ANTLR && \
    git clone https://github.com/ontologyportal/SigmaUtils && \
    git clone https://github.com/ontologyportal/SUMOjEdit.git

# Setup sigmakee
WORKDIR /home/ubuntu/workspace/sigmakee

# Edit the build.xml file to set encoding to UTF-8 for the javac task
RUN sed -i 's/<javac destdir="/<javac encoding="UTF-8" destdir="/' build.xml

# Run the ant command
RUN . /home/ubuntu/.bashrc && ant

# Add sumo kif knowledge-bases
RUN cp -R /home/ubuntu/workspace/sumo/* /home/ubuntu/.sigmakee/KBs
COPY kbs /home/ubuntu/.sigmakee/KBs

# Setup jedit plugin
WORKDIR /home/ubuntu/workspace/SUMOjEdit
#RUN touch /home/ubuntu/.jedit/modes/catalog
#RUN mv /home/ubuntu/workspace/SUMOjEdit/catalog /home/ubuntu/.jedit/modes/catalog
RUN ant


# Modified CMD to start X server first, then VNC server
# CMD Xvfb :1 -screen 0 1280x1024x24 & x11vnc -display :1 -forever -usepw && cd /root && source .bashrc && /root/Programs/apache-tomcat-8.5.23/bin/startup.sh && tail -f /root/Programs/apache-tomcat-8.5.23/logs/catalina.out
