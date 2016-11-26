# JUnit集成
Moco利用JUnit中的测试规则来简化JUnit集成。 MocoJunitRunner提供了多种方法来运行Moco服务器作为测试规则，它可以在测试之前启动Moco服务器，并在测试后停止。

## HTTP Server
### POJO HTTP Server
httpRunner可以引用一个HttpServer对象。
```java
	public class MocoJunitPojoHttpRunnerTest {
      private static HttpServer server;

      static {
        server = httpServer(12306);
        server.response("foo");
      }

      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.httpRunner(server);

      ...
    }
```

### JSON HTTP Server
jsonHttpRunner可以将JSON文件引用作为HTTP服务器。
```java
	public class MocoJunitJsonHttpRunnerTest {
      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.jsonHttpRunner(12306, "foo.json");

      ...
    }
```

JSON配置可以从classpath中检索。
```java
	public class MocoJunitJsonHttpRunnerTest {
      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.jsonHttpRunner(12306, Moco.pathResource("foo.json"));

      ...
    }
```


## HTTPS Server
### POJO HTTPS服务器
httpsRunner可以引用一个HttpsServer对象。
```java
	public class MocoJunitPojoHttpRunnerTest {
      private static final HttpsCertificate DEFAULT_CERTIFICATE = certificate(pathResource("cert.jks"), "mocohttps", "mocohttps");
      private static HttpServer server;

      static {
        server = httpsServer(12306, DEFAULT_CERTIFICATE);
        server.response("foo");
      }

      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.httpsRunner(server);

      ...
    }
```

### JSON HTTPS Server
jsonHttpsRunner可以将JSON文件引用为HTTP服务器。
```java
	public class MocoJunitJsonHttpRunnerTest {
      private static final HttpsCertificate DEFAULT_CERTIFICATE = certificate(pathResource("cert.jks"), "mocohttps", "mocohttps");
      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.jsonHttpsRunner(12306, "foo.json", DEFAULT_CERTIFICATE);

      ...
    }
```
JSON配置可以从classpath中检索。
```java
	public class MocoJunitJsonHttpRunnerTest {
      private static final HttpsCertificate DEFAULT_CERTIFICATE = certificate(pathResource("cert.jks"), "mocohttps", "mocohttps");
      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.jsonHttpsRunner(12306, Moco.pathResource("foo.json"), DEFAULT_CERTIFICATE);

      ...
    }
```

## Socket Server
### POJO Socket Server
socketRunner可以引用SocketServer对象。
```java
	public class MocoJunitPojoSocketRunnerTest {
      private static SocketServer server;

      static {
        server = socketServer(12306);
        server.response("bar\n");
      }

      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.socketRunner(server);

      ...
    }
```

### JSON Socket Server
jsonHttpRunner可以将JSON文件引用为Socket服务器。
```java
	public class MocoJunitJsonSocketRunnerTest {
      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.jsonSocketRunner(12306, "foo.json");

      ...
    }
```
JSON配置可以从classpath中检索。
```java
	public class MocoJunitJsonHttpRunnerTest {
      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.jsonSocketRunner(12306, Moco.pathResource("foo.json"));

      ...
    }
```

## Rest Server
### POJO Rest Server
restRunner可以引用RestServer对象。
```java
	public class MocoJunitPojoRestRunnerTest {
      private static RestServer server;

      static {
        server = restServer(12306);
        server.resource("targets",
          post().response(status(201), header("Location", "/targets/123"))
        );
      }

      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.restRunner(server);

      ...
    }
```

### JSON Rest Server
jsonRestRunner可以将JSON文件引用为HTTP服务器。
```java
	public class MocoJunitJsonRestRunnerTest {
      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.jsonRestRunner(12306, "rest.json");

      ...
    }
```

JSON配置可以从classpath中检索。
```java
	public class MocoJunitJsonRestRunnerTest {
      @Rule
      public MocoJunitRunner runner = MocoJunitRunner.jsonRestRunner(12306, Moco.pathResource("foo.json"));

      ...
    }
```

