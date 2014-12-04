# 开发计划 #

<br><br><br>

### 计划内容　　2014-10-07 ###

- <font color=blue>（已实现 2014-10-12）</font>实现OpenStack网络模式采用neutron时，管理网络和外部网络共用同一个网口，Tunnel网络单独使用一个网口。

- <font color=blue>（已实现 2014-10-12）</font>实现neutron下vxlan的配置。

- <font color=blue>（已实现 2014-10-18）</font>在Haproxy中将memcached改为主备式。

- <font color=blue>（已实现 2014-11-02）</font>实现自动化安装部署ceph，并且glance、cinder、nova-compute使用ceph block devices做为后端存储。

- <font color=blue>（已实现 2014-11-15）</font>跟随OpenStack本身的升级，升级到安装部署 Juno 版本。

- <font color=blue>（已实现 2014-11-29）</font>实现对OpenStack集群和Ceph集群进行完全卸载的功能。

- 实现在OpenStack集群中新增计算节点的功能。

- 实现安装OpenStack其它组件的功能，比如计量、FWaaS、LBaaS等等。

- 实现对OpenStack集群各项配置的相关修改功能，比如修改服务器IP、网络组件、网络模式等等。

- <font color=red>（寻合作）</font>实现在同样的安装部署方式下，支持RedHat/CentOS等操作系统。

- <font color=red>（寻合作）</font>实现像FUEL那样可以直接在裸机上连同操作系统一起安装的功能。

- 网站上的所有文档，都放上Github，同时寻高手翻译成E文。

<br><br><br>
