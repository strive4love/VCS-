# VCS(Veritas Cluster Server)集群维护的常用命令 
-----------------
在前面的文章“盘点Linux平台下的高可用集群软件”介绍了当前主流的集群软件，其中VCS（Veritas Cluster Server）是在电信和金融行业用得较为广泛的，因为VCS提供了较开放的定制接口，可以根据自身业务系统的需要定制其监控、启停和倒换的动作，这些属于集群或高可靠系统设计和开发的范畴 了，在这之前有必要先熟悉VCS的常用操作，这在后期的维护中也是很常用的。
安装加载VCS license
vxlicinst

启动单个节点的VCS服务，如果要启动所有节点的VCS服务，或者说要启动整个集群，那么就需要到集群内各个节点上分别执行hasart
hastart

停止单个节点的业务：
hastop -local

停止整个集群的业务：
hastop -all

只关闭VCS相关进程，而不停止VCS所保护的业务进程：
hastop -local -force （单个节点）
hastop -all -force （整个集群）

查看集群状态：
hastatus
hastat -sum

查看资源状态：
hares -state

查看资源组状态：
hagrp -state

查询指定的资源组service_group状态：
hagrp -state [service_group] -sys [system]

强制VCS读取system上$VCS_CONF/conf/config目录下的配置文件进行启动集群工作：
hasys -force systemname

使配置文件可读写：
haconf -makerw

使配置文件只读：
haconf -dump -makero

查询资源resource及其相关参数（hagrp类似）：
hares -display [resource] hares -display -attribute [attribute] hares -display -group [service_group] hares -display -type [resource_type] hares -display -sys [system] 

查询主机列表：
hasys -list
hasys -display [system] haclus -value attribute
haclus –display

查询集群日志：
hamsg -list
hamsg -info [-path path_name] LDF

启动服务组及使服务联机：
hagrp -online service_group -sys system

关闭服务组及使服务脱机：
hagrp -offline service_group -sys system

将服务组切换到另一个系统上：（只对failover组有效，且当服务组中服务部分或全部联机时）：
hagrp -switch service_group -to system

冻结服务组（disable onlining, offlining, and failover)，option -persistent 是使cluster重启时freeze：
hagrp -freeze service_group [-persistent]

解冻被冻结的服务组 (reenable onlining, offlining, and failover)
hagrp -unfreeze service_group [-persistent]

激活服务组：（服务组激活后才能进行联机操作）
hagrp -enable service_group [-sys system]

禁用服务组：（服务组禁用后不能进行联机或切换操作)
hagrp -disable service_group [-sys system]

激活服务组中资源：
hagrp -enableresources service_group

禁用服务组中资源：（如资源为disable时agents不监控资源组）
hagrp -disableresources service_group

清除故障状态：
hagrp -clear [service_group] -sys [system] （资源组）
hares -clear [resource] （资源）

使资源服务启动：
hares -online resource -sys system

使资源服务停止：
hares -offline resource -sys system

在ADMIN_WAIT状态下强制主机加载集群，此命令会覆盖正在使用的集群配置，使用前请确认准备使用的主机的集群配置文件是否有效：
hacf –verify /etc/VRTSvcs/conf/concig
hasys -force system

修改主机的属性，一些属性是VCS的内部属性，不能修改：
hasys -modify modify_options

冻结主机 (防止主机进行联机或切换操作)：
hasys -freeze [-persistent] [-evacuate] system

解冻被冻结的主机 (使主机可以进行联机或切换操作)：
hasys -unfreeze [-persistent] system

管理集群：
haclus [-help [-modify]]

转载请注明：运维派 ? VCS(Veritas Cluster Server)集群维护的常用命令

