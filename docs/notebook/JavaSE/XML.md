### XML(可扩展标记语言 EXtensible Markup Language)

>独立于软件和硬件的信息传输工具，XML应用于web开发的许多方面，常用于简化数据的储存和共享

**用途**

1.XML简化数据共享
2.XML简化数据传输
3.XML简化平台的变更

例:
```
<?xml version="1.0" encoding="UTF-8"?>
<list>
	<emp id="1">
		<name>张三</name>
		<age>20</age>
		<gender>男</gender>
		<salary>5000</salary>
	</emp>
</list>
```

## CDATA段

>不由XML解析器解析

```
<![CDATA[文本内容]]>
```

##### 解析xml导入dom4j包

>新建maven项目在pom.xml里配置

最后面添加上maven依赖
```
<dependencies>
    <dependency>
        <groupId>dom4j</groupId>
        <artifactId>dom4j</artifactId>
        <version>1.6.1</version>
    </dependency>
</dependencies>
```

##### 阿里云jar组件"坐标"搜索

>https://maven.aliyun.com/mvn/search

##### DOM4J解析XML文档的大致流程

1:创建SAXReader
2:使用SAXReader读取要解析的XML文档使其生成一个Document对象，该对象保存着该XML文档的表示的整棵树。
3:通过Document获取根元素
4:按照XML文档的树结构从根元素开始逐级获取子元素以达到遍历XML文档的目的 

## Document read();

>读取xml文件

例:
```
SAXReader reader = new SAXReader();
Document doc = reader.read(new File("emplist.xml"));
```

## Element getRootElement()

>获取根元素(根标签)的方法

例:
```
Element root = doc.getRootElement();
```

## Element

>该类的每一个实例用于表示一个XML文档中的一对标签，其提供了用于获取该标签信息的一组方法

例:
```
//String getName()  获取当前标签的名字
root.getName();

//String getText()  获取当前标签中的文本(开始与结束标签之间)
root.getText();

//Element element(String name)  获取当前标签下指定名字的子标签
Element ele = root.element("emp");

//List elements(String name)  获取当前标签下所有同名子标签(指定的名字)
List<Element> List =  root.elements("emp");

//List elements()  获取当前标签下的所有子标签
List<Element> List =  root.elements();

//获取emp标签的id的值
for(Element empEle : list) {
	empEle.attribute("id").getValue();
    或
    empEle.attributeValue("id")；
}
```

## 使用dom4j生成一个XML文件

1:创建一个**Document对象**，表示一个空白文档
2:向Document中添加根元素
3:按照需要的树结构从根元素开始逐级添加子元素。
4:创建XmlWriter
5:使用XmlWriter将Document对象写出来生成XML文档
6:关闭XmlWriter  

##### Element addElement(String name)

>Document提供了添加根元素的方法，返回值为添加的根元素，以便继续向根元素中追加内容。

**注意**：该方法只能调用一次，因为一个文档只能有一个根元素

例:
```
Document doc = DocumentHelper.createDocument();
Element root = doc.addElement("list");

Element empEle = root.addElement("emp");
empEle.addAttribute("id",6+"");
empEle.addElement("name").addText("张三");

try {
	//生成xml
    XMLWriter writer = new XMLWriter(
		new FileOutputStream("myemp.xml"),
        OutputFormat.createPrettyPrint()  //文件以整理好的格式输出
    );
    writer.write(doc);
    System.out.println("写出完毕");
    writer.close();
    } catch (Exception e) {
    e.printStackTrace();
}
```















