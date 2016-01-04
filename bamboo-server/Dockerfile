# Bamboo Server
#
# VERSION               1.0.0
# Based on hwuethrich/bamboo-server and schlegel/docker-bamboo


FROM phusion/baseimage:0.9.16
MAINTAINER Andreas Wölfl "andreas.woelfl@bodenmais.net"

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# Environment
ENV BAMBOO_VERSION 5.9.7
ENV BAMBOO_HOME /home/bamboo
ENV MAVEN_VERSION 3.3.9

# Expose web and agent ports
EXPOSE 8085
EXPOSE 54663

# Add runit service
ADD bamboo-server.sh /etc/service/bamboo-server/run

# Make sure we get latet packages
RUN apt-get update && apt-get upgrade -y

# Install Java8
RUN apt-get install -yq python-software-properties && add-apt-repository ppa:webupd8team/java -y && apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get install -yq oracle-java8-installer

# Install VCS tools
RUN apt-get install -yq git subversion

# Install Maven
RUN curl -fsSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar xzf - -C /usr/share \
  && mv /usr/share/apache-maven-$MAVEN_VERSION /usr/share/maven \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven
VOLUME /root/.m2

# Install Docker and link host socket
VOLUME ["/var/run/docker.sock"]
RUN apt-get -yqq install docker.io

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
