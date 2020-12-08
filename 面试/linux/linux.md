linux

## 常用命令（重要）

ls/ll、cd、mkdir、rm-rf、cp、mv、ps -ef | grep xxx、kill、free-m、tar -xvf file.tar 

**查看进程：**（例：如何查看所有xx进程）

　ps -ef | grep xxx

​	ps -aux | grep xxx（-aux显示所有状态）



**编辑 vi/vim ： **

**vi x.log**  编辑你的日志文件

i  写入

:wq 保存退出  

## top⭐

显示系统中各个进程的资源占用状况，可以看是否有 CPU 占用过大的进程。



## tail⭐

**查看日志：**

tail -f  *.log ： 适用于实时查看日志,开发环境还行，生产就算了，日志会很多。

**tail -f error.log**  ：生产中一般用这个实时看异常日志

**-f ：循环读取 ，用于查阅正在改变的日志文件。** 

## netstat⭐

用于显示网络状态。

## grep 查找⭐

**grep 是必备日志分析命令**

**grep -r '关键字如商品ID' \*.log （使用频率最高）** 

**grep '关键字如商品ID' \*.log | grep 免费商品（在管道符前条件结果中，在加条件筛选下) **

**grep '关键字如商品ID' \*.log >> anan.txt 【相关日志输入到一个txt中，下载到本地慢慢看，我最喜欢】

## 对文件内容做统计 awk ⭐

依次处理文件的每一行，并读取里面的每一个字段，可用作统计。

$ awk 动作 文件名 

## 批量替换 sed

sed 配合正则表达式批量替换文本内容

## 你经常使用哪些 Linux 命令，主要用来解决什么问题？

 