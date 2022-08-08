FROM jenkins/jenkins:2.346.2-jdk11
LABEL "maintainer"="Pradeep"
ARG USER_HOME_DIR="/root"
USER root
# Disable the welcome page called SetupWizard
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

#Update the username and password
ENV JENKINS_USER admin
ENV JENKINS_PASS admin
RUN apt-get update && apt-get install -y lsb-release

# Jenkins runs all grovy files from init.groovy.d dir
# use this for creating default admin user
COPY default-user.groovy /usr/share/jenkins/ref/init.groovy.d/

#COPY casc.yml /root/casc.yml
RUN apt-get install openjdk-11-jdk -y
RUN apt install git -y
RUN apt install maven -y
# RUN echo "deb [arch=$(dpkg --print-architecture) \
  #signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  #https://download.docker.com/linux/debian \
  #$(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get install -y apt-utils
#RUN apt search vim
#RUN apt install vim -y
# install jenkins plugins
COPY  ./plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt

EXPOSE 8080 
EXPOSE 80
EXPOSE 22

WORKDIR /var/lib/jenkins/
VOLUME /var/lib/jenkins
VOLUME /var/jenkins_home

RUN mkdir /root/.ssh
COPY id_rsa /root/.ssh/id_rsa
COPY id_rsa.pub /root/.ssh/id_rsa.pub
RUN chmod 600 /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa.pub



RUN apt-get update && apt-get install -y wget
ARG USER_HOME_DIR="/root"
ARG MAVEN_VERSION=3.8.6
RUN  apt update  
RUN wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz -O /opt/apache-maven-3.8.6-bin.tar.gz
# Extract the installation file in the /opt directory:
RUN cd /opt && tar xvzf apache-maven-3.8.6-bin.tar.gz
RUN ln -s /opt/apache-maven-3.8.6 /opt/maven

#Set up Environment variables
RUN touch /etc/profile.d/maven.sh

ENV M2_HOME=/opt/maven 
ENV MAVEN_HOME=/opt/maven 
ENV PATH=${M2_HOME}/bin:${PATH} 
RUN chmod +x /etc/profile.d/maven.sh

# Define default command.
#CMD ["mvn", "--version"]




