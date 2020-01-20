---
title: springboot整合swagger2，配置swagger2支持传参token进行验证
date: 2019-9-26 22:39:31
tags:
    - Java
    - SpringBoot
    - Swagger2
categories:
        - 后端
---
#### 本文旨在教你如何在springboot整合swagger2，配置swagger2支持传参token进行验证

前言：该博客主要是记录自己成长的点滴，当然也希望能够帮助到读者，本人小白一枚，难免会出错，如果文中有任何错误的地方，请务必留言指出，感激不尽，大佬们不喜勿喷，谢谢~~~
<!-- more -->
#### 由于本人在项目中引入了shiro权限认证框架，有时候调试接口就需要传参token，所以配置swagger2支持传参token。
#### 还不了解springboot整合swagger2的朋友可以参考我上一篇文章：https://www.jianshu.com/p/7386a0e04ca8

#### 第一步，修改配置文件swagger2configration的createRestApi()方法，如下

```
    @Bean
    public Docket createRestApi() {
        ParameterBuilder tokenPar = new ParameterBuilder();
        List<Parameter> pars = new ArrayList<>();
        tokenPar.name("token").description("token").modelRef(new ModelRef("string")).parameterType("header").required(false).build();
        pars.add(tokenPar.build());

        return new Docket(DocumentationType.SWAGGER_2)
                .globalOperationParameters(pars)
                .apiInfo(apiInfo())
                .select()
                //此处添加需要扫描接口的包路径
                .apis(basePackage("com.jinhaoxun.acweb.controller.applyController" + splitor + "com.jinhaoxun.acweb.controller.shiroController" + splitor + "com.jinhaoxun.acweb.test"))
                .paths(PathSelectors.any())
                .build();
    }
```

#### 第二步，启动项目，访问：http://localhost:8088/swagger-ui.html，如下图
![1.jpg](https://upload-images.jianshu.io/upload_images/16847375-33c35dc055a056e0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

后记：本次的“springboot整合swagger2，配置swagger2支持传参token进行验证”教程到此结束，有任何意见或建议请留言，谢谢~~~
