## （一）概述
在平常的工作中，经常会遇到要写接口的情况，现在最常用的就是http接口，今天我就介绍一下如何去写一个规范的http接口。

## （二）搭建项目
首先我们先搭建一个SpringBoot项目，如何搭建这里就不讲了，引入相关的依赖：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
## （三）写一个通用结果对象
一个接口的返回信息应该至少包含以下几项：

1、结果编码

2、结果信息

3、返回数据

因此新建一个类来记录返回的结果集Result ：

```
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Result {
    private int code;
    private String message;
    private Object data;
}
```
其中结果编码和结果信息需要是统一的，这里用枚举类型最合适，新建一个枚举类ResponseCode

```
public enum ResponseCode {

    // 系统模块
    SUCCESS(0, "操作成功"),
    ERROR(1, "操作失败"),
    SERVER_ERROR(500, "服务器异常"),

    // 通用模块 1xxxx
    ILLEGAL_ARGUMENT(10000, "参数不合法"),
    REPETITIVE_OPERATION(10001, "请勿重复操作"),
    ACCESS_LIMIT(10002, "请求太频繁, 请稍后再试"),
    MAIL_SEND_SUCCESS(10003, "邮件发送成功"),

    // 用户模块 2xxxx
    NEED_LOGIN(20001, "登录失效"),
    USERNAME_OR_PASSWORD_EMPTY(20002, "用户名或密码不能为空"),
    USERNAME_OR_PASSWORD_WRONG(20003, "用户名或密码错误"),
    USER_NOT_EXISTS(20004, "用户不存在"),
    WRONG_PASSWORD(20005, "密码错误"),
            ;

    ResponseCode(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    private Integer code;

    private String msg;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

}

```
## （四）接口编写
上面的两个类可以作为其他项目的通用类，要写接口时直接放代码就行，接下来写一个接口测试一下：

新建一个ResponseController

```
@RestController
public class ResponseController {

    @RequestMapping(value = "/getData",method = RequestMethod.GET)
    public Result getData(){
        Map<String,Object> map=new HashMap<>();
        map.put("name","javayz");
        map.put("age","23");
        Map<String,String> childMap=new HashMap<>();
        childMap.put("home","浙江");
        childMap.put("job","java");
        map.put("childMap",childMap);
        Result result=new Result(ResponseCode.SUCCESS.getCode(),ResponseCode.SUCCESS.getMsg(),map);
        return result;
    }

}
```
这里展示的是通过Map集合插入数据，最后返回Result，调用结果如下：

```
{
    "code": 0,
    "message": "操作成功",
    "data": {
        "name": "javayz",
        "childMap": {
            "job": "java",
            "home": "浙江"
        },
        "age": "23"
    }
}
```
除了使用Map传递数据之外，还可以通过对象来传递数据，新建两个类分别是User和UserDetail：

```
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private String age;
    private UserDetail userDetail;
}
@Data
@AllArgsConstructor
@NoArgsConstructor
public class UserDetail {
    private String home;
    private String job;
}
```
新写一个Get请求的接口，在接口中用对象传递数据

```
@RequestMapping(value = "/getData2",method = RequestMethod.GET)
public Result getData2(){
    UserDetail userDetail=new UserDetail("浙江","java");
    User user=new User("javayz","23",userDetail);
    Result result=new Result(ResponseCode.SUCCESS.getCode(),ResponseCode.SUCCESS.getMsg(),user);
    return result;
}
```
调用接口后返回值如下：

```
{
    "code": 0,
    "message": "操作成功",
    "data": {
        "name": "javayz",
        "age": "23",
        "userDetail": {
            "home": "浙江",
            "job": "java"
        }
    }
}
```
## （五）总结
一般来说公司里都会有一套自己的规则，如果是自己写一个新的项目，那么上面的这几个通用类就可以直接用了。