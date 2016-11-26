# Socket APIs
Moco主要关注服务器配置.目前只有两种API：request和response

这意味着我们如果得到预期的请求,就会返回预定的响应.
>下面的json配置只是一对request和response的代码片段,而不是整个配置文件。

## Java API复合设计
Moco Java API是以功能性方式设计的，这意味着您可以轻松地组合任何请求或响应。
```java
    server.request(and(by(uri("/target")), by(version(VERSION_1_0)))).response(with(text("foo")), header("Content-Type", "text/html"));
```

## description说明
在所有JSON API中，可以使用description描述此会话的内容。 它只是用作注释，在运行时被忽略。
```json
    [
        {
            "description": "any response",
            "response": {
                "text": "foo"
            }
        }
    ]
```

## Request
### Content
如果要根据请求内容进行响应，Moco服务器可以配置如下：
- java api
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

如果请求内容太大，可以将其放在一个文件中：
- Java API
	```java
    	server.request(by(file("foo.request"))).response("bar");
    ```
- JSON
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

### XML
XML是Web服务的流行格式。 当请求是XML时，在大多数情况下只有XML结构是重要的，并且可以忽略空格。 对于这种情况，可以使用xml运算符。
- Java API
	```java
    	server.request(xml(text("<request><parameters><id>1</id></parameters></request>"))).response("foo");
    ```
- JSON
	```json
        {
          "request":
            {
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
注意：请在文本中转义引用。
大型请求可以放入一个文件：
```json
    {
       "request":
         {
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
- Java API
	```java
    	server.request(eq(xpath("/request/parameters/id/text()"), "1")).response("bar");
    ```
- JSON
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
#### JSON Text
- Java Api
	```java
    	server.request(json(text("{\"foo\":\"bar\"}"))).response("foo");
    ```
- JSON
	```json
        {
          "request":
            {
              "text":
                {
                  "json": "{\"foo\":\"bar\"}"
                }
            },
          "response":
            {
              "text": "foo"
            }
        }
	```
注意:引用要再文本中转换

#### JSON文本快捷方式
这是json快捷方式
```json
    {
        "request": {
            "json": {
                "foo": "bar"
            }
        },
        "response": {
            "text": "foo"
        }
    }
```

#### JSON File
大量请求可以放入一个文件：
- java api
	```java
    	server.request(json(file("your_file.json"))).response("foo");
    ```
- json
	```json
    	{
          "request":
            {
              "file":
                {
                  "json": "your_file.json"
                }
            },
          "response":
            {
              "text": "foo"
            }
        }
    ```

### JSONPath
对于JSON/HTML请求，Moco允许我们将请求与JSONPath匹配。
- Java Api
	```java
    	server.request(eq(jsonPath("$.book[*].price"), "1")).response("response_for_json_path_request");
    ```
- JSON
	```json
    	{
          "request":
            {
              "json_paths":
                {
                  "$.book[*].price": "1"
                }
            },
          "response":
            {
              "text": "response_for_json_path_request"
            }
        }
    ```

### Operator
Moco还支持一些Operator，可以帮助您轻松写下您的期望。

#### Match
正则表达式
- java api
	```java
    	server.request(match(text("/\\w*/foo"))).response("bar");
    ```
- json
	```json
        {
          "request":
            {
              "text":
                {
                  "match": "/\\w*/foo"
                }
            },
          "response":
            {
              "text": "bar"
            }
        }
    ```
Moco是由Java正则表达式实现的，你可以在这里参考更多的细节。

#### Starts With
startsWith操作符可以帮助您确定请求信息是否以一段文本开头。
- java api
	```java
    	server.request(startsWith(text("/foo"))).response("bar");
    ```
- json
    ```json
        {
          "request":
            {
              "text":
                {
                  "startsWith": "/foo"
                }
            },
          "response":
            {
              "text": "bar"
            }
        }
    ```

#### Ends With
endsWith操作符可以帮助您确定请求信息是否以一段文本结束。
- java api
	```java
    	server.request(endsWith(text("foo"))).response("bar");
    ```
- json
    ```json
        {
          "request":
            {
              "text":
                {
                  "endsWith": "foo"
                }
            },
          "response":
            {
              "text": "bar"
            }
        }
    ```
#### Contain
contains运算符可帮助您了解请求信息是否包含一段文本。
- java api
	```java
    	server.request(contain(text("foo"))).response("bar");
    ```
- JSON
	```json
    	{
          "request":
            {
              "text":
                {
                  "contain": "foo"
                }
            },
          "response":
            {
              "text": "bar"
            }
        }
    ```

#### Exist
exists运算符用于决定请求信息是否存在。
- java api
	```java
    	server.request(exist(header("foo"))).response("bar");
    ```
- JSON
	```json
        {
          "request":
            {
              "headers": {
                "foo": {
                  "exist" : "true"
                }
            },
          "response":
            {
              "text": "bar"
            }
        }
    ```
对于JSON API，您可以决定该信息是否不存在：
```json
    {
      "request":
        {
          "headers": {
            "foo": {
              "exist" : "not"
            }
        },
      "response":
        {
          "text": "bar"
        }
    }
```

### Response
#### Content
正如你在前面的例子中看到的，响应内容是很容易的。
- Java API
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

#### Latency
有时，我们需要一个延迟来模拟缓慢的服务器端操作。
- Java API
    ```java
        server.request(by("foo")).response(latency(5000));
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
              "latency" : 5000
            }
        }
	```
以时间单位设置延迟也很容易:
	```java
    	server.response(latency(1, TimeUnit.SECONDS));
    ```

#### Sequence
有时，我们想要模拟一个改变服务器端资源的真实操作。 例如：
- 首次请求资源并返回“foo”
- 我们更新此资源
- 再次请求相同的网址，更新的内容，例如 “bar”是预期的。

我们可以这样做:
```java
	server.request(by(text("/foo"))).response(seq("foo", "bar", "blah"));
```

#### JSON Response
JSON响应是没有Java API的API，因此如果响应是json，我们不必使用转义字符编写json。 提示，json api也将设置Content-Type头。
```json
	{
        "request": {
            "text": "json"
        },
        "response": {
            "json": {
                "foo" : "bar"
            }
        }
    }
```

### Template(Beta)
注意：模板是一个实验功能，将来可能会更改很多。 随意告诉它如何帮助或你需要更多的功能在模板中。

有时，我们需要根据某些内容，例如， 响应应该与请求具有相同的头。

目标可以通过模板达到：

#### Content
所有请求内容可以在模板中使用“req.content”
- java
	```java
    	server.request(by(text("template"))).response(template("${req.content}"));
    ```
- Json
	```json
        {
            "request": {
                "text": "template"
            },
            "response": {
                "text": {
                    "template": "${req.content}"
                }
            }
        }
	```

#### 自定义变量
您可以在模板中提供自己的变量。
- java
	```java
    	server.request(by(text("template"))).response(template("${'foo'}", "foo", "bar"));
    ```
- json
	```json
    	{
            "request": {
                "text": "template"
            },
            "response": {
                "text": {
                    "template": {
                        "with" : "${'foo'}",
                        "vars" : {
                            "foo" : "bar"
                        }
                    }
                }
            }
        }
    ```
您还可以使用提取器从请求中提取信息。
- java api
	```java
    	server.request(by(text("template"))).response(template("${'foo'}", "foo", jsonPath("$.book[*].price")));
    ```
- json
	```json
    	{
            "request": {
                "text": "template"
            },
            "response": {
                "text": {
                    "template": {
                        "with" : "${'foo'}",
                        "vars" : {
                            "foo" : {
                              "json_paths": "$.book[*].price"
                            }
                        }
                    }
                }
            }
        }
    ```

#### File Name Template
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
                  "template": "${req.content}.txt")"
                }
              }
            }
          }
        ]
    ```