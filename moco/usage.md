# 用法
Moco有几种使用方式:一种是API,你可以在单元测试中使用它。另外一个是独立运行Moco。一般,你需要将所有的配置放在json文件中
另外,Moco有几种不同的方法与一些工具集成:maven插件,gradle插件和shell支持

## 依赖
### maven核心依赖:
```xml
	<dependency>
      <groupId>com.github.dreamhead</groupId>
      <artifactId>moco-core</artifactId>
      <version>0.11.0</version>
    </dependency>
```

### gradle依赖:


```xml
repositories {
  mavenCentral()
}

dependencies {
  testCompile(
    "com.github.dreamhead:moco-core:0.11.0",
  )
}
```

## API 例子
下面是一个JUnit中典型的Moco的测试用例:
```java
    import org.junit.Test;
    import java.io.IOException;
    import com.github.dreamhead.moco.HttpServer;
    import org.apache.http.client.fluent.Content;
    import org.apache.http.client.fluent.Request;
    import com.github.dreamhead.moco.Runnable;

    import static com.github.dreamhead.moco.Moco.*;
    import static com.github.dreamhead.moco.Runner.*;
    import static org.hamcrest.core.Is.is;
    import static org.junit.Assert.assertThat;

    @Test
    public void should_response_as_expected() throws Exception {
        HttpServer server = httpServer(12306);
        server.response("foo");

        running(server, new Runnable() {
            @Override
            public void run() throws IOException {
                Content content = Request.Get("http://localhost:12306").execute().returnContent();
                assertThat(content.asString(), is("foo"));
            }
        });
    }
```
如上所示,我们创建了一个新的服务器并配置它的预期值.然后对这个服务器运行我们的测试.
在这里，我们使用[Apache Http Client Fluent API](http://hc.apache.org/httpcomponents-client-ga/tutorial/html/fluent.html)请求我们的测试服务器。

## Runner API
你可能需要自己控制服务器的运行/停止.例如:在before(或者setup)方法中使用runner api
```java
    import org.junit.After;
    import org.junit.Before;
    import org.junit.Test;

    import java.io.IOException;

    import static com.github.dreamhead.moco.Moco.httpServer;
    import static com.github.dreamhead.moco.Runner.runner;
    import static org.hamcrest.CoreMatchers.is;
    import static org.junit.Assert.assertThat;

    public class MocoRunnerTest {
        private Runner runner;

        @Before
        public void setup() {
            HttpServer server = httpServer(12306);
            server.response("foo");
            runner = runner(server);
            runner.start();
        }

        @After
        public void tearDown() {
            runner.stop();
        }

        @Test
        public void should_response_as_expected() throws IOException {
            Content content = Request.Get("http://localhost:12306").execute().returnContent();
            assertThat(content.asString(), is("foo"));
        }
    }
```

## 独立运行
Moco可以独立运行或配置,你可以直接下载:[Standalone Moco Runner](https://repo1.maven.org/maven2/com/github/dreamhead/moco-runner/0.11.0/moco-runner-0.11.0-standalone.jar)

首先，需要提供一个JSON配置文件来启动Moco.
```json
    [
      {
        "response" :
          {
            "text" : "foo"
          }
      }
    ]
```
(foo.json)


执行脚本运行Moco服务：
```shell
	java -jar moco-runner-<version>-standalone.jar http -p 12306 -c foo.json
```
在浏览器中访问localhost:12306就会看到foo了·

## Java API中的JSON配置
Mock还支持java api配置服务器
```java
    import static com.github.dreamhead.moco.Moco.file;
    import static com.github.dreamhead.moco.Moco.pathResource;
    import static com.github.dreamhead.moco.MocoJsonRunner.jsonHttpServer;
    import static com.github.dreamhead.moco.Runner.running;
    import static com.github.dreamhead.moco.helper.RemoteTestUtils.port;
    import static com.github.dreamhead.moco.helper.RemoteTestUtils.root;
    import static org.hamcrest.CoreMatchers.is;
    import static org.junit.Assert.assertThat;

    public class MocoJsonHttpRunnerTest extends AbstractMocoStandaloneTest {
        @Test
        public void should_return_expected_response() throws Exception {
            final HttpServer server = jsonHttpServer(12306, file("foo.json"));
            running(server, new Runnable() {
                @Override
                public void run() throws Exception {
                    Content content = Request.Get("http://localhost:12306").execute().returnContent();
                    assertThat(content.asString(), is("foo"));
                }
            });
        }
    }
```

## HTTPS
HTTPS也是HTTP协议的主流用法。 Moco也支持HTTPS。 API的主要区别是HTTPS需要证书。 另一方面，应该使用httpsServer。
```java
    final HttpsCertificate certificate = certificate(pathResource("cert.jks"), "mocohttps", "mocohttps");
    final HttpsServer server = httpsServer(certificate, hit);
```
如果要对独立服务器使用HTTPS。 证书信息可以作为CLI参数提供。
```shell
	java -jar moco-runner-<version>-standalone.jar https -p 12306 -c foo.json --https /path/to/cert.jks --cert mocohttps --keystore mocohttps
```

## Socket
Socket是一个通用的集成通道。 只有内容可用在套接字。
```java
	final SocketServer server = socketServer(12306);
```
也可以在独立服务器中使用套接字。
```shell
	java -jar moco-runner-<version>-standalone.jar socket -p 12306 -c foo.json
```
更多socket api可以访问[这里](https://github.com/dreamhead/moco/blob/master/moco-doc/socket-apis.md)

