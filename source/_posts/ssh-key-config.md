date: 2014-04-25 16:20:15
title: "SSH通过密钥访问远程机器"
categories: shell
tags: [shell,ssh,mac]
---

以下描述的步骤是通过mac访问远程Linux服务器

####生成ssh密钥文件
在~/.ssh中生成相关密钥文件

```
cd ~/.ssh
ssh-keygen -t rsa -C "xxxxx@xxx.com"

```
**-t** 后面参数为ssh加密类型，这里是**RSA**类型  
**-C** 后面为comments  

提示输入文件名和密码，密码可以为空。运行完后在.ssh目录下生成了2个文件，xxx和xxx.pub,后缀为.pub的文件为公有密钥文件，另外一个为私有文件。

####加入ssh密钥文件
通过命令**ssh-add**加入刚刚生成的私有密钥文件到ssh-agent

```
ssh-add xxx

```

####加入公有密钥到远程客服端
通过命令**scp**上传.pub文件到远程客服端**.ssh**目录

```
scp ~/.ssh/xxx.pub  user@192.168.1.110:~/.ssh 

```
登入远程客服端,然后把密钥内容加入到**authorized_keys**文件

```
ssh user@192.168.1.110
cd ~/.ssh
cat xxx.pub >> authorized_keys
rm  xxx.pub

```

修改 ssh 的配置文件（默认是在 etc/ssh/sshd_config）, **PubkeyAuthentication** 设置成 **yes**。这样完成后可以直接这样访问，不用再输入密码

```
ssh user@192.168.1.110

```
我们还可以在本地config文件中进行配置，让访问变的更简单

####配置本地ssh的config文件
配置.ssh/config文件，加入以下内容
```
Host localserver  
     HostName 192.168.1.110  
     User uname  
     IdentityFile ~/.ssh/xxx 
      
```

localserver为host的别名，可以随意取。IdentityFile指定生成在本地的私有密钥文件。这样就完成了配置,以后我们可以直接这样访问,不需要输入用户名和密码

```
 ssh localserver
 
```


