# 1、依赖

```xml
<!-- 用于JSON API文档的生成-->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>

<!--用于文档界面展示-->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
</dependencies>
```



# 2、配置类

```java
package com.example.demo.util;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.autoconfigure.condition.ConditionalOnExpression;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
@ConditionalOnExpression("${swagger.enable:true}")
public class SwaggerConfig {

    @Value("${swagger.contact.name}")
    String contactName;

    @Value("${swagger.contact.url}")
    String contactUrl;

    @Value("${swagger.contact.email}")
    String contactEmail;

    @Value("${swagger.title}")
    String title;

    @Value("一个Swagger示例")
    String description;

    @Value("1.0.0")
    String version;

    @Bean
    public Docket createDocket(){

        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.demo"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }

    /**
     * 站点描述信息
     * @return
     */
    private ApiInfo apiInfo(){
        Contact contact = new Contact(contactName, contactUrl, contactEmail);
        return new ApiInfoBuilder()
                .title(title)
                .description(description)
                .contact(contact)
                .version(version)
                .build();
    }

}

```

# 3、注解说明

* `@Api`：修饰整个类，描述Controller的作用
  - tags：描述模块功能
* `@ApiOperation`：标注在方法上
  - value：接口功能作用
  - notes：接口功能详述

* `@ApiImplicitParam`：一个请求参数描述
  - name：参数名称
  - value：参数说明
  - paramType：参数放在什么地方
  - path：路径参数
  - required：参数是否必须传
  - dataType：参数的类型
  - defaultValue：参数默认值

* `@ApiParam`：单个参数描述，与`@ApiImplicitParam`作用一样，不同的是：

  - 对Servlets或者非 JAX-RS的环境，只能使用 ApiImplicitParam。

  - 在使用上，ApiImplicitParam比ApiParam具有更少的代码侵入性，只要写在方法上就可以了，但是需要提供具体的属性才能配合swagger ui解析使用。

  - ApiParam只需要较少的属性，与swagger ui配合更好。

* `@ApiImplicitParams`：多个参数描述

```java
@ApiImplicitParams(value = {@ApiImplicitParam(),@ApiImplicitParam()})
```

* `@ApiResponses`：用于请求的方法上，表示一组响应
  - `@ApiResponse`：用在`@ApiResponses`中，一般用于表达一个错误的响应信息
            `code`：数字，例如400
            `message`：信息，例如"请求参数没填好"
            `response`：抛出异常的类

`@ApiModel`：标注在响应类上

`@ApiModelProperty`：标注在响应类的属性上