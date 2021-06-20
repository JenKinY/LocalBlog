## windows下每次git push都需要输入账号密码

> 原文链接：https://blog.csdn.net/adorable_/article/details/111080883

1、通过"github -> account -> settings -> Developer settings -> Personal access tokens"处，点击Generate new token。

![](Pasted%20image%2020210619112050.png)

2、因为只是需要git push之类的操作，所以勾选repo选项，即可。

![](Pasted%20image%2020210619112632.png)

复制生成的token。

3、随后token生成成功，然后再在本地git bash中进行git push，账号还是原来的github账号，密码改为填写刚刚生成的token即可。之后就不会再重复输入密码了。

4、之后可以到"控制面板 -> 用户账户 -> 凭据管理器 -> Windows凭据"下查看，可以发现系统中已经保存了新的GitHub的token，此后就可以安全的使用了。

