# 开发计划 #

##### 　　　　本文档用于说明下一步将要开发的内容或者将要实现的功能。 #####

<br><br>

### 计划内容　　2014-10-07 ###

- <font color=blue>（已实现 2014-10-12）</font>实现OpenStack网络模式采用neutron时，管理网络和外部网络共用同一个网口，Tunnel网络单独使用一个网口。（这个功能其实现在就有，需要修改的地方，是keepalived和haproxy的配置方法。）

- <font color=blue>（已实现 2014-10-12）</font>实现neutron下vxlan的配置。

- <font color=blue>（已实现 2014-10-18）</font>在Haproxy中将memcached改为主备式。

- <font color=blue>（已实现 2014-11-02）</font>实现自动化安装部署ceph，并且glance、cinder、nova-compute使用ceph block devices做为后端存储。

- 实现对已安装的OpenStack组件进行完全卸载的功能。

- 实现安装OpenStack其它组件的功能，比如计量、FWaaS、LBaaS等等。

- 跟随OpenStack本身的升级，升级到安装部署 J 版。

- 实现在OpenStack集群中新增计算节点的功能。

- 实现对OpenStack集群各项配置的相关修改功能，比如修改服务器IP、网络组件、网络模式等等。

- <font color=red>（寻合作）</font>实现在同样的安装部署方式下，支持RedHat/CentOS等操作系统。

- <font color=red>（寻合作）</font>实现像FUEL那样可以连操作系统一起安装的功能。

- 网站上的所有文档，都放上Github，同时寻高手，给翻译成E文。



<br><br><br>
