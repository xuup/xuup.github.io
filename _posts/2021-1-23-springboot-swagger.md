### springboot 结合 swagger的使用

#### swagger 介绍

Swagger是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。

#### springboot 结合 swagger使用步骤

##### 1. 引入maven依赖
```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
        <version>2.9.2</version>
    </dependency>

<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
        <version>2.9.2</version>
    </dependency>
```
##### 2.配置swagger
在项目中新增swagger的配置类，配置信息如下：
```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    private ApiInfo apiInfo(){
        return new ApiInfoBuilder()
                .title("spring-boot swagger 接口文档")
                .description("用户管理")
                .version("1.0.0")
                .build();
    }

    @Bean
    public Docket createRestApi(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.xup"))
                .paths(PathSelectors.any())
                .build();
    }
}
```

##### 3.配置swagger注释
在controller类上可通过Api注解进行标识，在具体的方法以及方法参数内，可以通过ApiOperation和ApiParam进行标识
```java
@Api(value = "用户信息管理")
@RestController
@RequestMapping("user")
public class UserController {

    Logger logger = LoggerFactory.getLogger(UserController.class);

    @Autowired
    UserRepository userRepository;

    @ApiOperation(value = "根据id获取用户信息", notes = "通过用户ID获取")
    @PostMapping("findByCondition")
    public UserEntity findByCondition(@ApiParam(value = "查询条件") UserEntity entity){
        logger.info("entity: {}", entity);
        Optional<UserEntity> res = userRepository.findById(entity.getId());
        return res.get();
    }
}
```

##### 4.启动本地项目，访问swagger-ui页面
```java
http://localhost:8081/swagger-ui.html
```
主界面如下：

