# Swagger

#### 一、何为Swagger

 Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。 

代码即文档。

#### 二、Swagger功能组成

##### 1.Swagger Codegen

通过Codegen 可以将描述文件生成html格式和cwiki形式的接口文档，同时也能生成多钟语言的服务端和客户端的代码。支持通过jar包，docker，node等方式在本地化执行生成。也可以在后面的Swagger Editor中在线生成。

##### 2.Swagger UI

提供了一个可视化的UI页面展示描述文件。接口的调用方、测试、项目经理等都可以在该页面中对相关接口进行查阅和做一些简单的接口请求。该项目支持在线导入描述文件和本地部署UI项目。

##### 3.Swagger Editor 

类似于markendown编辑器的编辑Swagger描述文件的编辑器，该编辑支持实时预览描述文件的更新效果。也提供了在线编辑器和本地部署编辑器两种方式。

##### 4.**Swagger **Inspector

和postman差不多，是一个可以对接口进行测试的在线版的postman。比在Swagger UI里面做接口请求，会返回更多的信息，也会保存你请求的实际请求参数等数据。

##### 5.Swagger Hub

集成了上面所有项目的各个功能，你可以以项目和版本为单位，将你的描述文件上传到Swagger Hub中。在Swagger Hub中可以完成上面项目的所有工作，需要注册账号，分免费版和收费版。

#### 三、如何使用（Swagger UI）

1.导入Maven依赖

~~~
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>2.7.0</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.7.0</version>
</dependency>
~~~

2.添加swagger配置类

~~~java
@Configuration
@EnableSwagger2
public class SwaggerConfiguration {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                //.host("")//域名
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com"))//扫描com路径下的api文档
                .paths(PathSelectors.any())//路径判断
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("xxx项目Api文档")//标题
                .description("xxx项目开发规范详文档细地址-")//描述
                .termsOfServiceUrl("http://www.baidu.com")//（不可见）条款地址
                .version("1.0.0")//版本号
                .build();
    }
}

~~~

3.正常编写代码，在对应的Controller上添加Swagger的注解(请先看常用注解及说明)

4.访问对应的Swagger-ui页面，我本地是 localhost:8080/swagger-ui.html 主机名+端口号+/swagger-ui.html即可查看对应的接口文档。

5.在yml或者properties配置 **springfox.documentation.swagger.v2.path=/doart/api-docs**（可配置）  使用**主机名+端口号+/doart/api-docs** 即可获取所有的文档信息。

#### 四、常用的注解及其常用属性

##### 1.@Api

```
作用于类上,表示对类的说明
@Api(tags = "关于宠物接口相关Api")
@RestController
public class PetController {}
tags属性:用于说明此类的作用
```

##### 2.@ApiOperation

~~~
作用于方法上,表示对该方法的说明
@ApiOperation(value = "查询宠物信息",notes = "根据宠物ID查询宠物信息,ID必须为数字且合法")
@GetMapping("/pet/{id}")
public Pet getPetById(@PathVariable("id") Integer id) {}
value属性:用于说明该方法的作用、用途
notes属性:对该方法的备注说明
~~~

##### 3.@ApiImplicitParam

~~~
作用于方法上,表示对该方法参数的说明
@ApiImplicitParam(name = "id",value = "宠物ID", required = true, dataType = "int", paramType = "path")
@GetMapping("/pet/{id}")
public Pet getPetById(@PathVariable("id") Integer id) {}
name属性:参数名称
value属性:参数的解释说明
required属性:参数是否是必须项(true 或 false)
dataType属性:参数类型,默认为String,可以为int...
paramType属性:参数获取的位置->
1)header-->@RequestHeader
2)query-->@RequestParam
3)path(用于Restful接口)-->@PathVariable
4)body-->@RequestBody
5)form-->@RequestBody
~~~

##### 4.@ApiImplicitParams

~~~
作用于方法上,当请求参数大于1时,可以使用该注解
@ApiOperation(value = "根据宠物名称和宠物年龄查询宠物",notes = "宠物年龄和宠物名称都可以为空")
    @ApiImplicitParams({
    	@ApiImplicitParam(name = "name",value = "宠物名称",paramType = "query"),
    	@ApiImplicitParam(name = "age",value = "宠物年龄",paramType = "query")})
    @GetMapping("/pet")
    public List<Pet> getPetByNameAndAge(@RequestParam("name") String name, @RequestParam("age")Integer age) {

        return new ArrayList<>(map.values());
    }
~~~

##### 5.@ApiModel

~~~
作用于类上,描述一个实体类.
@ApiModel(description = "宠物实体对象")
public class Pet {

    @ApiModelProperty(value = "宠物ID",required = true)
    private Integer id;
    @ApiModelProperty(value = "宠物名称")
    private String name;
    @ApiModelProperty(value = "宠物的年龄")
    private Integer age;
}
@ApiModel的description属性:对该类的描述说明.
@ApiModelProperty作用于实体类的属性上,表示对属性的解释说明->
value属性:对该实体类属性的解释
required属性(true或false):表示当前属性是否为必须项
~~~

##### 6.@ApiResponse

~~~
作用于方法上,表示该方法响应状态码为404时,对应的提示信息,作用于类上时,对类中所有方法都生效
@ApiResponse(code = 404, message = "页面走丢了",response = com.doart.swagger.bean.RestMessage.class)
@RestController
public class PetController {}
code属性:表示Http响应状态码
message属性:表示对应状态码的提示信息
response属性:用于描述消息有效负载的可选响应类
~~~

##### 7.@ApiResponses

~~~
作用于方法上,表示该方法响应,作用于类上时,对类中所有方法都生效
@Api(value = "Pet-Api", tags = "关于宠物接口相关Api")
@ApiResponses({
	@ApiResponse(code = 400, message = "400错误"),
    @ApiResponse(code = 200,message = "响应成功"),
    @ApiResponse(code = 404, message = "页面走丢了")
    })
@RestController
public class PetController {}
~~~

##### 8.@ApiIgnore()

~~~
@ApiIgnore()
@ApiOperation(value = "测试", notes = "测试")
@PutMapping("/test")
public Object test() {}
@ApiIgnore表示忽略该方法,不生成该方法的文档说明
~~~

#### 五、案例

~~~
@Api(tags = "关于宠物接口相关Api")
@ApiResponses({@ApiResponse(code = 400, message = "400错误"),
        @ApiResponse(code = 200,message = "响应成功",response = Pet.class),
        @ApiResponse(code = 404, message = "页面走丢了")})
@RestController
public class PetController {

    private Map<Integer, Pet> map = new HashMap<>();

    @ApiOperation(value = "查询宠物信息",notes = "根据宠物ID查询宠物信息,ID必须为数字且合法")
    @ApiImplicitParam(name = "id",value = "宠物ID", required = true, dataType = "int", paramType = "path")
    @GetMapping("/pet/{id}")
    public Pet getPetById(@PathVariable("id") Integer id) {

        return map.get(id);
    }

    @ApiOperation(value = "根据宠物名称和宠物年龄查询宠物",notes = "宠物年龄和宠物名称都可以为空")
    @ApiImplicitParams({@ApiImplicitParam(name = "name",value = "宠物名称",paramType = "query"),
            @ApiImplicitParam(name = "age",value = "宠物年龄",paramType = "query")})
    @GetMapping("/pet")
    public List<Pet> getPetByNameAndAge(@RequestParam("name") String name, @RequestParam("age")Integer age) {

        return new ArrayList<>(map.values());
    }

    @ApiOperation(value = "添加宠物信息", notes = "添加宠物信息")
    @PostMapping(value = "/pet", consumes = "application/json")
    public Pet addPet(@RequestBody Pet pet) {

        Integer id = map.size();
        pet.setId(id);
        map.put(id, pet);
        return pet;
    }

    @ApiOperation(value = "删除宠物信息", notes = "根据宠物ID删除宠物信息,ID必须为数字且合法")
    @ApiImplicitParam(name = "id", value = "宠物ID", required = true, dataType = "int", paramType = "path")
    @DeleteMapping("/pet/{id}")
    public Pet deletePet(@PathVariable Integer id) {

        return map.remove(id);
    }

    @ApiOperation(value = "更新宠物信息", notes = "根据传入宠物信息进行更新")
    @PutMapping("/pet")
    public Pet updatePetById(@RequestBody Pet pet) {

        Integer id = pet.getId();
        map.put(id, pet);
        return pet;
    }

    @ApiOperation(value = "查询全部宠物信息", notes = "查询全部宠物信息集合")
    @GetMapping("/pets")
    public List<Pet> getPets(HttpServletResponse response) {

        return new ArrayList<>(map.values());
    }

    @ApiIgnore()// 不显示该方法的文档
    @ApiOperation(value = "测试", notes = "测试")
    @PutMapping("/test")
    public Object test() {

        return null;
    }
}

~~~

对应UI:

github