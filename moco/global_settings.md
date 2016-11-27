# 全局设置
我们可以将所有配置放在一个单一的配置文件中。 但是如果我们想在单个Moco实例中存根许多服务，配置文件将是巨大的。

在这种情况下，我们可以使用设置文件将我们的配置分为不同的配置文件。
假设我们有两个服务stub：
```json
	[
        {
            "request" : {
                "uri" : "/foo"
            },
            "response" : {
                "text" : "foo"
            }
        }
    ]
```
和
```json
	[
        {
            "request" : {
                "uri" : "/bar"
            },
            "response" : {
                "text" : "bar"
            }
        }
    ]
```
现在，我们可以编写一个设置文件来组合这两个配置：
```json
	[
        {
            "include" : "foo.json"
        },
        {
            "include" : "bar.json"
        }
    ]
```
启动服务器使用此设置：
```shell
	java -jar moco-runner-<version>-standalone.jar start -p 12306 -g settings.json
```

## Configuration
实际上，有一些设置配置可以简化您的配置。
### Context(上下文)
我们可以将一个服务的所有响应放在指定的上下文中：
```json
	[
        {
            "context": "/foo",
            "include": "foo.json"
        },
        {
            "context": "/bar",
            "include": "bar.json"
        }
    ]
```
现在foo.json中的所有配置必须由/foo上下文访问。 在这种情况下，当你访问http：//localhost：12306/foo/foo时，你会得到“foo”。
另一方面，bar.json将在/bar上下文中，这意味着http：//localhost：12306/bar/bar将返回“bar”。

### File Root
如果您的配置中有许多文件API，file_root设置有助于缩短配置。顾名思义file_root设置将作为文件的根目录进行配置.因此,所有文件api都可以用作相对路劲
```json
	[
        {
            "file_root": "fileroot",
            "include": "fileroot.json"
        }
    ]
```
现在，包括设置和文件API使用相对路径。 在这种情况下，fileroot/fileroot.json将用作于它所包含的文件.
```json
	[
        {
            "request" : {
                "uri" : "/fileroot"
            },
            "response" : {
                "file" : "foo.response"
            }
        }
    ]
```
(fileroot/fileroot.json)
当请求被启动时，将返回src/test/resources/foo.response。

### 环境变量
对于某些不同的情况，您有不同的配置。 例如，您的代码可能访问远程服务器的代理和模拟服务器进行本地测试。

现在，environment将帮助您在一个设置中配置所有相关的配置。
```json
	[
        {
            "env" : "remote",
            "include": "foo.json"
        },
        {
            "env" : "local",
            "include": "bar.json"
        }
    ]
```
(env.json)
您可以使用与CLI不同的环境启动服务器。
```shell
	java -jar moco-runner-<version>-standalone.jar start -p 12306 -g env.json -e remote
```
现在，当你访问你的服务器，所有配置与“远程”的环境岩石！

在这种情况下，http：// localhost：12306 / foo将给你“foo”，但http：// localhost：12306 / bar将不会返回任何内容。

### Request
在某些情况下，您可能需要一个全局请求匹配器，例如，一些REST API的令牌，实际上是请求参数。 全局请求会帮助你。
```json
	[
      {
        "request" : {
          "headers" : {
            "foo" : "bar"
          }
        },
        "include": "blah.json"
      }
    ]
```
在这种情况下，您将不会收到响应，直到您的请求有“foo”，“bar”标题。

#### Response
在某些情况下，您可能需要为所有响应设置全局响应，例如HTTP版本或HTTP标头，因此您不必为每个响应都设置它。
```json
	[
        {
            "response" : {
                "headers" : {
                    "foo" : "bar"
                }
            },
            "include": "blah.json"
        }
    ]
```
当您向服务器发出任何请求时，它将返回带有“foo”，“bar”标题的响应。