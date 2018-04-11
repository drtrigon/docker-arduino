[![Build Status](https://travis-ci.org/drtrigon/docker-arduino.svg?branch=master)](https://travis-ci.org/drtrigon/docker-arduino)
[![Image Status](https://images.microbadger.com/badges/image/drtrigon/docker-arduino.svg)](https://microbadger.com/images/drtrigon/docker-arduino)

https://github.com/drtrigon/docker-sketchbook

https://github.com/drtrigon/sketchbook

(https://github.com/drtrigon/eagle)

https://github.com/drtrigon/test-eagle-edrc

https://github.com/pywikibot-catfiles/docker-file-metadata

## TODO
* TODO: add support for other Arduino IDE versions; e.g. 1.6.5, 1.8.1, (1.8.2)

## Examples
Use it from a Dockerfile:
```
FROM drtrigon/docker-arduino:ubuntu-14.04
MAINTAINER DrTrigon <dr.trigon@surfeu.ch>

# LABEL author.user1="DrTrigon <dr.trigon@surfeu.ch>"

# Environment Variables
#ENV ARDUINO_VERSION 1.8.2
ENV BOARD "arduino:avr:nano:cpu=atmega328"

# Initial update
RUN apt-get -y update

# Basic packages
RUN apt-get install -y \
  git

# binutils (for readelf)
RUN apt-get install -y binutils

# tree
RUN apt-get install -y tree

# Clone repository
RUN git clone https://github.com/drtrigon/sketchbook.git

## Setup a pseudo-display/window manager
#RUN "/sbin/start-stop-daemon --start --quiet --pidfile /tmp/custom_xvfb_1.pid --make-pidfile --background --exec /usr/bin/Xvfb -- :1 -ac -screen 0 1280x1024x16"
#RUN sleep 3
#RUN export DISPLAY=:1.0

# Run Arduino verify
RUN arduino --verify --board $BOARD --verbose --preserve-temp-files $PWD/sketchbook/MultiWii_2_4/MultiWii/MultiWii.ino

# Output post-verify info
RUN export ELF_FILE=`find /tmp/ -name "MultiWii.ino.elf"`; readelf -a $ELF_FILE
RUN export ELF_FILE=`find /tmp/ -name "MultiWii.ino.elf"`; strings -d $ELF_FILE
RUN export ELF_FILE=`find /tmp/ -name "MultiWii.ino.elf"`; size $ELF_FILE
RUN tree
```
Use it in travis-ci:
```
language: bash

sudo: false

env:
  global:
    - DOCKER=drtrigon/docker-arduino DOCKERTAG=ubuntu-14.04  # ARDUINO_VERSION=1.8.2
  matrix:
    #- BOARD="arduino:avr:uno"
    - BOARD="arduino:avr:nano:cpu=atmega328"

services:
  - docker

install:
  - docker pull $DOCKER:$DOCKERTAG ;

script:
  - docker images ;
  - docker run -t -d --name testing drtrigon/docker-arduino:ubuntu-14.04 bin/bash ;
  - docker ps -l ;
  - docker exec -t testing apt-get install -y git ;
  - docker exec -t testing git clone https://github.com/drtrigon/sketchbook.git ;
  - docker exec -t testing arduino --verify --board $BOARD --verbose --preserve-temp-files sketchbook/MultiWii_2_4/MultiWii/MultiWii.ino ;
  # Output post-verify info
  - docker exec -t testing arduino export ELF_FILE=`find /tmp/ -name "MultiWii.ino.elf"` ;
  - docker exec -t testing arduino readelf -a $ELF_FILE
  - docker exec -t testing arduino strings -d $ELF_FILE
  - docker exec -t testing arduino size $ELF_FILE
  - docker exec -t testing arduino tree
  - docker stop testing ;
  - docker rm testing ;
  #- docker rmi $REPO:$COMMIT
  #- docker images ;
  #- docker container ls ;
  - docker ps -l ;

notifications:
  email: false
```
or
```
script:
  - docker images ;
  - git clone https://github.com/drtrigon/sketchbook.git ;
  - docker run -t --rm -v $PWD/sketchbook:/sketchbook $REPO:$COMMIT arduino --verify --board "arduino:avr:nano:cpu=atmega328" --verbose --preserve-temp-files sketchbook/MultiWii_2_4/MultiWii/MultiWii.ino ;
  - docker rmi $REPO:$COMMIT
  - docker images ;
  - docker container ls ;
  - docker ps -l ;
```

## Setup
Overload Origin with Both Remotes in order to clone and set this repo up use:
```
$ git clone file://///data/mount/gvfs/smb-share:server=.../01git/docker-arduino.git
$ cd docker-arduino
$ git remote set-url --add origin https://github.com/drtrigon/docker-arduino.git
```
you can check the settings with:
```
$ git remote -v
origin  /data/mount/gvfs/smb-share:server=.../01git/docker-arduino.git/ (fetch)
origin  /data/mount/gvfs/smb-share:server=.../01git/docker-arduino.git/ (push)
origin  https://github.com/drtrigon/docker-arduino.git (push)
```
(needs one single pull/push only)
