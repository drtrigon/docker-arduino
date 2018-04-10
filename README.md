[![Build Status](https://travis-ci.org/drtrigon/docker-arduino.svg?branch=master)](https://travis-ci.org/drtrigon/docker-arduino)
[![Image Status](https://images.microbadger.com/badges/image/pywikibotcatfiles/file-metadata.svg)](https://microbadger.com/images/pywikibotcatfiles/file-metadata)

https://github.com/pywikibot-catfiles/docker-file-metadata

https://github.com/drtrigon/sketchbook

(https://github.com/drtrigon/eagle)

https://github.com/drtrigon/test-eagle-edrc

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
