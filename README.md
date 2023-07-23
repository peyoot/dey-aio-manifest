# dey-aio toolset
This repository (dey-aio-manifest) contains the manifest files for the dey-aio toolset.
**[[中文说明]](README-cn.md)**

dey-aio stands for Digi Embedded Yocto All In One. It is a system development toolset for Digi's embedded products(som,sbc/dvk) for easier tailoring and customization of device tree/firmwares.

The dey-aio project is developed and maintained by Robin, a senior system engineer/FAE. It integrates the official docker development method and native development method in a single toolset, provides a custom meta-custom layer to achieve file system customization, facilitates users to manage system images or custom device tree in their git repositories, and provide publish tools to help users quickly move/pack the firmwares or device tree files to the release folder. or upload to TFTP server or server after compile for quick testing and debugging.

The dey-aio project consists of two parts: dey-aio and dey-aio-manifest. dey-aio provide the docker-compose methods to develop DEY projects,  and provides meta-custom and publish tools. The dey-aio-manifest is a repo way to integrate dey-aio and DEY's official system development tools to form a complete toolset. Users can choose either docker-compose or official native development which share the same publishing tools.

The native development method of dey-aio use Digi official repositories. And the official source code tree of Digi and the customer's own design can be managed separately by different git repositories. And it is convenient for both parts to be maintained seperately while can work together and co-compile to build the final products' firmwares.

#### Installation

In order to properly install and use dey-aio, you need to install the dependencies required by the yocto development environment, and if you want to use docker for development, you also need to install docker and docker-compose. The following installation process takes Ubuntu 22.04 as an example, and also applies to Ubuntu 20.04, please use a normal user to execute these commands.

1\. Install the necessary packages

```text-plain
sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool
```

After installing the above dependencies, you also need to specify the python version as python3, the easiest way is to install a package:

```text-plain
sudo apt install python-is-python3
```

 2. Install repo and configure git

```text-plain
sudo apt install repo
git config --global user.name  “yourname”   
git config --global user.email "you@email.com“  
```

3\. Install docker and docker-compose

```text-plain
sudo apt install docker.io docker-compose  
sudo gpasswd -a $USER docker     
newgrp docker     
reboot       #some linux distribution will need reboot to make docker work.
docker ps    #after reboot, you can test if docker can work with your login account.
```

4、Install the full dey-aio toolset with repo

```text-plain
cd
mkdir dey-aio
cd dey-aio
repo init -u https://github.com/peyoot/dey-aio-manifest.git -b main
repo sync -j8
```

Now dey-aio toolset is ready to work!

#### Usage

dey-aio folder struture  ：  
/  
├── aio                     dey-aio source  
│   ├──gatesgarth        
│   ├── kirkstone  
├── dey4.0                       
│   ├──docker-compose.yml      
│   ├── mkproject.sh           
│   ├── publish.sh               
│   ├── workspace               
├── dey3.2  
│   ├──docker-compose.yml      
│   ├── mkproject.sh           
│   ├── publish.sh               
│   ├── workspace               
| ...  
├── release                      
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

To do project development, you need to go into the desired DEY version of the project and then create the project . You can use docker-compose to create a project, or you can create a project directly using the official native method under workspace sub-folder. Both ways can share workspace as the project directory.

1\. Use docker-compose 

docker-compose can quickly create a dey development environment container and it is isolated from the host. To create a new docker container for development, you can use:   
`docker-compose run dey<version>`  
 here the version number can be 3.2 or 4.0, the container defaults to peyoot/dey as the DEY image, you can also modify docker-compose.yml, use the official digidotcom/dey image. Example: 

```text-plain
cd dey4.0
docker-compose run dey4.0
```

This automatically opens the container and prompts you to create a project or continue development using the original project:

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

 when you input “Y”, it will let you choose which som platform you're working with can create the project based on your choice. And then you can start to build the firmwares.

2\. Use the official native Digi Embedded Yocto development method

The dey-aio toolset automatically pulls the DEY source code when it is being installed. And you can create a project in the workspace folder and compile it directly. This method is no different from the official one, except that it install DEY in the sources folder of currentdirectory.  we need to enter the corresponding workspace folder to create the new project.  dey-aio has made some optimizations for different projects to share the download folder and sstate cache, which are stored in the project\_shared under workspace. Take the creation of the cc93 project as an example:

```text-plain
cd workspace
mkdir cc93
cd cc93
source ../../mkproject.sh -l
source ../../mkproject.sh -p ccimx93-dvk
bitbake dey-image-qt
```

More documents  will comming soon. You can also refer to Digi official document web portal for help.

#### About meta-custom

meta-custom serves as a Yocto example layer for users to compile custom programs or configuration files, self-starting services or scripts, drivers,  into the system images. Users can change the source code and maintain their own version according to the needs of the project.

#### Special note for docker working with VPN 

 In some countries where goverment have enforced internet censorship. You may need VPN to get full access to github and other resources. The default docker-compose file will fail to work when you enable openvpn while not specify the network. The solution is to create a docker network in advance and use this dedicated network instead of docker default one.   
`docker network create pvpn` `--``subnet` `172.100``.``100.0``/``24`

In docker-compose.yml , add two line to use this newly created network and everything will work as expected.

```text-plain
version: ‘3’
services:
 dey4.0:
   …
networks:
 default:
+    external:
+      name: pvpn
```

To simplify the process, customer can aslo check “china” branch instead of “main” to enable the toolset working with VPN after create the external docker network.



