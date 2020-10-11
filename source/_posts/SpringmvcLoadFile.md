---
title: SpringMVC+JSP实现文件上传
date: 2020-06-10 15:31:36
cover: true
tags: 
    - SpringMVC
    - java
categories: SSM
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/journal-2850091_1920.jpg
summary: 本篇文章以一个点餐系统添加新菜品为例来讲解文件上传，图片上传功能。
---

> eclipse以一个点餐系统添加新菜品为例来讲解文件上传，图片上传功能。

### 导入需要的jar包

1、commons-fileupload-1.2.2.jar
2、commons-io-2.0.1.jar

### 在springmvc.xml的配置文件中配置

```xml
<!-- 文件上传 --> 
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 上传文件大小限制 --> 
    <property name="maxUploadSize"> 
        <value>10485760</value> 
    </property> 
    <!-- 请求的编码格式, 和 jsp 页面一致 --> 
   <property name="defaultEncoding"> 
       <value>UTF-8</value> 
    </property> 
</bean>
```

### JSP页面代码

这里只是表单提交部分，注意`enctype="multipart/form-data"`以及图片的`input`标签`type="file"`，jsp中 input标签当type=”file”的时候，浏览器会把文件的内容连同form的所有字段格式化后传递到服务器。

```html
<form action="${pageContext.request.contextPath}/FoodAddDo.do" method="post" enctype="multipart/form-data"> 
    <div class="input-group input-group-lg" style="margin-top: 20px;"> <div class="input-group-prepend"> 
        <span class="input-group-text" id="inputGroup-sizing-lg">菜品名称</span> 
        </div> 
        <input type="text" name="foodname" class="form-control" aria-label="Sizing example input" aria-describedby="inputGroup-sizing-lg"> </div> 
    <div class="input-group input-group-lg" style="margin-top: 20px;">
        <div class="input-group-prepend"> 
            <span class="input-group-text" id="inputGroup-sizing-lg">特色</span>
        </div>
        <input type="text" name="feature" class="form-control" aria-label="Sizing example input" aria-describedby="inputGroup-sizing-lg"> 
    </div>
    <div class="input-group input-group-lg" style="margin-top: 20px;">
        <div class="input-group-prepend"> 
            <span class="input-group-text" id="inputGroup-sizing-lg">食材</span>            </div>
        <input type="text" name="material" class="form-control" aria-label="Sizing example input" aria-describedby="inputGroup-sizing-lg">
    </div> 
    <div class="input-group input-group-lg" style="margin-top: 20px;"> <div class="input-group-prepend"> 
        <span class="input-group-text" id="inputGroup-sizing-lg">价格/元</span> 
        </div> 
        <input type="text" name="price" class="form-control" aria-label="Sizing example input" aria-describedby="inputGroup-sizing-lg">
       </div> 
       <div class="input-group mb-4" style="margin-top: 20px;"> 
     <div class="input-group-prepend"> 
        <label class="input-group-text" for="inputGroupSelect01">分类</label> 
        </div> 
        <select class="custom-select" name="type"> 
            <option value="1">家常</option>
            <option value="2">凉菜</option> 
            <option value="3">主食</option> 
            <option value="4">饮品</option> 
        </select> </div> 
    <div class="input-group input-group-lg" style="margin-top: 20px;">
        <div class="input-group-prepend"> 
            <span class="input-group-text" id="inputGroup-sizing-lg">图片</span>              </div>
        <input type="file" name="pitcture" id="pitcture" class="form-control" aria-label="Sizing example input" aria-describedby="inputGroup-sizing-lg"> 
      </div> 
    <div style="margin-top: 20px;"> 
        <input type="submit" class="btn btn-success" value="确认添加" style="margin-right: 20px;"> 
        <input type="reset" class="btn btn-primary" value="重置">
    </div> 
</form>
```

### Controller部分

用MultipartFile对象接收文件，然后使用file的transferTo方法上传文件。

```java
@RequestMapping(value = "/FoodAddDo", method = RequestMethod.POST) 
public ModelAndView foodAddDo( 
    @RequestParam(name = "pitcture") MultipartFile pitcture, 
    @RequestParam(name = "foodname", required = true) String foodname,              
    @RequestParam(name = "feature", required = true) String feature, 
    @RequestParam(name = "material", required = true) String material, 
    @RequestParam(name = "price", required = true) int price, 
    @RequestParam(name = "type", required = true) int type, ModelAndView model, HttpServletRequest request) throws IllegalStateException, IOException, SQLException { 
    String picturePath = ""; 
    String ext = FilenameUtils.getExtension(pitcture.getOriginalFilename()); 
    // System.out.println("isEmpty："+pitcture.isEmpty()); 
    String oriPitctureNameString = pitcture.getOriginalFilename();// 获取后缀名
    String extPitctureNameString = oriPitctureNameString.substring(oriPitctureNameString.indexOf("."), oriPitctureNameString.length());// 加上点儿 
    picturePath = "images/newfood/" + Calendar.getInstance().getTimeInMillis() + extPitctureNameString;// 以时间点命名保证文件名称的唯一性 
    // System.out.println("picturePath:"+picturePath); 
    // picturePath:/images/newfood/...... 
    String pathString = request.getServletContext().getRealPath(picturePath); 
    // System.out.println("pathString:"+pathString); 
    // 这是上传后我们在Tomcate找图片的路径，pathString:D:\Program 
    // Files\apache-tomcat-9.0.5\apache-tomcat-9.0.5\wtpwebapps\zhouhailin0506_MealSystem\images\newfood\...... 
    File file = new File(pathString); if (pitcture != null) { 
        //文件上传 pitcture.transferTo(file);
    } 
    //写入数据库，注意存入数据库的是字符串新式的路径，图片文件在pathString中 
    if (fooddao.addFood(foodname, feature, material, price, type, picturePath)) {
        model.setViewName("admin/food_list"); 
    }
    return model;
}
```

### IDEA

maven项目导入依赖

```xml
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
    </dependency>

    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.3</version>
    </dependency>
```

upload.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>图片上传</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">
</head>
<body>
<h1>图片上传</h1>

<div class="container">
    <!-- Content here -->

<form action="${pageContext.request.contextPath}/doupload" method="post" enctype="multipart/form-data">
    <div class="form-group">
        <label for="exampleInputEmail1">ID</label>
        <input  name="id"  type="text" class="form-control"  aria-describedby="emailHelp">
        <small id="" class="form-text text-muted">请输入id</small>
    </div>
    <div class="form-group">
        <label for="exampleInputEmail1">姓名</label>
        <input  name="name"  type="text" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
        <small id="emailHelp" class="form-text text-muted">请输入姓名</small>
    </div>
    <div class="form-group">
        <label for="exampleInputPassword1">年龄</label>
        <input name="age" type="text" class="form-control" id="exampleInputPassword1">
    </div>

    <div class="input-group input-group-lg" style="margin-top: 20px;">
        <div class="input-group-prepend">
            <span class="input-group-text" id="inputGroup-sizing-lg">图片</span>
        </div>
        <input type="file" name="pitcture" id="pitcture" class="form-control"
               aria-label="Sizing example input"
               aria-describedby="inputGroup-sizing-lg">
    </div>

    <div class="form-group form-check">
        <input type="checkbox" class="form-check-input" id="exampleCheck1">
        <label class="form-check-label" for="exampleCheck1">检查</label>
    </div>
    <button type="submit" class="btn btn-primary">确认</button>
    <input type="reset" class="btn btn-primary" value="重置">
</form>

</div>

</body>
</html>
```

see_infor.jsp

> 注意这里第24行的请求地址

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>表单查询</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">
</head>
<body>

<h1>展示账户数据列表</h1>
<table class="table">
    <tr>
        <th scope="col">学生id</th>
        <th scope="col">学生姓名</th>
        <th scope="col">学生年龄</th>
        <th scope="col">图片</th>
        <th scope="col">操作</th>
    </tr>

    <c:forEach items="${student}" var="student">
    <tr>
        <td>${student.id}</td>
        <td>${student.name}</td>
        <td>${student.age}</td>
        <td><img src="${pageContext.request.contextPath}/studentPhoto?id=${student.id}&photoPath=${student.photoPath}" width="200px" height="200px" alt="..." class="img-thumbnail"></td>
        <td>删除。修改。</td>
    </tr>
    </c:forEach>

</body>
</html>
```

Student

```java
public class Student {

    private  Integer id;
    private  String name;
    private  int age;
    private  String photoPath;

    @Override
    public String toString() {
        return "Studnet{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", photoPath='" + photoPath + '\'' +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getPhotoPath() {
        return photoPath;
    }

    public void setPhotoPath(String photoPath) {
        this.photoPath = photoPath;
    }
}
```

Controller

```java
import java.io.*;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.MultipartFile;
import org.springframework.web.servlet.ModelAndView;
import zhouhailini506.pojo.Student;

import javax.servlet.ServletContext;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@org.springframework.stereotype.Controller
public class Controller {

    @RequestMapping(value = "/upload")
    public ModelAndView uploadPhoto(ModelAndView model) {
        System.out.println("uploadPhoto get in");
        model.setViewName("upload");
        return model;
    }

    @RequestMapping(value = "/doupload", method = RequestMethod.POST)
    public ModelAndView doupload(@RequestParam(name = "pitcture") MultipartFile uploadfile,
                                 @RequestParam(name = "id", required = true) int id,
                                 @RequestParam(name = "name", required = true) String name,
                                 @RequestParam(name = "age", required = true) int age,
                                 ModelAndView model, HttpServletRequest request) throws IOException {
        System.out.println("doupload mapping get in");
        Student student=new Student();
        student.setId(id);
        student.setName(name);
        student.setAge(age);

        ServletContext application=request.getServletContext();
        String realPath=application.getRealPath("upload/");
        int index=uploadfile.getOriginalFilename().lastIndexOf(".");
        String suffix=uploadfile.getOriginalFilename().substring(index+1);
        String fileName=realPath+File.separator+id+"."+suffix;
        uploadfile.transferTo(new File((fileName)));

        student.setPhotoPath(suffix);
        System.out.println("student=>"+student);

        model.addObject("student",student);
        model.setViewName("see_infor");
        return  model;
    }

    @RequestMapping("/studentPhoto")
    public void studentPhoto(String id, String photoPath, HttpServletRequest
                             request, HttpServletResponse response) throws IOException {

        ServletContext application =request.getServletContext();
        String realPath=application.getRealPath("upload/");
        String fileName=realPath+File.separator+id+"."+photoPath;
        File file=new File(fileName);
        if (file.exists()){
            byte[] buffer=new byte[1024];
            FileInputStream fis=null;
            BufferedInputStream bis=null;
            fis=new FileInputStream(file);
            bis=new BufferedInputStream(fis);
            OutputStream os=response.getOutputStream();
            int i=bis.read(buffer);
            while (i!=-1){
                os.write(buffer,0,i);
                i=bis.read(buffer);
            }
        }
    }
}
```

