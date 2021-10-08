# 天融信防火墙配置笔记

​													by元子 2019年10月



## 目的

-  访问控制
-  NAT
-  外网过滤
-  透明网络
-  MAP



## 远程管理

1. console口配置以下命令添加管理权限

   ```pf service add name telnet area area_eth0 addressname any```

2. console口下开启Telnet

   ```system telnetd start```

3. 添加管理地址

   ```network interface eth0 ip add 192.168.1.254 mask 255.255.255.0```

4. 利用xshell或者cmd登录并管理

   ```telnet 192.168.1.254```

**SSH同理**



## 常用命令

### 系统配置命令（system）

- version 系统版本
- config reset 恢复出厂
- information 当前设备状态信息
- time 系统时钟
- config 系统配置管理
- rebot 重启
- sshd 远程
- telnetd 远程
- httpd http服务管理

### 网络配置命令（network）

- interfere 防火墙接口管理
- vlan vlan配置管理
- route 路由表配置管理
- ping 验证网络连接

### 双击热备命令（HA）

- HA LOCAL <ipaddress> 设置 HA接口的本机地址 
- HA PEER <ipaddress> 设置 HA接口的对端地址 
- HA PEER-SERIAL <string> 设置 HA接口的对端的 licence 序列号 
- HA NO <local|peer|peer-serial> 复位 HA接口的本机地址/对端地址/对端 licence序列号 
- HA PRIORITY <primary|backup> 设定 HA 优先级是主机优先还是备份机优先（默认为 backup，即如果同时启动主机成为活
- HA SHOW <cr> 查看 HA的配置信息 
- HA ENABLE<cr> 启动 HA 
- HA DISABLE<cr> 停用 HA 
- HA CLEAN<cr> 清除 HA配置信息 
- HA SYNC <from-peer|to-peer> HA同步（从对端机上同步配置/同步配置到对端机上）

### 定义对象命令（Define）

- area	区域对象管理
- interface	配置防火墙接口对应的区域属性
- host	主机地址对象管理
- range	地址范围对象管理
- -subnet	子网地址对象
- group_address	地址组对象管理
- service	子定义服务对象管理
- group_service	服务组对象管理schedule	时间表对象管理
- server	服务器对象管理
- virtual_server	虚拟服务器对象管理

### 包过滤命令（PF）

```ruby
SERVICE ADD name <gui|snmp|ssh|monitor|ping|telnet|tosids|pluto|auth|ntp|update|otp|dhcp|rip|l2tp| pptp|webui|vrc|vdc>  area  <string> <[addressid  <number>]| [addressname <addr_name>]>
```