# Maven插件
这是一个用于管理Moco服务器的简单Maven插件。 它可以用于根据需要一致地运行Moco服务器，或者在Maven生命周期的某些部分（例如集成测试）期间自动启动服务器。

## 使用
要开始，请将插件添加到pom.xml。
```xml
	<plugin>
        <groupId>com.garrettheel</groupId>
        <artifactId>moco-maven-plugin</artifactId>
        <version>0.9.3</version>
        <configuration>
            <port>8081</port>
            <configFile>config.json</configFile>
        </configuration>
    </plugin>
```
您可能还需要添加Sonatype存储库，具体取决于您的环境。
```xml
	<pluginRepositories>
        <pluginRepository>
            <id>sonatype</id>
            <url>https://oss.sonatype.org/content/repositories/public/</url>
        </pluginRepository>
    </pluginRepositories>
```

## 手动运行
手动启动服务器很容易！ 只需运行以下命令：
```shell
	mvn com.garrettheel:moco-maven-plugin:run
```
这将无限期运行服务器，直到进程终止。

## 在maven生命周期中运行
您还可以将maven配置为在构建生命周期中启动和停止Moco服务器。 例如，以下配置将支持使用服务器进行集成测试：
```xml
	<plugin>
        <groupId>com.garrettheel</groupId>
        <!-- ... -->
        <executions>
            <execution>
                <id>start-moco</id>
                <phase>pre-integration-test</phase>
                <goals>
                    <goal>start</goal>
                </goals>
            </execution>
            <execution>
                <id>stop-moco</id>
                <phase>post-integration-test</phase>
                <goals>
                    <goal>stop</goal>
                </goals>
            </execution>
        </executions>
    </plugin>
```
