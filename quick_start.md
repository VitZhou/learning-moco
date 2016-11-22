快速开始
======
1. 点击[这里](https://repo1.maven.org/maven2/com/github/dreamhead/moco-runner/0.11.0/moco-runner-0.11.0-standalone.jar)下载Standalone Moco Runner
1. 编写自己的配置文件来描述Moco服务器配置，如下所示：
	```json
    	[
          {
            "response" :
              {
                "text" : "Hello, Moco"
              }
          }
        ]
    ```
    (foo.json)

1. 基于配置文件运行Moco HTTP服务
	```shell
    	java -jar moco-runner-<version>-standalone.jar http -p 12306 -c foo.json
    ```
1. 现在你可以在你的浏览器中输入 http://localhost:12306访问后你会看到"Hello,Moco"