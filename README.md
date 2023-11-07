坑 —— ssh如何使用私钥登录服务器？
这个坑不属于 actions，折腾了很久是因为easingthemes/ssh-deploy无法连接服务器。
制作密钥对
登录服务器
复制代码[root@host ~]$ ssh-keygen  
一路回车，会在root用户的家目录中生成一个.ssh的隐藏目录。内含两个密钥文件。id_rsa 为私钥，id_rsa.pub 为公钥。
在服务器上安装公钥
复制代码[root@host ~]$ cd .ssh
[root@host .ssh]$ cat id_rsa.pub >> authorized_keys
为了确保连接成功，请保证以下文件权限正确:
复制代码[root@host .ssh]$ chmod 600 authorized_keys
[root@host .ssh]$ chmod 700 ~/.ssh
设置 SSH，打开密钥登录功能
编辑 /etc/ssh/sshd_config 文件，进行如下设置：
复制代码RSAAuthentication yes
PubkeyAuthentication yes

PermitRootLogin yes
重启SSH服务
复制代码[root@host .ssh]$ service sshd restart
复制私钥到github
复制代码[root@host .ssh]$ cat id_rsa
复制文件，注意：要复制全，包括上下的 -------- BEGIN .... -----------。
点击进入项目设置：

进入左侧的Secrets

添加一个 secret
name随意设置，需要和上面设置${{ secrets.TOKEN }}对应上，value就可以将上面复制的私钥放进去，现在就可以正确的连接服务器了。

作者：Lightbringer
链接：https://juejin.cn/post/6844904070705053709
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
 
 
