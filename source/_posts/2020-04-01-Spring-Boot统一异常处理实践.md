---
title: Spring Boot统一异常处理实践
date: 2020-04-01 19:14:57
urlname: spring-boot-exception-handling
categories:
- 开发
- spring-boot
tags:
- spring-boot
- 工程实践
---

>在web开发中，经常需要处理各类异常情况，这种处理逻辑经常会被分散在代码的各个层次，导致异常的处理和业务逻辑混杂，后续重构代价升高，前后端交互成本提高，不利用系统的持续演进。本文针对工作中遇到的不良实践进行总结，试图找寻一种可以解决大部分问题的工程实践方案。

<!-- more -->

#### 核心关注点
对异常的处理，总结先来，主要有三个核心的关注点：
- 在什么时候，什么地方捕获异常（try-catch），什么时候需要抛出（throws）异常
- 在DAO、Service还是在Controller层次进行捕获
- 抛出一场后，如何处理，如何向前端返回错误

#### 工作中遇到的反例
以个人在工作中接触的一个django项目为例，其业务逻辑都集中在view.py文件中，通过装饰器，将返回内容统一转换为json形式进行返回，系统在异常处理存在的问题集中体现在：

- 分散在业务各处的返回信息处理
- 满大街的try-catch，可能一层套一层
- 混乱的返回方式

后端和前端约定返回结果形式：
```json
{
    "message":"ok",
    "data":[],
    "success": true
}
```

**分散的返回处理**
即返回结果的处理没有统一的入口，可能分散在代码的多个层次中，导致返回信息的组织和业务逻辑交织在一起，代码逻辑非常不清晰，其次，会导致返回信息不对的情况下，排查人力投入明显增加。经常看到返回的结果message对应的从约定的"ok"变成了其他的词，"success"也出现五花八门的返回结果。

**满大街的try-catch**
在代码的不同层次，你都能找到活多或少的try-cath处理段，过多的try-catch 导致业务代码分割严重，也增加了对应处理逻辑，导致系统的复杂度上升，后续维护重构举步维艰。

**混乱的返回方式**
在spring-mvc中，返回统一在controller层次，第一层的函数就负责了结果的返回，但是接触到的django项目，它的返回逻辑甚至能在入口跳转6次的函数中，没有统一的处理入口，出了问题需要到处排查，前后端要一起折腾。

**总结**
当前项目的异常处理方式非常失败，处于xx原因，就不好列举代码了。

#### 异常处理规范

为了进行统一的异常处理，一般会约定异常处理规范，这里不仅仅是将结果形式定义了就完事了，还得通过前后端各种工程实践将这个规范践行下去。

#### 原则一： 不捕获任何异常
根据大佬们的实践经验，不再对与业务逻辑和数据处理中的异常进行捕获，即将DAO、Service、Controller中所有的异常全部抛出到上层，从而不眠代码中一堆的try-catch，减少维护的难度。

#### 原则二：统一返回结果集
这个原则很多开发实践都已经明确，就是按照统一的一个格式进行结果返回，返回结果中应该起码包含请求的处理状态标志，此外用一层数组包装查询的结果。

而在spring-boot(mvc)架构中，通常会定义一个Java的DTO对象，用来进行统一的结果返回，for example:

```java
public class ResultBean<T> {
    private int code; // 统一协商的状态码
    private String message; // 携带的额外提示信息
    private Collection<T> data; // 请求返回的数据集

    private ResultBean() {

    }

    public static ResultBean error(int code, String message) {
        ResultBean resultBean = new ResultBean();
        resultBean.setCode(code);
        resultBean.setMessage(message);
        return resultBean;
    }

    public static ResultBean success() {
        ResultBean resultBean = new ResultBean();
        resultBean.setCode(0);
        resultBean.setMessage("success");
        return resultBean;
    }

    public static <V> ResultBean<V> success(Collection<V> data) {
        ResultBean resultBean = new ResultBean();
        resultBean.setCode(0);
        resultBean.setMessage("success");
        resultBean.setData(data);
        return resultBean;
    }

    // getter / setter 略
}

```

从而，在成功的情况下，只需要返回ResultBean.success() 或 ResultBean.success(Collection<V> data)需要返回数据按照下面的形式：
```java
@RequestMapping("/host/add")
@ResponseBody
public ResultBean<Goods> getAllHosts() {
    List<Host> hosts = hostService.findAll();
    return ResultBean.success(hosts);
}
@RequestMapping("/host/update")
@ResponseBody
public ResultBean updatehosts(Host hosts) {
    hostsService.update(host);
    return ResultBean.success();
}
````

只有查询方法需要调用 ResultBean.success(Collection<V> data) 来返回 N 条数据, 删除, 修改等方法都应该调用 ResultBean.success(), 即在业务代码中只处理正确的功能, 不对异常做任何判断. 也不需要对 update 或 delete 的更新条数做判断. 只要没有抛出异常, 就等价于用户请求处理成功，再加足够的日志即可，且操作成功的提示信息在前端处理, 不要后台返回 “操作成功” 等字段，这种设计在国际化的时候能减轻不少的工作量。

前台接受的json for example：
```javascript
{
    "code": 0,
    "message": "success",
    "data": [
        {
            "sn": "xxxx",
            "hostname": "xxxxx",
        },
        {
            "sn": "xxxx",
            "hostname": "xxxx",
        }
    ]
}

```

- 在后端报错情况下，在后端统一调用ResultBean.error(int code, String message), 来将状态码和错误信息返回, 我们约定 code 为 0 表示操作成功, 1 标示系统错误， 2标示 参数错误（协商解决）

前台收到错误结果后，只要根据对应的错误code进行相应的提示信息弹出即可。

#### 后端统一异常处理
这里是最重点的地方，spring-boot（mvc）开发的web应用中，如何进行统一的异常处理呢，spring利用AOP，提供了一种@Advice的处理方式，我们所有的异常只要能保证抛出到controller层次， 就能由统一的Advice进行处理：

```java
@ControllerAdvice
@ResponseBody
public class RequestExceptionHandler {

    private static final Logger log = LoggerFactory.getLogger(RequestExceptionHandler.class);

    @ExceptionHandler
    public ResultBean unknownSn(UnknownSntException e) {
        log.error("机器sn不存在", e);
        return ResultBean.error(1, "sn不存在");
    }

    @ExceptionHandler
    public ResultBean incorrectMac(IncorrectMacException e) {
        log.error("mac地址不对", e);
        return ResultBean.error(-2, "mac地址错误");
    }

    @ExceptionHandler
    public ResultBean unknownException(Exception e) {
        log.error("内部错误", e);
        return ResultBean.error(-99, "系统出现错误，联系管理员处理");
    }
}

```

#### 总结
- 不要满大街的try-catch
- 统一返回值，集中异常的处理逻辑
- 认真对待写的代码
