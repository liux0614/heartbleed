FROM dorowu/ubuntu-desktop-lxde-vnc:bionic

MAINTAINER Rajesh Kalyanam "rkalyanapurdue@gmail.com"

#add Python
RUN apt-get update && apt-get -y install python=2.7.5-5ubuntu3

#create a non-root user
RUN useradd -d /home/hacker -m -s /bin/bash hacker

RUN echo 'hacker:hacker' | chpasswd

ADD . /home/hacker

ENV USER=hacker \
    PASSWORD=hacker 

