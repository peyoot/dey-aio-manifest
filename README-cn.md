# dey-aio 快速开发Digi嵌入式产品的工具集
dey-aio-manifest项目配合dey-aio，使用repo来管理dey-aio和DEY官方源码。
**[[English]](README.md)**

dey-aio项目全称是Digi Embedded Yocto All In One。它是一个Digi嵌入式核心板的Yocto系统开发工具集，用于更方便地进行SOM系统的裁剪和定制。

功能包括：
  * 支持用docker容器的方式来搭建DEY 系统开发环境。在单个目录下支持所有 DEY 版本（从 DEY 3.2 开始）。
  * docker-compose 和官方原生开发方式并存，可共享相同的工作区和工具。
  * meta-custom示例，用于打包应用程序、配置、驱动程序到固件中。
  * 跨项目共享下载目录和状态缓存目录以节省磁盘空间
  * 用户的源码库和 Digi 源码库可单独分开维护，同时又能协同编译项目。
  * 快速复制必要的固件或设备树驱动到发布目录 和自动生成卡刷安装包zip 文件。
  * 可以选择发布到本地 TFTP 服务器文件夹或 scp 到远程服务器进行共享。

dey-aio项目由中国区的高级系统工程师Robin开发和维护，它将官方的docker开发方式和native开发方式整合在一个工具集内，提供一个自定义的meta-custom layer来实现文件系统定制，方便用户用git的方式来管理系统镜像或自定义设备树驱动，并提供发布脚本帮助用户快速将编译结果的镜像或设备树文件移到发布目录，或是上传到tftp服务器或发布服务器，以方便快速进行开发测试。

dey-aio项目包括了dey-aio和dey-aio-manifest两个部分。dey-aio将DEY项目用docker的方式进行开发，并提供发布脚本和工具。而dey-aio-manifest是用repo的方式将dey-aio和DEY官方的系统开发工具整合在一起，形成一个完整工具集。用户既可以选择docker-compose的方式，也可以用官方的方式来进行开发，两者可共用相同的发布工具。

dey-aio的开发方式可以和官方的源码树协同工作，把Digi官方的源码树和客户的开发部分用不同的git项目分开管理，但又便于整合和升级重构，更有利于产品整个生命周期内开发运维整个项目。如果您觉得本项目不错，欢迎到github上为本项目点个赞。

#### **dey-aio完整工具集安装方法**
---

为了正确安装和使用dey-aio，您需要安装yocto开发环境所需的依赖包，如果您要使用docker进行开发，则还需要安装docker和docker-compose。下面安装过程以Ubuntu 22.04为例，同样也适用于Ubuntu 20.04，请使用普通用户来执行这些命令。

1、安装必要的包

```text-plain
sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool
```

安装上述依赖包之后，还需要指定python版本为python3，最简单的方式是安装一个包：

```text-plain
sudo apt install python-is-python3
```

2、 安装repo并配置好git

```text-plain
sudo apt install repo
git config --global user.name  “yourname”   请用你的英文名称替换yourname
git config --global user.email "you@email.com“  请用你的邮箱替换
```

3、 安装docker和docker-compose

```text-plain
sudo apt install docker.io docker-compose  
sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
newgrp docker     #更新用户组
reboot 请先重启一下电脑
docker ps    #测试docker命令是否可以使用sudo正常使用
docker network create pvpn --subnet 172.100.100.0/24    #创建配合科学上网使用自定义网络

```

4、用repo安装dey-aio工具集

```text-plain
cd
mkdir dey-aio
cd dey-aio
repo init -u https://github.com/peyoot/dey-aio-manifest.git -b china    
repo sync -j8
```

这样，dey-aio的工具集就安装好了。  
 

#### **使用dey-aio工具集进行Digi Embedded Yocto的系统开发**
---

关于科学上网：考虑到国内对github的访问并不顺畅，由于墙对境外IP的阻断方式是间歇式的断开TCP连接，在使用本工具集首次编译时需要科学上网。如果您从未实践过科学上网，可以在位于境外IP的任意云服务器上使用[peyoot/pvpn](https://www.github.com/peyoot/pvpn.git)的开源工具快速架设科学上网环境所需的vpn服务器（[文档](https://www.eccee.com/soft-platform/224.html)），然后在本地开发机器上使用相同的工具自动部署vpn客户端，从而实现编译时所需的科学上网环境。（注：上述的安装过程，不论是否需要科学上网，均可正常使用）

dey-aio的目录结构如下：  
/  
├── aio                     不同dey版本所需的dey-aio工具和自定义layer  
│   ├──gatesgarth        
│   ├── kirkstone  
├── dey4.0                     DEY 4.0工具集  
│   ├──docker-compose.yml      
│   ├── mkproject.sh       项目创建工具    
│   ├── publish.sh             发布工具  
│   ├── workspace            项目目录   
├── dey3.2  
│   ├──docker-compose.yml      
│   ├── mkproject.sh       项目创建工具    
│   ├── publish.sh             发布工具  
│   ├── workspace            项目目录   
| ...  
├── release                    发布文件夹 (可选发布到这里或服务器上)  
│   ├── dey4.0                     
│        ├── cc6ul  
│        ├── ccmp15  
│        ├── cc8mn  
│        ├── cc8mm  
│        ├── cc8x  
│        ├── ...  
│   ├── dey3.2                     
│        ├── ...  
│   └ …  
├── README.md  
└── README-cn.md

要进行项目开发，您需要进入项目所需的DEY版本，然后创建项目。您可以使用docker-compose的方式来创建项目，也可以直接使用官方原生的方法来创建项目。两种方式可共用workspace作为项目目录 。

1、使用docker-compose的开发方式

docker-compose可以快速创建一个与主机隔离的dey的开发环境容器，要创建一个新的docker容器来进行开发，可以用：docker-compose run dey<版本号>，这里的版本号可以是3.2或4.0，容器默认使用peyoot/dey作为DEY的镜像，您也可以修改docker-compose.yml，使用官方的digidotcom/dey镜像。  
例：`docker-compose run dey4.0`  
这样就可以自动开启容器，并提示您创建项目还是使用原有项目继续开发，如下图所示：  
<pre>
 +------------------------------------------------------------------------------------+
 |                                                                                    |
 |                                                                                    |
 |                   Welcome to Digi Embedded Yocto Docker container                  |
 |                                                                                    |
 |  This Docker image is a ready to use system based on Digi Embedded Yocto (DEY) to  |
 |  build custom images for the Digi platforms. DEY is an open source and freely      |
 |  available Yocto Project (TM) based embedded Linux distribution.                   |
 |                                                                                    |
 |                                                                                    |
 +------------------------------------------------------------------------------------+
 Do you wish to create a new platform project [Y/N]?
</pre>
 
输入Y会让您从所支持的开发板中选择当前项目所使用的som或单板机，然后就可以开始编译生成镜像了。  
`bitbake dey-image-qt`  
  
如果输入N，则不创建新项目，用户可以到原有的项目中继续从事开发。

您可以使用exit退出 docker环境，并用docker-compose down来关闭容器。更多用法请参考[dey-aio项目](https://github.com/peyoot/dey-aio.git)。

2、使用官方原生的Digi Embedded Yocto开发方式

dey-aio工具集在安装时就已经自动拉取DEY源码到sources，您可以在workspace中创建项目，直接编译。开发方式和官方并没有区别，只是我们把DEY安装在当前目录下，我们需要进入workspace创建新项目的名称，然后和官方一样，用mkproject.sh来创建项目。本项目对下载目录和sstate缓存做了一些优化处理，它们都存放于父级目录下的project\_shared，以方便不同项目使用。以创建cc93项目为例：

```text-plain
cd workspace
mkdir cc93
cd cc93
source ../../mkproject.sh -l
source ../../mkproject.sh -p ccimx93-dvk
bitbake dey-image-qt
```

#### **关于meta-custom**
---
meta-custom作为一个Yocto的示例layer，用于用户将自定义的程序或配置文件，自启动服务或脚本，驱动程序等文件编译到系统镜像中。用户可以根据项目需要自行更改源码和维护自己的版本库。

#### **使用git的方式管理项目的源码变更**

待续...

#### **使用发布工具快速进行部署和测试**

待续...

