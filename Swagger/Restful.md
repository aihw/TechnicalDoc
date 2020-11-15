# Rest

#### 一、何为Rest

Rest即为Representational State Transfer ，意思为**表现层状态转化**。一个设计架构符合Rest原则，则称之为Restful架构；一个设计符合Rest原则的接口即为Restful的接口。

#### 二、如何理解Rest

在Rest的名称中，**表现层**其实指的是**资源（Resource）**的**表现层**。

所谓的资源就是指网络上的一个实体，或者说是网络上一个具体的信息。他可以是一段文字、一张图片、一首歌曲，总之就是一个具体的存在。我们可以使用一个URI指向它，每种资源对应一个特定的URI。 要获取这个资源，访问它的URI即可。所谓的**上网**就是与互联网上众多资源的互动，调用它的URI。

那何为表现层？

资源存在的形式即为资源的表现性，比如文本使用txt格式表现，也可以使用html、xml、Json格式表现，图片可以使用png、jpg、jpeg格式表现。

那何为状态转化？

访问一个网站，代表了客户端和服务器的交互过程；在这个过程中，势必涉及到数据和状态的变化。

http协议是一个无状态协议，这代表着所有的状态都保存在服务器端，因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生**状态转化（State Transfer）**，而这种转化是建立于表现层之上的，所以就是**表现层状态转化**。

客户端用到的手段，只能是HTTP协议。就是使用HTTP协议中四个表示操作方式的动词：

**GET:获取资源**

**POST:新建资源（也可用于更新资源）**

**PUT:更新资源**

**DELETE:删除资源**

综上->Restful就是，客户端通过四个HTTP动词，对服务器端的资源（URI）进行操作，实现表现性状态转化，即为Restful。换句话说就是使用URL定位资源，再使用HTTP动词（GET、POST、PUT 、DELETE）描述操作。

#### 三、Rest设计时常见的误区

URl不可包含动词，因为资源表示的是一个实体，应为名词。

例：http://localhost/users/get/1 其中get是动词，意思就是获取，这个URI设计有误，正确的写法如下：http://localhost/users/1 ,然后用GET方法表示获取

如果某些动作是HTTP动词无法表示的，应该把动作做成一种资源，如网上汇款，从A向B汇款

错误示例：http://localhost/accounts/A/transfer/500/to/B 包含动词 且过于复杂

正确示例：http://localhost/transaction POST方式  from=A&to=B&amount=500（body中）

URI中不可加入版本号，不同的版本，可以理解成同一资源的不同表现形式，应该采取同一个URI。版本号可以在HTTP请求头信息的Accept字段中进行区分。

#### 四、如何设计Restful API

##### 1.协议

API与用户的通信协议，总是使用HTTPs协议。

##### 2.域名

应该尽量将API部署在专用域名之下

如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。

##### 3.版本

应该将API的版本号放入URL。或者将版本号放入HTTP头信息中， 但不如放入URL方便和直观 。

##### 4.路径

路径又称"终点"（endpoint），表示API的具体网址。

在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以API中的名词也应该使用复数。

举例来说，有一个API提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

```
https://api.example.com/v1/zoos
https://api.example.com/v1/animals
https://api.example.com/v1/employees
```

##### 5.HTTP动词

 对于资源的具体操作类型，由HTTP动词表示。 

下面是一些例子。

```
GET /zoos 列出所有动物园
POST /zoos 新建一个动物园
GET /zoos/ID 获取某个指定动物园的信息
PUT /zoos/ID 更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/ID 更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/ID 删除某个动物园
GET /zoos/ID/animals 列出某个指定动物园的所有动物
DELETE /zoos/ID/animals/ID 删除某个指定动物园的指定动物
```

##### 6.过滤信息

如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。
 下面是一些常见的参数。

```bash
?limit=10 指定返回记录的数量
?offset=10 指定返回记录的开始位置。
?page=2&per_page=100 指定第几页，以及每页的记录数。
?sortby=name&order=asc 指定返回结果按照哪个属性排序，以及排序顺序。
?animal_type_id=1 指定筛选条件
```

参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

##### 7.错误处理

如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。

~~~
{
	error: "Invalid API key"
}
~~~

##### 8.返回结果

针对不同操作，服务器向用户返回的结果应该符合以下规范。

```undefined
GET /collection 返回资源对象的列表（数组）
GET /collection/resource 返回单个资源对象
POST /collection 返回新生成的资源对象
PUT /collection/resource 返回完整的资源对象
PATCH /collection/resource 返回完整的资源对象
DELETE /collection/resource 返回一个空文档
```

##### 9.Hypermedia API

RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。
 　比如，当用户向api.example.com的根目录发出请求，会得到这样一个文档。

```json
{"link": { 
  "rel":   "collection https://www.example.com/zoos",
  "href":  "https://api.example.com/zoos",
  "title": "List of zoos",
  "type":  "application/vnd.yourformat+json"
}}
```

上面代码表示，文档中有一个link属性，用户读取这个属性就知道下一步该调用什么API了。rel表示这个API与当前网址的关系（collection关系，并给出该collection的网址），href表示API的路径，title表示API的标题，type表示返回类型。
 　Hypermedia API的设计被称为[HATEOAS](http://en.wikipedia.org/wiki/HATEOAS)。Github的API就是这种设计，访问[api.github.com](https://api.github.com/)会得到一个所有可用API的网址列表。

```json
{
  "current_user_url": "https://api.github.com/user",
  "authorizations_url": "https://api.github.com/authorizations",
  // ...
}
```

从上面可以看到，如果想获取当前用户的信息，应该去访问[api.github.com/user](https://api.github.com/user)，然后就得到了下面结果。

```json
  {
    "message": "Requires authentication",
    "documentation_url": "https://developer.github.com/v3"
}
```

上面代码表示，服务器给出了提示信息，以及文档的网址。

##### 10.其他

服务器返回的数据格式，应该尽量使用JSON，避免使用XML。 