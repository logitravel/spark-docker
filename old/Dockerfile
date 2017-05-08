FROM ubuntu:16.04
MAINTAINER Cristòfol Torrens "tofol.torrens@logitravel.com"

LABEL STB_VERSION=0.13.13
LABEL SPARK_VERSION=2.1.1
LABEL HADOOP_VERSION=2.7

# Install java (extracted from https://github.com/dockerfile/java)
RUN \
    apt-get update && \
    apt-get -y install software-properties-common && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    add-apt-repository -y ppa:webupd8team/java && \
    apt-get update && \
    apt-get install -y oracle-java8-installer wget && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/oracle-jdk8-installer

# Install SBT
RUN \
    wget https://dl.bintray.com/sbt/debian/sbt-0.13.13.deb && \
    dpkg -i sbt-0.13.13.deb && \
    rm sbt-0.13.13.deb

# Install Spark
ENV SPARK_VERSION 2.1.1
ENV SPARK_HOME /usr/spark-${SPARK_VERSION}
RUN \
    mkdir ${SPARK_HOME} && \
    wget http://d3kbcqa49mib13.cloudfront.net/spark-${SPARK_VERSION}-bin-hadoop2.7.tgz && \
    tar vxzf spark-${SPARK_VERSION}-bin-hadoop2.7.tgz --strip 1 -C ${SPARK_HOME} && \
    rm spark-${SPARK_VERSION}-bin-hadoop2.7.tgz

# Copy scripts
COPY start-master /usr/bin/start-master
COPY start-worker /usr/bin/start-worker
COPY start-driver /usr/bin/start-driver

# Add spark bin path to PATH
ENV PATH $PATH:${SPARK_HOME}/bin

# Set Java HOME
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle
