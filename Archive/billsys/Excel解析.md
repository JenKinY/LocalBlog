

解析映射规则主要在`EMapper.xml`文件中的 `<rule>`标签下。

用于将一些特定的 excel 数据映射

以判断单据类型的rule为例：

```xml
<!-- 单据类型 -->
<rule id="bill_type">
		<if item="单据类型" opera="=" value="员工借款单" source="excel">
			<return value="1"/>
		</if>
        ...
```

```xml
其中：
item:名称
opera: 逻辑运算符
value: excel表格中的原始数据
source: 源文件类型
return.value: 映射结果
```

例如原始数据表中，员工借款单在数据库里的映射就是1

![image-20210305103411400](image-20210305103411400.png)

![image-20210305103350951](image-20210305103350951.png)

页面展示时前端也会处理：

![image-20210305103605040](image-20210305103605040.png)