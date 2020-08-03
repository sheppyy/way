

MySQL5.7在网上可以很容易找到，要是找不到合适的，我给你整了一个，[点这里](链接：https://pan.baidu.com/s/1DtXnNx86UdTkOWFb0LlXIg)。

提取码：tjua 



**安装步骤：**

​	安装路径自己选择，一般不安装在系统盘就成。

​	1、双击mysql-installer-community-5.7.17.0.msi开始安装

```
1）勾选同意协议
2）之后的都是默认，到Installation Progress后，点击Execute
3）之后的都是默认，到设置root密码，自行设置
4）之后的都是默认，到Apply Server Configuration，点击Execute
5）最后finish即可完成安装
6）安装途中出现的提示框都点确定
```

2、验证mysql服务器是否已经成功安装

- 方法一：进入服务列表，看服务器是否已经启动

  在开始搜索那里输入：services.msc。快速加入服务列表

  

- 方法二：在cmd输入net start mysql57看服务器是否正常启动 



- 方法三：在开始文件列表，找到MySQL文件夹，进入MySQL Server 5.7启动第一个，看是否能够正常操作