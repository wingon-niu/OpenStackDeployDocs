# 架构介绍 #

## 1. OpenStack集群HA架构 ##

- OpenStack集群的HA架构实现方式与OpenStack官方文档中的一致，采用主主式（Active/Active）。
- 在两台前端节点上面运行着Keepalived和Haproxy，其中Keepalived负责管理虚拟IP，Haproxy负责对三台控制节点上面的相关服务进行负载均衡。
- 具体架构请参考下图：

![architecture_nova_network](http://www.lightcloud.cc/static/img/architecture_nova_network.png)

## 2. Master机与OpenStack集群的关系 ##

- 目前，Master机本身不能是OpenStack集群中的服务器。需要一台OpenStack集群以外的单独的服务器，做为Master机。在Master机上面，向OpenStack集群中的服务器，发出各种安装部署时需要执行的命令。

## 3. OpenStackDeploy安装部署程序运行流程 ##

- 本节叙述 /root/OpenStackDeploy/deploy.sh 程序的总体运行流程。

### 3.0 连通性检查 ###

- 首先检查Master机是否能够正常访问到OpenStack集群中的所有服务器，通过ping和ssh。

### 3.1 复制文件 ###

- 将安装部署过程中需要的文件，从Master机复制到所有OpenStack集群服务器上面。

### 3.2 基本环境安装配置 ###

- 在所有的OpenStack集群服务器上面，执行一个程序，进行基本的软件环境安装配置。
- 这一过程，在所有的OpenStack集群服务器上面，同时并发进行。
- 此过程中可能会升级内核相关文件，所以当这一过程结束后，所有OpenStack集群服务器都会重新启动一次。

### 3.3 安装前端节点 ###

- 当上一步完成，所有OpenStack服务器都重新启动以后，首先安装部署两台前端节点。
- 这一过程，在两台前端节点上面同时并发进行。
- 安装完成之后，两台前端节点会自动重新启动一次。（不重新启动一次的话，haproxy不能正常生成日志文件，ubuntu14.04上面才出现的问题，ubuntu12.04上面没有这个问题。待改进。）

### 3.4 安装控制节点 ###

- 当所有前端节点安装配置完成之后，开始安装配置所有的控制节点。
- 在每台控制节点上面，需要按照顺序执行3个安装命令，记为 Part-1，Part-2，Part-3，前两个命令只能是顺序串行进行，不能并发进行。

#### 3.4.1 在 控制节点1 上面执行 Part-1 #

- 安装 Mysql Galera 和 RabbitMQ 的第1部分

#### 3.4.2 在 控制节点2 上面执行 Part-1 #

- 安装 Mysql Galera 和 RabbitMQ 的第1部分

#### 3.4.3 在 控制节点3 上面执行 Part-1 #

- 安装 Mysql Galera 和 RabbitMQ 的第1部分

#### 3.4.4 在 控制节点1 上面执行 Part-2 #

- 安装 Mysql Galera 和 RabbitMQ 的第2部分

#### 3.4.5 在 控制节点2 上面执行 Part-2 #

- 安装 Mysql Galera 和 RabbitMQ 的第2部分

#### 3.4.6 在 控制节点3 上面执行 Part-2 #

- 安装 Mysql Galera 和 RabbitMQ 的第2部分

#### 3.4.7 在三台控制节点上面同时并发执行 Part-3 ####

- 安装OpenStack控制节点相关组件：Keystone、Glance、Cinder、Horizon、所有Nova-api、Neutron Server（如果选择使用neutron）。

### 3.5 安装网络节点 ###

- 在所有控制节点安装配置完成之后，开始安装配置所有的网络节点。
- 这个过程在所有网络节点上面同时并发进行。
- 如果选择使用 neutron，则所有网络节点安装完成之后，会自动重新启动一次，以让br-ex的IP生效。（安装程序会自动设置br-ex来取代外网网卡的IP，但是此时却不能够使用service networking restart来重启网络，命令失效，只好重启服务器。待改进。）

### 3.6 安装计算节点 ###

- 在所有网络节点安装配置完成之后，开始安装配置所有的计算节点。
- 这个过程在所有计算节点上面同时并发进行。

### 3.7 安装完成后的处理 ###

- 在所有计算节点安装配置完成之后，OpenStack集群本身的安装配置已经完成。
- 本步骤为admin租户创建了一个内部网络、设置了常用的安全组规则，以及进行创建外部网络、浮动IP池，上传cirros镜像等等辅助操作。

### 3.8 输出相关信息 ###

- 输出 Horizon 的访问地址和相关感谢信息。

<br>
##### <center> 2014-10-7 18:18:50 </center> #####
<br>



