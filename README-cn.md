# dey-aio-manifest
这个git库配合dey-aio，使用repo来管理dey-aio和DEY官方源码。
**[[English]](README.md)**

# 安装Digi-embedded Yocto All In One (dey-aio)
### 使用DEY原生开发方式 
建议使用Ubuntu 22.04 (或Ubuntu 20.04)来安装DEY 开发环境。要使用DEY官方的原生方式开发，需要在主机上预安装必要的依赖包。请执行下面这些命令：
<pre>sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool</pre>
<pre>sudo apt install python-is-python3</pre>
要下载DEY源码，您需要使用repo工具。
<pre>
sudo curl -o /usr/local/bin/repo http://commondatastorage.googleapis.com/git-repo-downloads/repo
sudo chmod a+x /usr/local/bin/repo
sudo chown $USER:$USER /usr/local/bin/repo
</pre>
然后使用repo下载dey-aio，它同样会下载DEY官方源码。
<pre>
mkdir ~/dey-aio
cd ~/dey-aio
repo init -u https://github.com/peyoot/dey-aio-manifest.git -b kirkstone
repo sync -j8 --no-repo-verify
</pre>

这样，你就可以使用dey-aio的全部工具和功能来配合官方的方式开发DEY项目。

### 使用容器的方式来安装DEY开发环境
首先需要安装docker和docker-compose:
<pre>
sudo apt install docker.io docker-compose  
sudo gpasswd -a $USER docker    
newgrp docker  
reboot
docker ps   (now you can see docker is ready)
cd dey4.0
docker-compose run dey4.0
</pre>

更多信息请参考 https://github.com/peyoot/dey-aio.git


License
Copyright 2022, Digi International Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.



