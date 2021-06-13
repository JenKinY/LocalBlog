> 参考：https://blog.csdn.net/shenshendeai/article/details/49794699

### **export命令**

**功能说明：**设置或显示环境变量。

**语　　法：**export \[-fnp][变量名称]=[变量设置值]

**补充说明：**在shell中执行程序时，shell会提供一组环境变量。 export可新增，修改或删除环境变量，供后续执行的程序使用。

export的效力仅及于该此登陆操作。

**参　　数：**

  -f 　代表[变量名称]中为函数名称。

　 -n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。

　 -p 　列出所有的shell赋予程序的环境变量。

   一个变量创建时，它不会自动地为在它之后创建的shell进程所知。而命令export可以向后面的shell传递变量的值。当一个shell脚本调用并执行时，它不会自动得到原来脚本（调用者）里定义的变量的访问权，除非这些变量已经被显式地设置为可用。

   export命令可以用于传递一个或多个变量的值到任何后继脚本。

 

### **在 linux 里设置环境变量的三种实现方法（export PATH）：**

#### **1.直接使用 export 命令 （我们以 mysql 服务举例说明）**

[root@liyao ~]# export PATH=$PATH:/usr/local/mysql/bin

查看是否已经设置好，可以使用命令 export 命令来查看，也可以直接$#变量名#来查看

zhongweichaomatoMacBook-Pro:~ zhongweichao$ $PATH

-bash: :/Users/zhongweichao/.local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/X11/bin:/Users/zhongweichao/Develop/jboss-5.1.0.GA/bin

需要注意： 直接使用 export 设置的变量都是**临时变量**，也就是说退出当前的 shell ，为该变量定义的值便不会生效了。如何能让我们定义的变量永久生效呢？那就看我们的第二种定义的方式。

#### **2. 修改 /etc/profile**

[root@liyao ~]# vi /etc/profile

export PATH=$PATH:/usr/local/mysql/bin # 在配置文件中加入此行配置

需要注意的是：修改完这个文件必须要使用 以下命令在不用重启系统的情况下使修改的内容生效。

[root@liyao ~]# source /etc/profile

或者是用 ‘.’：

[root@liyao ~]# . /etc/profile

查看：

[root@liyao ~]# echo $PATH

/usr/kerberos/sbin:/usr/kerberos/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin:/usr/local/mysql/bin

\# 配置已经生效

#### **3. 修改 .bashrc 文件是在当前用户 shell 下生效**

\# vi /root/.bashrc?在里面加入：

export PATH=$PATH:/usr/local/mysql/bin

修改这个文件之后同样也需要使用 source 或者是 . 使配置文件生效。

再来使用 echo $PATH看下变量是否生效

[root@liyao ~]# echo $PATH

/usr/kerberos/sbin:/usr/kerberos/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin:/usr/local/mysql/bin

###  

### **shell与export命令**

   用户登录到Linux系统后，系统将启动一个用户shell。在这个shell中，可以使用shell命令或声明变量，也可以创建并运行 shell脚本程序。运行shell脚本程序时，系统将创建一个子shell。此时，系统中将有两个shell，一个是登录时系统启动的shell，另一 个是系统为运行脚本程序创建的shell。当一个脚本程序运行完毕，它的脚本shell将终止，可以返回到执行该脚本之前的shell。从这种意义上来 说，用户可以有许多 shell，每个shell都是由某个shell（称为父shell）派生的。 

   在子 shell中定义的变量只在该子shell内有效。如果在一个shell脚本程序中定义了一个变量，当该脚本程序运行时，这个定义的变量只是该脚本程序内 的一个局部变量，其他的shell不能引用它，要使某个变量的值可以在其他shell中被改变，可以使用export命令对已定义的变量进行输出。 export命令将使系统在创建每一个新的shell时定义这个变量的一个拷贝。这个过程称之为变量输出。