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