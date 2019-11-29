## 一、使用JSP 内置的 request 对象

```jsp
<%
String path = request.getContextPath(); 
String basePath = request.getScheme()+”://”+request.getServerName()+”:”+request.getServerPort()+path+”/”;
%>
```

最后的 basePath 获取的 该请求所对应`项目的根目录路径`。例如：

```java
http://localhost:8080/skyzc_project/
```

因此，如果我们要获取项目路径下的一个静态文件，就可以使用 JSP 表达式`<%=basepath%>`参考 basePath 来实现：

```jsp
<img src="<%=basePath%>static/images/myPhoto.jpg"></img>
```

## 二、使用 EL 表达式

```jsp
${pageContext.request.getContextPath}
```

该表达式会`直接输出项目路径`。例如：/skyzc_project

因此，如果我们要获取项目路径下的一个静态文件，就可以使用 EL 表达式`${pageContext.request.getContextPath}`来读取：

```jsp
<img src="${pageContext.request.getContextPath()}/static/images/myPhoto.jpg" alt="">
```



**tip**: 

在使用IDEA开发的过程中，有可能 EL 表达式会直接被解析成字符串输出，可以在 page 指令中添加一条属性：

```jsp
<%@ page ... isELIgnored="false" %>
```

 `＜%@ page isELIgnored＝"true|false"%＞` 如果设定为真，那么JSP中的表达式被当成字符串处理。而isELIgnored＝"false"时才会解析 EL 表达式。Web容器默认isELIgnored＝"false"。 