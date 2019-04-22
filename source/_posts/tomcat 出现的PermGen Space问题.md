---
title: tomcat 出现的PermGen Space问题
date: 2019-03-05 22:05:24
tags: Tomcat
declare: true
---
> 记录一下以前遇到的问题

问题： 
tomcat服务器运行一段时间，总是会自动报异常：java.lang.OutOfmemoryError: PermGen Space 的错误，导致项目无法正常运行。
介绍： 
PermGen Space：指的是内存的永久保存区，该块内存主要是被JVM用来存放 class 和 mete 信息的，当 class 被加载 loader 的时候就会被存储到该内存区中，与存放类的实例的heap区不同，java中的 垃圾回收器GC 不会在主程序运行期对 PermGen space 进行清理。
原因：
  1. 当我们的应用中有很多的class时，很可能就会出现PermGen space的错误。
  2. 我们的 tomcat 在重启的时候，不是使用的 ./bin/shutdown.sh 而是使用 kill -9 xxx 直接杀掉，这样的话，存在 PermGen space 里面的内存不会被释放的，这样多长进行 kill 之后，就会导致系统的内存被渐渐吃完了，直到最后 tomcat 报错。

 <!--more-->
解决方法：
## Windows下 ##
手动设置MaxPermSize的大小
修改 TOMCAT_HOME/bin/catalina.bat文件
在echo "using CATALINA_BASE：$CATALINA_BASE"上面加入这一行内容：

``set JAVA_OPTS=%JAVA_OPTS% -server -XX:PermSize=128m -XX:MaxPermSize=512m``

如果是服务模式启动的，在tomcat7w的Java栏里填入：
``-XX:MaxPermSize=512m``

## Linux下 ##
如果是 linux 环境，则修改 TOMCAT_HOME/bin/catalina.sh：

``JAVA_OPTS="$JAVA_OPTS" -server -XX:PermSize=128m -XX:MaxSize=512m``


  1. 在关闭重启 tomcat 的过程中使用 shutdown.sh 而不是 使用 kill -9
  2. 如果使用 shutdown.sh 不能将 tomcat 关掉的话，就必须要使用 kill -9 来关闭了，这个时候只有手动的来回收垃圾了： 在 linux 命令下执行如下的命令，把 缓存给丢弃掉。
echo 3 > /proc/sys/vm/drop_caches 