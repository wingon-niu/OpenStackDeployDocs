# 使用介绍 #
##### 　　本文档将讲述快速搭建一个带有HA的OpenStack集群的方法和步骤。 #####
## 1. 安装配置OpenStack集群服务器
#### （先决条件：所有服务器都需要有两个网口） ####
1. 请先为您的OpenStack集群服务器安装好操作系统，只支持Ubuntu 14.04 LTS（或以上版本），按照一般方法安装即可，无特殊要求。
2. 按照实际网络情况，设置好所有服务器上两个网口的IP地址。注意，如果网络节点使用neutron，则这台网络节点的IP地址必须设置在/etc/network/interfaces文件中，不能设置在/etc/network/interfaces.d目录下的文件中。
3. 在所有服务器上面启用root用户，并且修改ssh配置文件，允许root用户远程登录。
4. 如果没有本地的Ubuntu 14.04软件源，则OpenStack集群中的所有服务器都必须能够正常访问互联网。 
5. 到此，对OpenStack集群服务器的基本安装设置已经完成。
## 2. 建立并下载安装包 ##
### 2.1 在本网站注册用户并登录 ###

![注册并登录](http://www.lightcloud.cc/static/img/cn/regist.png)

### 2.2 建立一个OpenStack集群 ###

![功能列表](http://www.lightcloud.cc/static/img/cn/functions.png)

![新增集群](http://www.lightcloud.cc/static/img/cn/add_new_cluster.png)

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

- 网络组件使用：nova-network，只支持 multi_host 方式，无需设置。

![网络设置nova-network](http://www.lightcloud.cc/static/img/cn/network_setting.png)

- 网络组件使用：neutron

![网络设置neutron](http://www.lightcloud.cc/static/img/cn/network_setting_neutron.png)

### 2.7 进行：认证设置 ###
- 在本页面中设置OpenStack各组件在Mysql中的用户名和密码、Mysql root用户的密码，以及Keystone的admin和service用户的密码。
- 设置完成后，点击　保存并下一步　按钮。

![认证设置](http://www.lightcloud.cc/static/img/cn/auth.png)

### 2.8 进行：存储设置 ###
- 在本页面中设置一个磁盘文件，供cinder使用来提供块存储。（目前只支持这一种方法）
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
- 注意：在OpenStack集群之外，还需要有一台服务器，做为Master机，用于管理OpenStack集群。（目前还不支持使用OpenStack集群中的服务器做为Master机。）
- Master机的操作系统可以是：RedHat/CentOS/Ubuntu等常见的Linux系统。建议使用Ubuntu。
- 在部署OpenStack集群以前，在Master机上面，需要做以下的准备工作：
- 安装需要的软件：expect screen rsync wget。如果没有安装，使用如下命令安装：
- 　　RedHat/CentOS: yum install -y expect screen rsync wget
- 　　Ubuntu: apt-get install -y expect screen rsync wget
- Master机需要能够正常访问OpenStack集群中的所有服务器，默认是通过在OpenStack集群中设置的外部网络来进行访问。
- 为Master机的root用户建立到OpenStack集群中的所有服务器的信任关系。也即能够在Master机上，使用root用户ssh远程登录OpenStack集群中的所有服务器，登录时不用输入密码。
- 到此，在Master机上面的准备工作已经完成。
- 以下的操作，都是在Master机上面进行。

### 3.1 开始安装部署OpenStack集群 ###
- 将下载的安装包放到Master机的 /root 目录下，接着执行下面的命令：

    cd /root

    tar xzf OpenStackDeploy.tar.gz

    cd /root/OpenStackDeploy

    ls -l

    ./download-pkg.sh
- 上面的 ./download-pkg.sh 用于从互联网下载 Mysql Galera 的安装包和cirros镜像文件，共3个文件，总共30M左右。命令成功结束之后，会在 /root 目录下，创建两个子目录，mysql_galera 和 images，在里面存放下载到的文件。请务必保证这3个文件能够正常下载成功，后续的安装部署过程中会使用到。正常的下载文件结果如下图所示：

![下载mysql_galera_cirros](http://www.lightcloud.cc/static/img/mysql_galera_images.png)

- 在确保上面的下载命令成功完成之后，运行下面两条命令，然后等待命令执行完成。

    cd /root/OpenStackDeploy

    ./deploy.sh
- 等待命令执行完成。到此，OpenStack集群已经安装部署完毕。

### 3.2 在安装部署过程中查看进度 ###
- 在执行 ./deploy.sh 以前，将文件 /root/OpenStackDeploy/locale.txt 中的 LOCALE=EN 换成 LOCALE=CN，则可以在安装部署过程中，将输出信息显示为中文，如下图所示。（前提是Master机本身设置为中文的locale环境zh_CN.UTF-8，而且用于连接Master机的ssh客户端软件比如 Putty/Xshell/SecureCRT 等，也需要设置为utf-8，才能正常显示中文。）

![输出信息为中文](http://www.lightcloud.cc/static/img/cn/output_cn_info.png)

- 在安装部署过程中，每当出现例如上图中红色椭圆框内所示‘screen -r niu’字样的提示信息，则表明此时正在OpenStack集群中相应的服务器上面进行安装配置的操作，此时可以通过执行下述两个步骤，来实时查看OpenStack集群中相应服务器上面的安装配置过程：
- 1、使用ssh客户端软件，建立一个到Master机的新的连接。
- 2、在这个新的连接中，直接运行 ‘screen -r niu’，就可以实时看到OpenStack集群中相应服务器上面的命令运行情况。可以同时按下Ctrl键和a，都松开，再按n，来切换不同服务器对应的窗口。（screen的具体用法，请参考相关的技术文档）
- 　
- 如果不需要实时查看在OpenStack集群服务器上面的命令运行情况，则可以不理会3.2本节的内容，静候 ./deploy.sh 命令执行完毕，然后可以查看安装部署过程中每个步骤生成的日志文件。
- 　

### 3.3 在安装部署结束之后查看日志 ###
- 在 ./deploy.sh 执行完毕之后，会在 /root/OpenStackDeploy 目录下，创建一个 log 目录，里面存放着全部安装部署过程中每个步骤生成的日志文件，如下图所示：

![日志列表1](http://www.lightcloud.cc/static/img/log_list_1.png)

- 具体的安装部署过程，请查看本网站首页上面的《架构介绍》部分。
- 上图中的日志文件，可以在如下链接中访问到：
- [https://github.com/wingon-niu/OpenStackDeployDocs/tree/master/zh-CN/some-logs/2014-10-05](https://github.com/wingon-niu/OpenStackDeployDocs/tree/master/zh-CN/some-logs/2014-10-05 "OpenStackDeploy-logs-2014-10-05") 

<br>
##### <center> 2014-10-7 14:10:12 </center> #####
<br>
