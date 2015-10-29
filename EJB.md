# JavaBean

## 概要
JavaBean是特殊的Java类，使用J ava语言书写，并且遵守JavaBeans API规范。</br>
接下来给出的是JavaBean与其它Java类相比而言独一无二的特征：</br>
* 提供一个默认的无参构造函数。
* 需要被序列化并且实现了Serializable接口。
* 可能有一系列可读写属性。
* 可能有一系列的"getter"或"setter"方法。
## JavaBeans属性
一个JavaBean对象的属性应该是可访问的。这个属性可以是任意合法的Java数据类型，包括自定义Java类。</br>
一个JavaBean对象的属性可以是可读写，或只读，或只写。JavaBean对象的属性通过JavaBean实现类中提供的两个方法来访问：</br>
<table>
<tr><td>方法</td><td>描述<td></tr>
<tr><td>getPropertyName()</td><td>举例来说，如果属性的名称为myName，那么这个方法的名字就要写成getMyName()来读取这个属性。这个方法也称为访问器。</td></tr>
<tr><td>setPropertyName()</td><td>	举例来说，如果属性的名称为myName，那么这个方法的名字就要写成setMyName()来写入这个属性。这个方法也称为写入器。</td></tr>
</table>
**一个只读的属性只提供getPropertyName()方法，一个只写的属性只提供setPropertyName()方法。**
### JavaBeans程序示例
这是StudentBean.java文件：

      package com.tutorialspoint;
      public class StudentsBean implements java.io.Serializable
      {
       private String firstName = null;
       private String lastName = null;
       private int age = 0;

       public StudentsBean() {
       }
       public String getFirstName(){
          return firstName;
       }
       public String getLastName(){
          return lastName;
       }
       public int getAge(){
          return age;
       }
       public void setFirstName(String firstName){
          this.firstName = firstName;
       }
       public void setLastName(String lastName){
          this.lastName = lastName;
       }
       public void setAge(Integer age){
          this.age = age;
       }
      }


## 访问JavaBeans
<jsp:useBean>标签可以在JSP中声明一个JavaBean，然后使用。声明后，JavaBean对象就成了脚本变量，</br>
可以通过脚本元素或其他自定义标签来访问。<jsp:useBean>标签的语法格式如下：</br>

    <jsp:useBean id="bean's name" scope="bean's scope" typeSpec/>

其中，根据具体情况，scope的值可以是page，request，session或application。</br>
id值可任意只要不和同一JSP文件中其它<jsp:useBean>中id值一样就行了。</br>
接下来给出的是<jsp:useBean>标签的一个简单的用法：</br>

    <html>
    <head>
    <title>useBean Example</title>
    </head>
    <body>

    <jsp:useBean id="date" class="java.util.Date" />
    <p>The date/time is <%= date %>

    </body>
    </html>

它将会产生如下结果：</br>
The date/time is Thu Sep 30 11:18:11 GST 2013
## 访问JavaBeans对象的属性
在<jsp:useBean>标签主体中使用<jsp:getProperty/>标签来调用getter方法，</br>
使用<jsp:setProperty/>标签来调用setter方法，语法格式如下：

    <jsp:useBean id="id" class="bean's class" scope="bean's scope">
       <jsp:setProperty name="bean's id" property="property name"  
                        value="value"/>
       <jsp:getProperty name="bean's id" property="property name"/>
       ...........
    </jsp:useBean>

name属性指的是Bean的id属性。property属性指的是想要调用的getter或setter方法。
</br>
接下来给出使用以上语法进行属性访问的一个简单例子：

    <html>
    <head>
    <title>get and set properties Example</title>
    </head>
    <body>

    <jsp:useBean id="students"
                        class="com.tutorialspoint.StudentsBean">
       <jsp:setProperty name="students" property="firstName"
                        value="Zara"/>
       <jsp:setProperty name="students" property="lastName"
                        value="Ali"/>
       <jsp:setProperty name="students" property="age"
                        value="10"/>
    </jsp:useBean>

    <p>Student First Name:
       <jsp:getProperty name="students" property="firstName"/>
    </p>
    <p>Student Last Name:
       <jsp:getProperty name="students" property="lastName"/>
    </p>
    <p>Student Age:
       <jsp:getProperty name="students" property="age"/>
    </p>

    </body>
    </html>
