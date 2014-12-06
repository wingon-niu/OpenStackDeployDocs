# 系统简介 #

#### 　　自动化安装部署带有HA的OpenStack/Ceph集群。 ####

# 使用方法总体介绍 #

## 安装步骤 ##

1. 假设集群总共有10台服务器（1台用作Master节点，9台用于安装OpenStack/Ceph集群）。

2. 首先请使用常规方法在所有服务器上面安装好操作系统，并设置好网卡的IP。

3. 在本网站注册用户并登录，创建一个拥有9台服务器的OpenStack/Ceph集群。

4. 按照设置向导，一步一步将OpenStack集群的相关信息输入。

5. 生成并下载安装包，并将安装包复制到Master节点上面。(安装包由安装脚本打包压缩组成，大小总共不到90K)

6. 在Master节点及OpenStack/Ceph集群节点上面进行少量的辅助操作。

7. 在Master节点上面，运行一条命令，就可以完成对OpenStack/Ceph集群的安装部署。

8. 在安装部署完成以后，可以运行一条命令，将OpenStack/Ceph集群完全卸载。

## 支持特性 ##

1. 默认安装部署OpenStack Icehouse版本，Juno版本还在测试中。

2. OpenStack网络模型支持 nova-network(FlatDHCPManager/VlanManager) 和 neutron(gre/vxlan)。

3. OpenStack后端存储支持 Local disk 和 Ceph block device。

4. 如果选择使用Ceph，则默认安装部署Ceph firefly版本，giant版本还在测试中。

5. OpenStack HA（高可用）架构请参看《架构介绍》。

## 前提条件 ##

1. OpenStack/Ceph集群中的服务器至少有两个网口。

2. Master节点及OpenStack/Ceph集群中的服务器都需要能够正常访问互联网。

# 使用方法详细介绍 #

## 1. 安装配置OpenStack集群服务器 ##

1. 请先为您的所有OpenStack集群服务器安装好操作系统，只支持 Ubuntu 14.04.x LTS Server 版，按照一般常规方法安装即可，无特殊要求。
2. 按照实际网络情况，设置好所有服务器上两个网口的IP地址。注意，如果网络节点使用neutron，则这台网络节点的IP地址必须设置在/etc/network/interfaces文件中，不能设置在/etc/network/interfaces.d目录下的文件中。
3. 在所有服务器上面启用root用户，并且修改ssh配置文件，允许root用户远程登录。
4. 到此，对OpenStack集群服务器的基本安装设置已经完成。

## 2. 建立并下载安装包 ##

### 2.1 在本网站注册用户并登录 ###

![注册并登录](http://www.lightcloud.cc/static/img/cn/regist.png)

### 2.2 建立一个OpenStack集群 ###

![功能列表_新增集群](http://www.lightcloud.cc/static/img/cn/functions_add_new_cluster.png)

![集群信息](http://www.lightcloud.cc/static/img/cn/cluser_info.png)

### 2.3 开始对OpenStack集群进行设置，使用设置向导 ###

![设置向导](http://www.lightcloud.cc/static/img/cn/director.png)

### 2.4 进行：服务器设置 ###

- 按照您的OpenStack集群物理服务器的网口和IP的真实情况，在本页面进行相应设置。
- 主机名称和FQDN可以按照需要设置成新的内容，建议按照服务器充当的角色来设置，例如图片中的设置。
- 设置完成后，点击　保存并下一步　按钮。

![Server-1](http://www.lightcloud.cc/static/img/cn/server_1_info.png) 

![Server-2](http://www.lightcloud.cc/static/img/cn/server_2_info.png)

![Server-3](http://www.lightcloud.cc/static/img/cn/server_3_info.png)

![Server-4](http://www.lightcloud.cc/static/img/cn/server_4_info.png)

![Server-5](http://www.lightcloud.cc/static/img/cn/server_5_info.png)

![Server-6](http://www.lightcloud.cc/static/img/cn/server_6_info.png)

![Server-7](http://www.lightcloud.cc/static/img/cn/server_7_info.png)

### 2.5 进行：角色分配 ###

- 在本页面对每台OpenStack集群服务器充当的角色进行设置。
- 设置完成后，点击　保存并下一步　按钮。 

![角色分配](http://www.lightcloud.cc/static/img/cn/rules.png)

### 2.6 进行：网络设置 ###

- 在本页面中进行OpenStack集群网络相关的设置。
- 网络组件可以选择使用 nova-network 或者 neutron。
- 设置完成后，点击　保存并下一步　按钮。

- 网络组件使用：nova-network，默认支持 multi_host 方式。

![网络设置nova-network](http://www.lightcloud.cc/static/img/cn/network_setting.png)

- 网络组件使用：neutron，管理网络和外部网络使用同一个网口，Tunnel网络单独使用一个网口。

![网络设置neutron](http://www.lightcloud.cc/static/img/cn/network_setting_neutron.png)

### 2.7 进行：认证设置 ###

- 在本页面中设置OpenStack各组件在Mysql中的用户名和密码、Mysql root用户的密码，以及Keystone的admin和service用户的密码。
- 设置完成后，点击　保存并下一步　按钮。

![认证设置](http://www.lightcloud.cc/static/img/cn/auth.png)

### 2.8 进行：存储设置 ###

- 在本页面中设置一个磁盘文件，供cinder使用来提供块存储。（在安装部署的时候可以改为使用Ceph）
- 设置完成后，点击　保存并下一步　按钮。

![存储设置](http://www.lightcloud.cc/static/img/cn/storage.png)

### 2.9 进行：其他设置 ###

- 在本页面进行虚拟化类型、ubuntu软件源、keepalived通知邮箱的设置。
- 设置完成后，点击　保存并下一步　按钮。

![其他设置](http://www.lightcloud.cc/static/img/cn/other.png)

### 2.10 进行：生成并下载安装包 ###

- 到此，所有设置完成，在本页面生成并下载安装包到本地。安装包的文件名为：OpenStackDeploy.tar.gz
- 注意：只能使用浏览器在网页上面点击下载链接进行下载，不能通过wget等其它工具下载。

![生成安装包](http://www.lightcloud.cc/static/img/cn/make_pkg.png)

![下载安装包](http://www.lightcloud.cc/static/img/cn/download_pkg.png)

## 3. 部署 ##

- 注意：在OpenStack集群之外，还需要有一台服务器，做为Master节点，用于管理OpenStack集群。（目前还不支持使用OpenStack集群中的服务器做为Master节点。）
- Master节点的操作系统可以是：RedHat/CentOS/Ubuntu等常见的Linux系统。建议使用Ubuntu。
- 在部署OpenStack集群以前，在Master节点上面，需要做以下的准备工作：
- 安装需要的软件：expect screen rsync wget。如果没有安装，使用如下命令安装：
- 　　RedHat/CentOS: yum install -y expect screen rsync wget
- 　　Ubuntu: apt-get install -y expect screen rsync wget
- Master节点需要能够正常访问OpenStack集群中的所有服务器，默认是通过在OpenStack集群中设置的外部网络来进行访问。
- 为Master节点的root用户建立到OpenStack集群中的所有服务器的信任关系。也即能够在Master节点上，使用root用户ssh远程登录OpenStack集群中的所有服务器，登录时不用输入密码。
- 到此，在Master节点上面的准备工作已经完成。
- 以下的操作，都是在Master节点上面使用root用户进行。

### 3.1 开始安装部署OpenStack集群 ###

- 将下载的安装包放到Master节点的 /root 目录下，接着执行下面的命令：

    cd /root

    tar xzf OpenStackDeploy.tar.gz

    cd /root/OpenStackDeploy

    ls -l

    ./download-pkg.sh

- 上面的 ./download-pkg.sh 用于从互联网下载 Mysql Galera 的安装包和cirros镜像文件，共3个文件，总共30M左右。命令成功结束之后，会在 /root 目录下，创建两个子目录，mysql_galera 和 images，在里面存放下载到的文件。请务必保证这3个文件能够正常下载成功，后续的安装部署过程中会使用到。正常的下载文件结果如下图所示：

![下载mysql_galera_cirros](http://www.lightcloud.cc/static/img/mysql_galera_images.png)

- 如果要使用Ceph做为glance、cinder、nova-compute的后端存储，则需要运行下面两条命令，以更改安装配置信息，在后面的安装过程中将使用第2个前端节点做为ceph-deploy admin node，所有的计算节点做为mon和osd，第1个计算节点做为mds。<font color=red>同时前提条件是最少有2个计算节点。</font>（由于现在尚未在网页上面实现安装配置Ceph的功能，所以需要手动执行这个步骤。如果不使用Ceph，则不需要执行这个步骤。）

    cd /root/OpenStackDeploy

    ./activate-ceph.sh

- 在确保上面的 ./download-pkg.sh 命令成功完成之后，运行下面两条命令，然后等待命令执行完成，就可全部完成OpenStack/Ceph集群的安装部署。

    cd /root/OpenStackDeploy

    ./deploy.sh

- 等待命令执行完成。到此，OpenStack/Ceph集群已经安装部署完毕。

### 3.2 在安装部署过程中查看进度 ###

- 在执行 ./deploy.sh 以前，将文件 /root/OpenStackDeploy/locale.txt 中的 LOCALE=EN 换成 LOCALE=CN，则可以在安装部署过程中，将主要的输出信息显示为中文，如下图所示。（前提是Master节点本身设置为中文的locale环境zh_CN.UTF-8，而且用于连接Master节点的ssh客户端软件比如 Putty/Xshell/SecureCRT 等，也需要设置为utf-8，才能正常显示中文。）

![输出信息为中文](http://www.lightcloud.cc/static/img/cn/output_cn_info.png)

- 在安装部署过程中，每当出现例如上图中红色椭圆框内所示‘screen -r niu’字样的提示信息，则表明此时正在OpenStack集群中相应的服务器上面进行安装配置的操作，此时可以通过执行下述两个步骤，来实时查看OpenStack集群中相应服务器上面的安装配置过程：
- 1、使用ssh客户端软件，建立一个root用户到Master节点的新的ssh连接。
- 2、在这个新的ssh连接中，直接运行 ‘screen -r niu’，就可以实时看到OpenStack集群中相应服务器上面的命令运行情况。可以同时按下Ctrl键和a，都松开，再按n，来切换不同服务器对应的窗口。（screen的具体用法，请参考相关的技术文档）
- 如果不需要实时查看在OpenStack集群服务器上面的命令运行情况，则可以不理会3.2本节的内容，静候 ./deploy.sh 命令执行完毕，然后可以查看安装部署过程中每个步骤生成的日志文件。

### 3.3 在安装部署结束之后查看日志 ###

- 在 ./deploy.sh 执行完毕之后，会在 /root/OpenStackDeploy 目录下，创建一个 log 目录，里面存放着全部安装部署过程中每个步骤生成的日志文件，如下图所示：

![日志列表1](http://www.lightcloud.cc/static/img/log_list_1.png)

- 安装部署过程中具体做了哪些操作步骤，请查看本网站首页上面《架构介绍》中的相关部分。
- 上图中的日志文件，可以在如下链接中访问到：
- [https://github.com/wingon-niu/OpenStackDeployDocs/tree/master/zh-CN/some-logs/2014-10-05](https://github.com/wingon-niu/OpenStackDeployDocs/tree/master/zh-CN/some-logs/2014-10-05 "OpenStackDeploy-logs-2014-10-05") 
- 如果使用Ceph做为后端存储，则安装Ceph的时候也会生成对应的日志文件，示例见下面的链接：
- [https://github.com/wingon-niu/OpenStackDeployDocs/tree/master/zh-CN/some-logs/2014-10-28-ceph-install](https://github.com/wingon-niu/OpenStackDeployDocs/tree/master/zh-CN/some-logs/2014-10-28-ceph-install "OpenStackDeploy-logs-2014-10-28-ceph-install") 

### 3.4 在安装部署结束之后启动虚拟机 ###

- 使用admin用户在OpenStack Horizon界面登录，可以看到已经创建了相关的路由器、网络、安全组、镜像、云硬盘（如果使用Ceph）等等。
- 如果使用Ceph，在创建虚拟机的时候，可以选择“从云硬盘启动”或者“从镜像启动（创建一个新卷）”。
- 如果未使用Ceph，在创建虚拟机的时候，可以选择“从镜像启动”。

## 4. 卸载 ##

- 重要说明：在上述的安装部署过程中，会在Master节点上面的/root/OpenStackDeploy目录下创建一些用于记录集群相关信息的文件，这些文件对于集群的管控至关重要，如非必要，请不要手动修改或者删除这些文件。

- 前提条件：上述的安装部署过程正常完成以后，没有手动对Master节点上面的/root/OpenStackDeploy目录下的相关信息文件进行过修改或者删除。

- 卸载前，请先终止（删除）所有虚拟机。

- 在Master节点上面，使用root用户运行下面的命令，就可以完成对OpenStack/Ceph集群的卸载。

    cd /root/OpenStackDeploy

    ./undeploy.sh


<br>
##### <center> 2014-10-7 14:10:12 </center> #####
<br>
