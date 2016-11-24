# HTTP API
## Request
### Content
如果你要根据请求内容进行响应,Moco服务器可以配置如下:
- java api
	```java
    	server.request(by("foo")).response("bar");
    ```
- JSON
	```json
    	{
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```
如果请求内容非常大,你可以将它放到一个文件里面:
- java api
	```java
    	server.request(by(file("foo.request"))).response("bar");
    ```
- Json
	```json
        {
          "request" :
            {
              "file" : "foo.request"
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```

### URI
如果你很关注uri，moco支持这样设置:
- java api
	```java
    	server.request(by(uri("/foo"))).response("bar");
    ```
-  Json
	```json
    	{
          "request" :
            {
              "uri" : "/foo"
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```

### Query Parameter
有时,你的请求在有查询参数:
- java api
	```java
    	server.request(and(by(uri("/foo")), eq(query("param"), "blah"))).response("bar");
    ```
- json
	```json
    	{
          "request" :
            {
              "uri" : "/foo",
              "queries" :
                {
                  "param" : "blah"
                }
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```

### HTTP Method
想要根据指定的http方法响应也是很容易设置的
- java api
	```java
    	server.get(by(uri("/foo"))).response("bar");
    ```
- json
	```json
    	{
          "request" :
            {
              "method" : "get",
              "uri" : "/foo"
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```
post方法也一样:
- java api
	```java
    	server.post(by("foo")).response("bar");
    ```
- json
	```json
    	{
          "request" :
            {
              "method" : "post",
              "text" : "foo"
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```
put方法这样:
- java api
	```java
    	server.put(by("foo")).response("bar");
    ```
- json
	```json
    	{
          "request" :
            {
              "method" : "put",
              "text" : "foo"
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```
delete方法这样:
-  java api
	```java
    	server.delete(by(uri("/foo"))).response(status(200));
    ```
- json
	```json
        {
          "request" :
            {
              "method" : "delete",
              "uri" : "/foo"
            },
          "response" :
            {
              "status" : "200"
            }
        }
    ```
如果你需要其他方法,你也可以随意指定:
- java
	```java
    	server.request(by(method("HEAD"))).response("bar");
    ```
- json
	```json
    	{
          "request" :
            {
              "method" : "HEAD"
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```

### Version
我们可以为不同的HTTP版本返回不同的响应：
- java
	```java
    	server.request(by(version("HTTP/1.0"))).response("version");
    ```
- json
	```json
    	{
          "request":
            {
              "version": "HTTP/1.0"
            },
          "response":
            {
              "text": "version"
            }
        }
    ```

### Header
如果你很关注http的header头部：
- java api
	```java
    	server.request(eq(header("foo"), "bar")).response("blah")
    ```
- json
	```json
    	{
          "request" :
            {
              "method" : "post",
              "headers" :
              {
                "content-type" : "application/json"
              }
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```

### Cookie
- java
	```java
    	server.request(eq(cookie("loggedIn"), "true")).response(status(200));
    ```
- json
	```json
    	{
          "request" :
            {
              "uri" : "/cookie",
              "cookies" :
                {
                  "login" : "true"
                }
            },
          "response" :
            {
              "text" : "success"
            }
        }
    ```

### Form
在Web开发中，Form表单通常用于向服务器端提交信息。
- java
	```java
    	server.post(eq(form("name"), "foo")).response("bar");
    ```
- json
	```json
    	{
          "request" :
            {
              "method" : "post",
              "forms" :
                {
                  "name" : "foo"
                }
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```
### XML
XML是Web服务的流行格式。 当请求是XML时，在大多数情况下只有XML结构是重要的，并且可以忽略空格。 对于这种情况，可以使用xml运算符。
- java
	```java
    	server.request(xml(text("<request><parameters><id>1</id></parameters></request>"))).response("foo");
    ```
- json
	```json
    	{
          "request":
            {
              "uri": "/xml",
              "text":
                {
                  "xml": "<request><parameters><id>1</id></parameters></request>"
                }
            },
          "response":
            {
              "text": "foo"
            }
        }
    ```
注意：请在文本中转义字符。
如果请求很大,也可以放到一个文件中
```json
    {
       "request":
         {
            "uri": "/xml",
            "file":
              {
                "xml": "your_file.xml"
              }
        },
      "response":
        {
          "text": "foo"
        }
    }
```

### XPath
对于XML/HTML请求，Moco允许我们将请求与XPath匹配。
- java
	```java
    	server.request(eq(xpath("/request/parameters/id/text()"), "1")).response("bar");
    ```
- json
	```json
    	{
          "request" :
            {
              "method" : "post",
              "xpaths" :
                {
                  "/request/parameters/id/text()" : "1"
                }
            },
          "response" :
            {
              "text" : "bar"
            }
        }
	```

### JSON Request
跟sorcket api一样,详情请移步[这里](socket_api.md)
### JSONPATH
跟sorcket api一样,详情请移步[这里](socket_api.md)
### Operator
跟sorcket api一样,详情请移步[这里](socket_api.md)

## Response
### Content
- java
	```java
    	server.request(by("foo")).response("bar");
    ```
- json
	```json
    	{
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "text" : "bar"
            }
        }
    ```
与request相同，如果内容太大，无法放入字符串，可以使用文件进行响应。
- java
	```java
    	server.request(by("foo")).response(file("bar.response"));
    ```
- json
	```json
    	{
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "file" : "bar.response"
            }
        }
    ```
如果要在控制台中以正确的编码方式查看，可以指定文件字符集。
- java
	```java
    	server.response(file("src/test/resources/gbk.response", Charset.forName("GBK")));
    ```
- json
	```json
    	[
          {
            "response":
            {
              "file":
              {
                "name": "gbk.response",
                "charset": "GBK"
              }
            }
          }
        ]
    ```
字符集也可以在路径资源中使用。
- java
	```java
    	server.response(pathResource("src/test/resources/gbk.response", Charset.forName("GBK")));
    ```
- json
	```json
    	[
          {
            "response":
            {
              "path_resource":
              {
                "name": "gbk.response",
                "charset": "GBK"
              }
            }
          }
        ]
    ```

### Status Code
Moco支持HTTP状态码响应
- java
    ```java
        server.request(by("foo")).response(status(200));
    ```
- json
    ```json
        {
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "status" : 200
            }
        }
    ```

### Version
默认情况下，HTTP response版本对应HTTP request版本，但是您可以设置自己的HTTP版本：
- java
	```java
    	server.response(version(HttpProtocolVersion.VERSION_1_0));
    ```
- json
	```json
    	{
          "request":
            {
              "uri": "/version10"
            },
          "response":
            {
              "version": "HTTP/1.0"
            }
        }
    ```

### Header
我们还可以指定HTTP头作为响应。
- java
	```java
    	server.request(by("foo")).response(header("content-type", "application/json"));
    ```
- json
	```json
    	{
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "headers" :
                {
                  "content-type" : "application/json"
                }
            }
        }
    ```

### Proxy(代理)
#### 单一url
我们也可以响应指定的url，就像代理。
- java
	```java
    	server.request(by("foo")).response(proxy("http://www.github.com"));
    ```
- json
	```json
    	{
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "proxy" : "http://www.github.com"
            }
        }
	···
实际上，代理比这更强大。 它可以将整个请求转发到目标网址，包括HTTP方法，版本，标题，内容等。
#### Failover（故障转移）
除了基本功能，代理还支持故障转移，这意味着如果远程服务器暂时不可用，服务器将知道从本地配置恢复。
- java
	```java
    	server.request(by("foo")).response(proxy("http://www.github.com", failover("failover.json")));
JSON
	```
- json
	```json
    	{
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "proxy" :
                {
                  "url" : "http://localhost:12306/unknown",
                  "failover" : "failover.json"
                }
            }
        }
    ```
代理将会将请求/响应对保存到故障转移文件中。 如果代理目标不可访问，代理将从文件故障转移。 此功能对于开发环境非常有用，特别是对于集成服务器不稳定的情况。

像文件后缀建议的，这个故障转移文件实际上是一个JSON文件，这意味着我们可以读/编辑它返回任何我们想要的。

#### Playback(回放)
Moco也支持Playback，也保存远程请求和响应到本地文件。 故障转移和回放之间的区别在于，当本地请求和响应不可用时，Playback仅访问远程服务器。
- java
	```java
    	server.request(by("foo")).response(proxy("http://www.github.com", playback("playback.json")));
    ```
- json
	```json
    	{
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "proxy" :
                {
                  "url" : "http://localhost:12306/unknown",
                  "playback" : "playback.json"
                }
            }
        }
    ```

#### 批量url
如果我们想在同一上下文中用一批URL代理，代理也可以帮助我们。
- java
	```java
    	server.get(match(uri("/proxy/.*"))).response(proxy(from("/proxy").to("http://localhost:12306/target")));
    ```
- json
	```json
    	{
            "request" :
            {
                "uri" : {
                    "match" : "/proxy/.*"
                }
            },
            "response" :
            {
                "proxy" : {
                    "from" : "/proxy",
                    "to" : "http://localhost:12306/target"
                }
            }
        }
    ```
与单个网址相同，您还可以指定故障转移。
- java
	```java
    	server.request(match(uri("/proxy/.*"))).response(proxy("http://localhost:12306/unknown"), failover("failover.response")));
  	```
- json
	```json
        {
        	"request" :
            {
                "uri" : {
                    "match" : "/failover/.*"
                }
            },
            "response" :
            {
                "proxy" :
                {
                    "from" : "/failover",
                    "to" : "http://localhost:12306/unknown",
                    "failover" : "failover.response"
                }
            }
        }
    ```
也支持回放
- java
	```java
    	server.request(match(uri("/proxy/.*"))).response(proxy("http://localhost:12306/unknown"), playback("playback.response")));
    ```
- json
	```json
		{
            "request" :
            {
                "uri" : {
                    "match" : "/failover/.*"
                }
            },
            "response" :
            {
                "proxy" :
                {
                    "from" : "/failover",
                    "to" : "http://localhost:12306/unknown",
                    "playback" : "playback.response"
                }
            }
        }
    ```
正如你可能会发现，我们经常设置请求匹配相同的上下文与响应，所以Moco给了我们一个快捷方式。
- java
	```java
    	server.proxy(from("/proxy").to("http://localhost:12306/target"));
    ```
- json
	```json
    	{
            "proxy" : {
                "from" : "/proxy",
                "to" : "http://localhost:12306/target"
            }
        }
    ```
故障转移也一样
- java
	```java
    	server.proxy(from("/proxy").to("http://localhost:12306/unknown"), failover("failover.response"));
    ```
- json
	```json
    	{
          "proxy" :
          {
              "from" : "/failover",
              "to" : "http://localhost:12306/unknown",
              "failover" : "failover.response"
          }
        }
    ```
playback也一样
- java
	```java
    	server.proxy(from("/proxy").to("http://localhost:12306/unknown"), playback("playback.response"));
    ```
- json
	```json
    	{
          "proxy" :
          {
              "from" : "/failover",
              "to" : "http://localhost:12306/unknown",
              "playback" : "playback.response"
          }
        }
    ```

### Redirect（重定向）
重定向是正常Web开发的常见情况。 我们可以简单地将请求重定向到不同的网址。
- java
	```java
    	server.get(by(uri("/redirect"))).redirectTo("http://www.github.com");
    ```
- json
	```json
    	{
          "request" :
            {
              "uri" : "/redirect"
            },
          "redirectTo" : "http://www.github.com"
        }
	```
### Cookie
cookie也可以设置在response中
- java
	```java
    	server.response(cookie("loggedIn", "true"), status(302));
    ```
- json
	```json
    	{
          "request" :
            {
              "uri" : "/cookie"
            },
          "response" :
            {
              "cookies" :
              {
                "login" : "true"
              }
            }
        }
    ```
#### Cookie Attributes
##### Path
path cookie属性定义cookie的范围.你可以向响应中添加自己的path cookie属性
- java
	```java
    	server.response(cookie("loggedIn", "true", path("/")), status(302));
    ```
- json
	```json
    	{
          "request" :
            {
              "uri" : "/cookie"
            },
          "response" :
            {
              "cookies" :
              {
                "login" : {
                    "value" : "true",
                    "path" : "/"
                }
              }
            }
        }
    ```

##### Domain
- java
	```java
    	server.response(cookie("loggedIn", "true", domain("github.com")), status(302));
    ```
- json
	```json
    	{
          "request" :
            {
              "uri" : "/cookie"
            },
          "response" :
            {
              "cookies" :
              {
                "login" : {
                    "value" : "true",
                    "domain" : "github.com"
                }
              }
            }
        }
    ```

##### Secure
- java
	```java
    	server.response(cookie("loggedIn", "true", secure()), status(302));
    ```
- json
	```json
    	{
          "request" :
            {
              "uri" : "/cookie"
            },
          "response" :
            {
              "cookies" :
              {
                "login" : {
                    "value" : "true",
                    "secure" : "true"
                }
              }
            }
        }
    ```

##### HTTP Only
- java
	```java
    	server.response(cookie("loggedIn", "true", httpOnly()), status(302));
    ```
- json
	```json
    	{
          "request" :
            {
              "uri" : "/cookie"
            },
          "response" :
            {
              "cookies" :
              {
                "login" : {
                    "value" : "true",
                    "httpOnly" : "true"
                }
              }
            }
        }
    ```

#### Attachment（附件）
附件通常用于Web开发。 您可以在Moco中设置附件如下。 正如你将看到的，你最好设置客户端接收的一个文件名。
- java
	```java
    	server.get(by(uri("/"))).response(attachment("foo.txt", file("foo.response")));
    ```
- json
	```json
    	{
          "request": {
            "uri": "/file_attachment"
          },
          "response": {
            "attachment": {
                "filename": "foo.txt",
                "file": "foo.response"
            }
          }
        }
    ```
#### Latency（延迟）
有时候我们需要一个延迟来模拟缓慢的服务器端操作
- java
	```java
    	server.response(latency(1, TimeUnit.SECONDS));
    ```
- json
	```json
    	{
          "request" :
            {
              "text" : "foo"
            },
          "response" :
            {
              "latency":
                {
                  "duration": 1,
                  "unit": "second"
                }
            }
        }
    ```
#### Sequence（序列）
有时，我们想要模拟一个改变服务器端资源的真实操作。 例如：
    - 首次请求资源并返回“foo”
    - 我们更新此资源
    - 再次请求相同的网址,返回新的内容。
    ```java
        server.request(by(uri("/foo"))).response(seq("foo", "bar", "blah"));
    ```
其他响应设置也可以设置。
	```java
    	server.request(by(uri("/foo"))).response(seq(status(302), status(302), status(200)));
    ```

#### JSON Response
如果响应是JSON，我们不需要在代码中使用转义字符编写JSON文本。
您可以将POJO给予Java API，它将转换为JSON文本。 提示，这个api也将设置Content-Type头。
```java
	server.request(by(uri("/json"))).response(toJson(pojo));
```
请注意，此功能是用Jackson实现，请确保您的POJO是以Jackson可接受的格式编写的。
对于JSON API，只需直接提供json对象
```json
    {
        "request":
          {
            "uri": "/json"
          },
        "response":
          {
            "json":
              {
                "foo" : "bar"
              }
          }
    }
```

#### Mount(挂载)
Moco允许我们挂载一个目录到uri。
- java
	```java
    	server.mount(dir, to("/uri"));
    ```
- json
	```json
    	{
          "mount" :
            {
              "dir" : "dir",
              "uri" : "/uri"
            }
        }
    ```
通配符可以过滤指定的文件，例如我们可以包括
- java
	```json
    	server.mount(dir, to("/uri"), include("*.txt"));
    ```
- json
	```json
    	{
          "mount" :
            {
              "dir" : "dir",
              "uri" : "/uri",
              "includes" :
                [
                  "*.txt"
                ]
            }
        }
	```
排除也可以
- java
	```java
    	server.mount(dir, to("/uri"), exclude("*.txt"));
    ```
- json
	```json
    	{
          "mount" :
            {
              "dir" : "dir",
              "uri" : "/uri",
              "excludes" :
                [
                  "*.txt"
                ]
            }
        }
    ```
也可以组合用
- java
	```java
    	server.mount(dir, to("/uri"), include("a.txt"), exclude("b.txt"), include("c.txt"));
    ```
- json
	```json
    	{
          "mount" :
            {
              "dir" : "dir",
              "uri" : "/uri",
              "includes" :
                [
                  "a.txt",
                  "c.txt"
                ],
              "excludes" :
                [
                  "b.txt"
                ]
            }
        }
    ```
您还可以指定一些响应信息，如正常响应
- json
	```json
    	{
          "mount" :
            {
              "dir" : "dir",
              "uri" : "/uri",
              "headers" : {
                "Content-Type" : "text/plain"
              }
            }
        }
    ```

## Template(Beta)
### Version
使用req.version，可以在模板中检索请求版本。
- java
	```java
    	server.request(by(uri("/template"))).response(template("${req.version}"));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": "${req.version}"
                }
            }
        }
    ```

### Method
请求方法由req.method标识
- java
	```java
    	server.request(by(uri("/template"))).response(template("${req.method}"));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": "${req.method}"
                }
            }
        }
	```
### Content
所有请求内容都可以在带有req.content的模板中使用
- java
	```java
    	server.request(by(uri("/template"))).response(template("${req.content}"));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": "${req.content}"
                }
            }
        }
    ```

### Header
消息头是模板中的另一个重要元素，我们可以使用req.headers作为消息头。
- java
	```java
    	server.request(by(uri("/template"))).response(template("${req.headers['foo']"));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": "${req.headers['foo']}"
                }
            }
        }
    ```

### Query
req.queries帮助我们提取请求查询。
- java
	```java
    	server.request(by(uri("/template"))).response(template("${req.queries['foo']"));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": "${req.queries['foo']}"
                }
            }
        }
    ```

### Form
req.forms可以从请求中提取表单值。
- java
	```java
    	server.request(by(uri("/template"))).response(template("${req.forms['foo']"));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": "${req.forms['foo']}"
                }
            }
        }
	```

### Cookie
来自请求的Cookie可以由req.cookies提取。
- java
	```java
    	server.request(by(uri("/template"))).response(template("${req.cookies['foo']"));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": "${req.cookies['foo']}"
                }
            }
        }
    ```

### Custom Variable自定义变量
您可以在模板中提供自己的变量。
- java
	```java
    	server.request(by(uri("/template"))).response(template("${foo}", "foo", "bar"));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": {
                        "with" : "${foo}",
                        "vars" : {
                            "foo" : "bar"
                        }
                    }
                }
            }
        }
    ```

您还可以使用提取器从请求中提取信息。
- java
	```java
    	server.request(by(uri("/template"))).response(template("${foo}", "foo", jsonPath("$.book[*].price")));
    ```
- json
	```json
    	{
            "request": {
                "uri": "/template"
            },
            "response": {
                "text": {
                    "template": {
                        "with" : "${foo}",
                        "vars" : {
                            "foo" : {
                              "json_path": "$.book[*].price"
                            }
                        }
                    }
                }
            }
        }
    ```
其它提取器，例如。 xpath也可以。

### Redirect(重定向)
- java
	```java
    	server.request(by(uri("/redirectTemplate"))).redirectTo(template("${var}", "var", ""https://github.com"));
    ```
- json
	```json
    	{
          "request" :
          {
              "uri" : "/redirect-with-template"
          },

          "redirectTo" : {
              "template" : {
                  "with" : "${url}",
                  "vars" : {
                      "url" : "https://github.com"
                  }
              }
          }
      }
    ```

### File Name Template
模板也可以用在文件名中，因此响应可以根据不同的请求而不同。
- java
	```java
    	server.response(file(template("${req.headers['foo'].txt")));
    ```
- json
	```json
    	[
          {
            "response": {
              "file": {
                "name": {
                  "template": "${req.headers['foo'].txt"}"
                }
              }
            }
          }
        ]
    ```

## Event
### Complete
完成事件将在您的请求完全处理后触发。
- java
	```java
    	server.request(by(uri("/event"))).response("event").on(complete(get("http://another_site")));
    ```
- json
	```json
    	{
            "request": {
                "uri" : "/event"
            },
            "response": {
                "text": "event"
            },
            "on": {
                "complete": {
                    "get" : {
                        "url" : "http://another_site"
                    }
                }
            }
        }
    ```
也可以发布一些内容。
- java
	```java
    	server.request(by(uri("/event"))).response("event").on(complete(post("http://another_site", "content")));
    ```
- json
	```json
    	{
            "request": {
                "uri" : "/event"
            },
            "response": {
                "text": "event"
            },
            "on": {
                "complete": {
                    "post" : {
                        "url" : "http://another_site",
                        "content": "content"
                    }
                }
            }
        }
    ```

### Asynchronous(异步)
默认情况下使用同步请求，这意味着在事件处理程序完成之前，不会将响应返回到客户端。

如果这不是您的预期行为，您可以更改异步API，将异步地触发事件。
- java
	```java
    	server.request(by(uri("/event"))).response("event").on(complete(async(post("http://another_site", "content"))));
    ```
- json
	```json
    	{
            "request": {
                "uri" : "/event"
            },
            "response": {
                "text": "event"
            },
            "on": {
                "complete": {
                    "async" : "true",
                    "post" : {
                        "url" : "http://another_site",
                        "content": "content"
                    }
                }
            }
        }
    ```
您可以指定此异步请求的延迟，并等待一段时间。
- java
	```java
    	server.request(by(uri("/event"))).response("event").on(complete(async(post("http://another_site", "content"), latency(1000))));
    ```
- json
	```json
    	{
            "request": {
                "uri" : "/event"
            },
            "response": {
                "text": "event"
            },
            "on": {
                "complete": {
                    "async" : "true",
                    "latency" : 1000,
                    "post" : {
                        "url" : "http://another_site",
                        "content": "content"
                    }
                }
            }
        }
    ```

## Verify
有人可能想要验证在测试框架中向服务器发送了什么类型的请求。

您可以这样验证请求：
```java
	RequestHit hit = requestHit();
    final HttpServer server = httpServer(12306, hit);
    server.get(by(uri("/foo"))).response("bar");

    running(server, new Runnable() {
      @Override
      public void run() throws Exception {
        assertThat(helper.get(remoteUrl("/foo")), is("bar"));
      }
    });

    hit.verify(by(uri("/foo"))), times(1));
```
您还可以验证意外的请求，如下所示：
```java
	hit.verify(unexpected(), never());
```
可用的验证:
- never: 没有发送这种类型的请求。
- once: 只有这种请求已发送。
- time: 此类请求已发送多少次。
- atLeast: 至少已发送此类请求的次数。
- atMost: 至少这种类型的请求已发送多少时间。
- between: 此类请求已发送的次数应介于最小和最大次数之间。

## Miscellaneous
### Port
如果为存根服务器指定端口，则意味着在启动服务器时端口必须可用。 这不是有时候的情况。

Moco为您提供了另一种启动服务器的方法：没有指定端口，它将查找可用端口。 端口可以通过port（）方法获取。 示例如下：
```java
	final HttpServer server = httpServer();
    server.response("foo");

    running(server, new Runnable() {
      @Override
      public void run() throws Exception {
        Content content = Request.Get("http://localhost:" + server.port()).execute().returnContent();
        assertThat(content.asString(), is("foo"));
      }
    });
```
该端口将仅在服务器启动时返回，否则将抛出异常。

对于独立服务器，如果您需要此行为，只是不给端口参数。
```shell
	java -jar moco-runner-<version>-standalone.jar start -c foo.json
```
端口信息将显示在屏幕上。

### Log
如果你想知道更多关于你的Moco服务器如何运行，日志将是你的帮手。
```java
	final HttpServer server = httpServer(log());
```
Moco服务器将在控制台中记录您的所有请求和响应。
要保留日志，可以使用日志界面如下：
```java
	final HttpServer server = httpServer(log("path/to/log.log"));
```
日志内容可能包含一些非UTF-8字符，可以在日志API中指定字符集：
```java
	final HttpServer server = httpServer(log("path/to/log.log", Charset.forName("GBK")));
```
日志将保存在日志文件中。
使用验证器登录

日志将帮助一些遗留系统知道什么详细的request/response。 你还需要做一些验证工作。 这是这种情况。
```java
	RequestHit hit = requestHit();
	final HttpServer server = httpServer(port(), hit, log());
```