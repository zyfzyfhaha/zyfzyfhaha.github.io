---
title: 本地连接远程Oracle 11G
date: 2017-01-16 14:03:17

tags:

	- Oracle
	- 虚拟机

categories: Oracle

---

# 一般步骤 #

## 需要安装Oracle Client的客户端 ##
[Oracle Client 11G安装以及tnsnames.ora的操作](http://blog.csdn.net/lanchengxiaoxiao/article/details/39251947)

## 配置本机Oracle client的环境变量 ##

<!--more-->

变量名 `ORACLE_HOME`
变量值 `E:\Oracle\product\11.2.0\client_1`

变量名 `TNS_ADMIN`
变量值 `E:\Oracle\product\11.2.0\client_1\network\admin`

变量名 `NLS_LANG`
变量值 `SIMPLIFIED CHINESE_CHINA.ZHS16GBK`




# 遇到的问题 #

## 问题1 ##
用`Net Configuration Assistant`测试连接，报错为 `TNS-12541: TNS: 无监听程序：
有可能是tnsnames.ora没有配置正确`
解决方法：
``` 
本地实例名=
(DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 远程数据库IP地址)(PORT = 远程服务器端口号))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = 远程数据库服务名)
    )
  )
```
其中中文部分是需要修改的部分，除第一个“本地实例名”外，其他需要跟远程数据库管理员咨询，本地实例名就是方便自己识别数据库的一个名字，可以自定义
我的设置：
```
ROAD =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.110)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SERVICE_NAME = road)
    )
  )
```
## 问题2 ##
设置之后发现还是没用>.<
解决方法：
在linux服务器端（虚拟机图形模式）输入命令`netmgr`来查看配置服务器的listener locations和database service。发现配置中的的ip和host不一致

## 问题3 ##
改完配置之后在本地cmd中输入`tnsping road`，仍然报错为`TNS-12541: TNS:no listener`
解决方法：
发现是oracle虚拟机中监听程序没有启动，使用的命令为：

 `lsnrctl start --启动`
 `lsnrctl stop  --停止`
 `lsnrctl status --查看状态`
**（注：必须在oracle用户下执行）**

### 问题4 ##
启动成功之后显示`The command completed successfully`。用tnsping road测试也显示ping通了。之后Net Configuration Assistant测试连接，发现错误变成了
`ORA-27101 Shared memory realm does not exist`
解决方法：
查阅之后发现是没有开启oracle服务，解决办法是直接开启oracle相关数据库服务或按照下面的步骤操作:

 `lsnrctl start`
 `sqlplus '/as sysdba'`
 `sql> startup`
 `emctl start dbconsole`
 `isqlplusctl start`

 按照[这边博文](http://blog.chinaunix.net/uid-21795529-id-1815088.html)做到sql> startup这步就发现测试连接成功了，后两步没做。



# 其他 #

## [监听命令详解](http://blog.csdn.net/haiross/article/details/13629959 ) ##
 

## PL SQL设置 ##
个人下的是绿色版，tools-preferences配置为：
注意：登陆PL SQL前需要在远程服务器（虚拟机）上创建登陆的用户和表空间，然后用新建的用户登陆。
[这篇博文有详细操作](http://www.cnblogs.com/furenjian/articles/2889787.html)


