# H3C配置命令

by元子

### Telnet

```ruby
[SW2]local-user yuanzi class manage
[SW2-luser-manage-yuanzi]password simple yuanzi
[SW2-luser-manage-yuanzi]service-type telnet
[SW2-luser-manage-yuanzi]authorization-attribute user-role level-15
[SW2]user-interface vty 0 4
[SW2-line-vty0-4]authentication-mode scheme #AAA验证
[SW2-line-vty0-4]set authentication password simple ruijie #设置明文验证
[SW2-line-vty0-4]user-role level-15
```

- 配置密文直接passwd后回车（看到教程配置有chiper  我没有不知道为什么）

- H3C交换机super命令不能简单的看作思科的enable，它的口令用于低级别用户向高级别用户切换时进行验证，类似于UNIX系统和Linux系统中从普通用户转换到root帐户时须输入root密码进行切换。◆console访问方式：需要在 user-interface aux 0模式下配置authentication-mode。

  ◆telnet访问方式：需要在 user-interface vty 0 4模式下配置authentication-mode。

  ◆web访问方式：需要启用web管理服务，并要有local-user配置的用户和密码。

### 链路聚合

```ruby
[SW2]interface Bridge-Aggregation 666
[SW2-GigabitEthernet1/0/1]port link-aggregation group 666
```

### MSTP



