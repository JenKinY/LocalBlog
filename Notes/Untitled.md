有时候把项目上传到 GitHub/Gitee 平台时，可能项目的类型不能够正确显示，比如上传了一个 Flask 的项目，项目语言却被显示成了 HTML。

因此需要强制修改一下显示的语言：

1. 在根目录新建一个文件 `gitattributes`，然后在其中输入：

```xml
*.文件拓展名 linguist-language=编程语言 
例如，把.html 文件看作 Python 语言。
*.html linguist-language=Python
```

原理就是把某些文件统一在 Git 中视为另一种文件