1、TensorFlow分布式环境搭建
首先，我们配置一台有TensorFlow环境的服务器，作为 **ps0** 节点。

假设这时候，你已经有一台 **Ubuntu 16.04.2**，可以是虚拟机，也可以是阿里云服务器。

安装步骤很简单：
1.1、更新环境。使用sudo权限，为用户tensor输入密码，等待更新完成即可。
```shell
$sudo apt-get update
[sudo] password for tensor: 
Get:1 http://mirrors.aliyuncs.com/ubuntu xenial InRelease [247 kB]
Get:2 http://mirrors.aliyuncs.com/ubuntu xenial-security InRelease [102 kB]
·
·
·
Get:77 http://mirrors.aliyuncs.com/ubuntu xenial-backports/universe Translation-en [3,768 B]
Fetched 38.1 MB in 10s (3,516 kB/s)                                                                
Reading package lists... Done
```
1.2、检查 **python** 和 **pip**版本，pip需要8.5.0以上。
```shell
$ python -V
Python 2.7.12
$ pip -V
pip 9.0.1 from /usr/local/lib/python2.7/dist-packages (python 2.7)
```
1.3、安装TensorFlow。这里使用来自[清华大学开源软件镜像站](https://mirror.tuna.tsinghua.edu.cn/help/tensorflow/)的安装包进行安装。
将以下命令复制粘贴到命令框中，回车即可。（需要sudo）
```shell
sudo pip install \
  -i https://pypi.tuna.tsinghua.edu.cn/simple/ \
  https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/tensorflow-1.4.0-cp27-none-linux_x86_64.whl
```

1.4 验证是否安装成功。
```shell
$ python
Python 2.7.12 (default, Nov 19 2016, 06:48:10) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
>>> tf.__version__
'1.4.0'
>>> tf.__path__
['/usr/local/lib/python2.7/dist-packages/tensorflow']
```
可以看到第5行，tensorflow导入成功。以及查看tf的版本和安装路径。
ps0配置完成。我们还需要两台以上同样配置的工作节点：worker0和worker1。


2、目前，我们已经有了上面的ps0。（注：ps0其实相当于通常所说的master）
下一步，
如果你用的是虚拟机，现在可以复制出至少2台的worker，作为后面的工作节点。

如果你用的是云服务器，则需要重复1中的操作，搭建出另外，至少2台worker。

3、至此，我们就有了1个ps0，和2个worker0、worker1。TensorFlow分布式环境搭建完成。
也许你会疑问，难道不需要建立其他额外，诸如ssh之类的连接吗？
答案是不需要，只要虚拟机（或云服务器）可以互相 ping 的通即可。
原因是：分布式底层的通信是基于gPRC的，节点之间的通信，参数之间的更新等，TensorFlow已经都帮我们做好了。

下一节，简单分布式逻辑回归模型。
