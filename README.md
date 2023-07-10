# dey-aio-manifest
This repository contains the manifest files for the dey-aio for Digi's Embedded modules.

# Installing Digi Embedded Yocto All In One (dey-aio)
### use native DEY 
We recommend you use Ubuntu 22.04 (or Ubuntu 20.04) to install the development environment. 
To use native DEY developement tool You must install essential host packages on your build host first. 
<pre>sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool</pre>
<pre>sudo apt install python-is-python3</pre>

To download dey-aio, you need the repo tool.
<pre>
sudo curl -o /usr/local/bin/repo http://commondatastorage.googleapis.com/git-repo-downloads/repo
sudo chmod a+x /usr/local/bin/repo
sudo chown $USER:USER /usr/local/bin/repo
</pre>

Use repo to download dey-aio, it will also set up native Digi Embedded Yocto
$ repo init -u https://github.com/peyoot/dey-aio-manifest.git -b kirkstone
$ repo sync -j8 --no-repo-verify

### use DEY in docker container way
To use DEY container you'll need to install docker and docker compose first.
<pre>
sudo apt install docker.io docker-compose  
sudo gpasswd -a $USER docker    
newgrp docker  
reboot
docker ps   (now you can see docker is ready)
cd dey4.0
docker-compose run dey4.0
</pre>

for more details about dey-aio, please refer to https://github.com/peyoot/dey-aio.git


License
Copyright 2022, Digi International Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.



