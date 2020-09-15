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

> 本篇文章以一个点餐系统添加新菜品为例来讲解文件上传，图片上传功能。

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

