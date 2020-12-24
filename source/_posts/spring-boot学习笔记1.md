---
title: spring-boot学习笔记1
author: yuu
avatar: 'https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/custom/avatar.jpg'
authorLink: nzynl.top
categories: 技术
comments: true
date: 2020-12-23 14:03:17
authorAbout: 生存还是死亡，这是一个值得思考的问题。
authorDesc:
tags: spring-boot
keywords:
description: spring boot学习笔记
photos: https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot学习笔记/avatar.jpg
---

# spring boot 学习笔记1

## 软件和环境

IDEA、maven、git、JDK8或以上



## 创建项目

输入包名、版本号、选择JDK版本；选择需要的类库（web、mysql、devtools等等）



## 结构

```
.
├── target
├── src
│    ├── main
│    │   ├── java                                //类存放目录
│    │   └── resources                          
│    │        ├── static                         //存放js、css、img等静态资源
│    │        ├── templates                      //模板目录
│    │        └── application.properties         //全局配置文件
│    └── test                                    //测试目录
├── target                                       //生成的代码存放目录
├── back.iml      
└── pom.xml                                      //maven配置文件
```

### Spring Boot Starter

一组被依赖的第三方类库集合

### Spring Boot Starter Parent

替子项目管理版本

### 嵌入式web容器

帮助引入tomcat



## lombok插件中的一些注解

### @Data

安装lombok插件后在module类中使用，可以帮助重写get、set等基本方法

### @Slf4j

安装lombok插件后在module类中使用，可以帮助引用生成日志的代码

### @Builder

安装lombok插件后在module类中使用，可以使用链式的方式为对象赋值，例如

```java
Test test = Test.builder()
                .name("zy")
                .age("18")
                .weight("75")
                .build();
```

### @AllArgsConstructor

安装lombok插件后在module类中使用，全参的构造函数

### @NoArgsConstructor

安装lombok插件后在module类中使用，无参的构造函数



## 其他好用的插件

### Codota

代码自动补全工具，如遇到不会写的代码可在里面查看别人在别的项目里是怎么使用的

### GsonFormat

`alt+s` 弹出的框里粘上json代码可在类中自动生成对应的java数据结构的代码

### Rainbow Brackets

彩虹括号



## RESTFUL风格和HTTP协议

- API（Application Programming Interface）编程接口规范，客户端与服务端通过请求响应进行数据通信。
- REST（Representational State Transfer）表属性状态转移，决定了接口的形式与规则。
- RESTful是基于http方法的API设计风格，不是一种新技术。

需要达到的目的：

- 看Url就知道要什么资源
- 看http method 就知道要对资源干什么
- 看http status code 就知道结果如何

![API1](https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot学习笔记/API1.jpg)



## 增删改查

- @PostMapping
- @DeleteMapping
- @PutMapping
- @GetMapping



## 传参方法

### @PathVariable

```java
@PostMapping("/calculate/{age}")
public AjaxResponse uploadData(@PathVariable("age") int age){
    log.info("Data" + age);
    return AjaxResponse.success();
}
```

![API9](https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot学习笔记/API9.jpg)

### @RequestBody

```java
@PostMapping("/calculate")
public AjaxResponse uploadData(@RequestBody Object obj){
    log.info("Data" + obj);
    return AjaxResponse.success();
}
```

![API5](https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot学习笔记/API5.jpg)

### @RequestParam

```java
@PostMapping("/calculate")
public AjaxResponse uploadData(@RequestParam String author,
                               @RequestParam int age,
                               @RequestParam String size,
                               @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
                               @RequestParam Date time){
    log.info("Data" + time);
    return AjaxResponse.success();
}
```

![API3](https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot学习笔记/API3.jpg)

### @RequestHeader

```java
@PostMapping("/calculate")
public AjaxResponse uploadData(@RequestBody String author,
                               @RequestHeader String a){
    log.info("Data" + a + author);
    return AjaxResponse.success();
}
```

![API7](https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot学习笔记/API7.jpg)

@RequestBody方式可在里面继续嵌套obj，而@RequestParam方式不行。



## Spring请求处理流程

![API10](https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot学习笔记/API10.jpg)



## @RestController 详解

@RestController相当于@Controller + @ResponseBody

@Controller注解表示类中所有的方法都是对外提供http服务的方法。

@RestController的另外一个含义是相当于在类中的所有方法中加上了@ResponseBody注解，例如

```java
@Slf4j
//@RestController
@Controller
@RequestMapping("/azimuth")
public class Azimuth {

    @GetMapping("/history")
    public @ResponseBody AjaxResponse getAzimuth(){

        return AjaxResponse.success();
    }
```

@ResponseBody注解的作用是让所有返回给前端的数据默认以json的格式响应，如果不加此注解，Spring Boot 会将返回的数据拿到templates文件夹中寻找对应的html模板进行渲染，之后返回页面。

## 数据的excel返回形式

除返回json格式的数据外，Spring Boot还可以返回其他形式的数据，下面以excel举例。首先在maven中加入依赖：

```
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>3.9</version>
</dependency>
```

新建一个转换类：

```java
@Service
public class ResponseToXlsConverter extends AbstractHttpMessageConverter<AjaxResponse> {

    /**
    * 构造函数，表示处理excel数据类型
    */
    private static final MediaType EXCEL_TYPE = MediaType.valueOf("application/vnd.ms-excel");
    ResponseToXlsConverter(){
        super(EXCEL_TYPE);
    }

    @Override
    protected boolean supports(Class<?> aClass) {
        return (AjaxResponse.class == aClass);
    }

    @Override
    protected AjaxResponse readInternal(Class<? extends AjaxResponse> aClass, HttpInputMessage httpInputMessage) throws IOException, HttpMessageNotReadableException {
        return null;
    }

    @Override
    protected void writeInternal(AjaxResponse ajaxResponse, HttpOutputMessage httpOutputMessage) throws IOException, HttpMessageNotWritableException {
        final Workbook workbook = new HSSFWorkbook();
        final Sheet sheet = workbook.createSheet();
        final Row row = sheet.createRow(0);

        row.createCell(0).setCellValue(ajaxResponse.getMessage());
        row.createCell(1).setCellValue(ajaxResponse.getData().toString());
        workbook.write(httpOutputMessage.getBody());
    }
}
```

功能是让ajaxResponse对象中的内容以excel文件形式进行返回。

## JSON数据格式的处理

- 开源的Jackson（默认使用）
- Google的Gson
- 阿里巴巴的FastJson

序列化过程性能：fastjson >= jackson > Gson, Gson在数据并发量较大的时候会与其他二者有较明显差距。

反序列化性能：三者几乎不相上下，Gson略好。

### Jackson相关注解

#### @JsonFormat

```java
@JsonFormat(pattern = "yyyy/MM/dd HH:mm:ss", timezone = "GMT+8")
```

可在特定的项目中对返回的日期格式做个性化的更改。

#### @JsonPropertyOrder

```java
@JsonPropertyOrder(value = {"content","title"})
public class Azimuth {
    public String author;
    public int age;
    public String size;
    @JsonFormat(pattern = "yyyy/MM/dd HH:mm:ss", timezone = "GMT+8")
    public Date time;
}
```

用来调整构造函数中变量的顺序

#### @JsonProperty

```java
@JsonProperty("auther")
public String author;
```

用来调整返回的字段名

#### @JsonInclude

```java
@JsonInclude(JsonInclude.Include.NON_NULL)
public String size;
```

字段如果为空则不返回。

#### @JsonIgnore

```java
@JsonIgnore
public int id;
```

字段不会返回前端

### 序列化与反序列化过程

![API11](https://cdn.jsdelivr.net/gh/yuu2b/cdn/img/blog/spring-boot学习笔记/API11.jpg)

#### 对象转Json

```java
ObjectMapper mapper = new ObjectMapper();
Azimuth azimuth = Azimuth.builder()
                         .author("zy")
                         .age("18")
                         .size("large")
                         .build();
String jsonStr = mapper.writeValueAsString(azimuth)
```

#### Json转对象

``` java
ObjectMapper mapper = new ObjectMapper();
//readValue中第一个参数就是Json,第二个参数表示用哪个类进行接收
Azimuth azimuth = mapper.readValue(content:"",Azimuth.class)
```

注意，这种情况下需要在对象中加入全参和无参的构造方法。