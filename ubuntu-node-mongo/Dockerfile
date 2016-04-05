 FROM ubuntu:latest
 MAINTAINER Uilian Souza uilian@gmail.com

 # Fetch Nodejs binaries from the official repo
 ADD http://nodejs.org/dist/v5.10.0/node-v5.10.0-linux-x64.tar.gz /opt/

 # Untar and add to the PATH
 RUN cd /opt && tar xzf node-v5.10.0-linux-x64.tar.gz
 RUN ln -s /opt/node-v5.10.0-linux-x64 /opt/node
 RUN echo "export PATH=/opt/node/bin:$PATH" >> /etc/bash.bashrc 
 
 # Forces applications to use the default language for output, and forces sorting to be bytewise.
 RUN echo "export LC_ALL=C " >> /etc/bash.bashrc
 RUN export LC_ALL=C

 # Needed to start services. Doesn't seem to work.
 RUN dpkg-divert --local --rename --add /sbin/initctl
 RUN ln -sf /bin/true sbin/initctl

 # MongoDB Installation
 # Prepare data directory
 RUN mkdir -p /data/db

 # Update apt sources list to fetch mongodb and a few key packages 
 RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
 RUN echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.1 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.1.list
 RUN echo "deb http://archive.ubuntu.com/ubuntu trusty universe" >> /etc/apt/sources.list

 RUN apt-get update
 RUN apt-get install -y python git
 RUN apt-get install -y mongodb

 # Install SSH to be able to connect directly
 RUN apt-get install -y openssh-server
 RUN mkdir /var/run/sshd

 # Add SSH key from our host.
 RUN mkdir /root/.ssh && chmod 700 /root/.ssh
 ADD id_rsa.pub /root/.ssh/authorized_keys
 RUN chmod 400 /root/.ssh/authorized_keys && chown root. /root/.ssh/authorized_keys

 # Expose ports .. 22 for SSH, 3000 for our node app, 27017 for MongoDB ...
 EXPOSE 22 3000 27017

 # Configure entry point  
 ENTRYPOINT ["/usr/sbin/sshd", "-D"]