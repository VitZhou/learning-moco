# REST API
Restful服务是一种defacto服务设计风格。 Moco提供REST API，以便于实现静态服务桩。

## 基本用法
您将使用restServer创建一个rest服务器，如下所示：
```java
	RestServer server = restServer(port, log());

    ResourceObject resource = new ResourceObject();
    resource.code = 1;
    resource.message = "hello";

    server.resource("targets",
        get("1").response(toJson(resource))
    );
```
如上,你创建了一个名为targets的资源,如果在GET方法中使用/targets/1(1是资源id)访问此资源,将会返回一个资源对象。
注意:Restful服务默认也是一个http服务,因此HTTP api也可以用于rest服务器,正如你锁看到的，toJson是Moco的响应处理程序。
REST API也可在JSON API中使用。 可以在JSON API中创建相同的资源，如下所示：
```json
	[
        "resource": {
          "name": "targets",
          "get": [
            {
              "id": "1",
              "response": {
                "json": {
                  "code": 1,
                  "message": "foo"
                }
              }
            }
          ]
        }
    ]
```

##　复合REST设置
许多REST设置可以使用资源API添加，如下：
```java
	server.resource("targets",
        get("1").response(toJson(resource1)),
        get("2").response(toJson(resource2)),
        post().response(status(201), header("Location", "/targets/123"))
    );
```

##　请求与响应
在REST API设计中，ID用于标识指定的资源。 您可以使用字符串指定ID。
```java
    server.resource("targets",
        get("1").response(toJson(resource))
    );
```
或者，您可以匹配您的单个资源与ID匹配器。 目前，Moco REST API中提供anyId，这意味着以下设置将覆盖/targets/1和/targets/2，甚至是 /targets/anything。
```java
	server.resource("targets",
        get(anyId()).response(toJson(resource))
    );
```
也可以像json api中的id一样:
```json
	"resource": {
      "name": "targets",
      "get": [
        {
          "id": "*",
          "response": {
            "json": {
              "code": 1,
              "message": "foo"
            }
          }
        }
      ]
    }
```

## Methods
### GET
#### 根据id来GET
在REST API设计中，使用GET方法进行查询。 如果指定了ID，它将用于查询单个资源。 在GET方法中可以使用/targets/1访问以下设置。
- java
	```java
    	server.resource("targets",
            get("1").response(toJson(resource))
        );
	```
- json
	```json
    	"resource": {
          "name": "targets",
          "get": [
            {
              "id": "1",
              "response": {
                "json": {
                  "code": 1,
                  "message": "foo"
                }
              }
            }
          ]
        }
    ```

#### GET ALL
如果未指定id，则将返回所有相关资源。 在GET方法中可以使用/ targets访问以下设置。
- java
	```java
    	server.resource("targets",
            get().response(toJson(Arrays.asList(resource)))
        );
    ```
- json
	```json
    	"resource": {
          "name": "targets",
          "get": [
            {
              "response": {
                "json": {
                  "code": 1,
                  "message": "foo"
                }
              }
            }
          ]
        }
    ```

### POST
POST方法用于创建新资源。 在POST方法中可以使用/targets访问以下设置。
- java
    ```java
        server.resource("targets",
            post().response(status(201), header("Location", "/targets/123"))
        );
    ```
- json
	```json
    	"resource": {
          "name": "targets",
          "post": [
            {
              "response": {
                "status": 201,
                "headers" : {
                  "Location": "/targets/123"
                }
              }
            }
          ]
        }
    ```

### PUT
PUT方法用于更新指定的资源。 在PUT方法中可以使用/targets/1访问以下设置。
- java
	```java
    	server.resource("targets",
            put("1").response(status(200))
        );
    ```
- json
	```json
    	"resource": {
          "name": "targets",
          "put": [
            {
              "id": 1,
              "response": {
                "status": 200,
              }
            }
          ]
        }
    ```

### DELETE
DELETE方法用于删除指定的资源。 使用DELETE方法中的/targets/1可以访问以下设置。
- java
	```java
    	server.resource("targets",
            delete("1").response(status(200))
        );
	```
- json
	```json
    	"resource": {
          "name": "targets",
          "delete": [
            {
              "id": 1,
              "response": {
                "status": 200,
              }
            }
          ]
        }
    ```

### HEAD
#### HEAD with ID
HEAD方法用于查询资源元数据。 如果指定了ID，它将用于查询单个资源。 使用HEAD方法中的/targets/1可以访问以下设置。
- java
	```java
    	server.resource("targets",
            head("1").response(header("ETag", "Moco"))
        );
    ```
- json
	```json
    	"resource": {
          "name": "targets",
          "get": [
            {
              "id": "1",
              "response": {
                "headers": {
                  "ETag": "Moco"
                }
              }
            }
          ]
        }
    ```

#### HEAD ALL
如果未指定id，则将返回所有相关的资源元数据。 可以使用HEAD方法中的/targets访问以下设置。
- java
	```java
    	server.resource("targets",
            head().response(header("ETag", "Moco"))
        );
    ```
- json
	```json
    	"resource": {
          "name": "targets",
          "get": [
            {
              "response": {
                "headers": {
                  "ETag": "Moco"
                }
              }
            }
          ]
        }
    ```

### PATCH
PATCH方法用于更新指定的资源。 在PATCH方法中可以使用/targets/1访问以下设置。
- java
	```java
    	server.resource("targets",
            patch("1").response(status(200))
        );
	```
- json
	```json
    	"resource": {
          "name": "targets",
          "patch": [
            {
              "id": 1,
              "response": {
                "status": 200,
              }
            }
          ]
        }
    ```

### Sub-resource
子资源用于构建资源的关系。 在GET方法中可以使用/targets/1/subs/1访问以下设置。
- java
	```java
    	server.resource("targets",
            id("1").name("subs").settings(
              get("1").response(toJson(resource)),
            )
        );
	```
- json
	```json
    	"resource": {
          "name": "targets",
          "resource": [
            {
              "id": "1",
              "name": "subs",
              "get": [
                {
                  "id": "1",
                  "response": {
                    "json": {
                      "code": 3,
                      "message": "sub"
                    }
                  }
                }
              ]
            }
          ]
        }
    ```
子资源中的REST设置与常规REST API相同。