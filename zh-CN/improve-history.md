# 改进历史 #

##### 　　本文档用于记录现存的问题与问题的解决情况。 #####

<br><br>

### 修正记录　2014-11-02 ###

- <font color=blue>A04</font>　实现了自动化安装部署ceph，并将glance、cinder、nova-compute配置为使用ceph block devices。这样glance、cinder这两个服务可以同时运行在三台控制节点上面。

### 修正记录　2014-10-18 ###

- <font color=blue>A04</font>　解决了与memcached相关的部分，在Haproxy上面将三台控制节点上面的memcached做成了主备式，所有相关服务（Horizon、nova-novncproxy、nova-consoleauth、memcached）可以同时运行在三台控制节点上面。

### 修正记录　2014-10-12 ###

- <font color=blue>A03</font>　实现了在服务器只有两个网口的情况下正常使用neutron，管理网络和外部网络共用一个网口，Tunnel网络单独使用一个网口。

- <font color=blue>A05</font>　实现了neutron下vxlan的配置。

<br>

### 现存问题　2014-10-07 ###

- <font color=red>A01</font>　现在使用三台服务器安装了Mysql Galera集群，主主式备份。在OpenStack网络模式采用nova-network的时候，如果同时创建多台虚拟机，比如10台，会报出多条数据库发生死锁的错误，最后只能创建成功2、3台虚拟机，其它虚拟机都进入错误状态。但是在OpenStack网络模式采用neutron的时候，则没有这个错误，可以同时创建多台虚拟机。另外，已经在Haproxy这里把三台Mysql做成了主备式，OpenStack各个组件也是通过相同的虚拟IP来访问Mysql的。

- <font color=red>A02</font>　在OpenStack网络模式采用nova-network，租户网络使用VlanManager的时候，在同一台计算节点上面，不同租户各自的私有内部网络，没有隔离。全部的4种情况如下：1、同一个租户，在同一个计算节点上面的VM，可以互通。2、同一个租户，在不同的计算节点上面的VM，可以互通。3、不同的租户，在同一个计算节点上面的VM，可以互通。4、不同的租户，在不同的计算节点上面的VM，不能互通。其中第3种情况，是与预期不符的。

- <font color=blue>A03</font>　现在的安装部署程序，只支持服务器上面有两个网卡（网口）。如果网络模式采用neutron，管理网络和外部网络各自使用一个网口，那么Tunnel网络就必须和管理网络或者外部网络共用同一个网口，比如Tunnel网络和管理网络共用同一个网口，外部网络单独用一个网口。这种情况下，也能正常完成OpenStack集群的安装部署，创建的租户、内部网络、外部网络、浮动IP都可以正常使用，可以ping通绑定在VM上面的浮动IP，也可以通过浮动IP使用ssh登入VM，一切看起来都是正常的。但是在后续的使用当中，会出问题。例如这个问题：如果计算节点和网络节点不是同一台物理机，那么使用ssh通过浮动IP登入这台计算节点上面的VM，在上面可以正常执行一些命令，但是有时候运行一下vi，整个界面就静止不动了，再无反应。如果计算节点与网络节点是同一台物理机，就没有这个问题。

- <font color=blue>A04</font>　现在的安装部署，总共有3台控制节点，每台上面，都有完整的互相独立的OpenStack服务，这些服务，都使用本机的相应资源。这带来了三个方面的问题：Glance、Cinder、 以及与memcached相关的3个服务（Horizon、nova-novncproxy、nova-consoleauth）。解决这些问题有两个途径：1、在Haproxy上面将这些服务做成主备式。2、引入自身具有高可用性的共享存储和内存数据库。目前安装部署程序还没有采用这两种方法，而是采用了一个临时的做法：把后面两台控制节点上面的这5个服务停掉了，这样OpenStack的各个功能就可以正常使用，但是涉及到这5个服务的高可用性则失去了。

- <font color=blue>A05</font>　关于neutron下vxlan的配置，目前尚未实现，程序中已经搭好了架子，等待将代码填进去。

<br><br><br>
