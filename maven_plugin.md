# Maven plugin
### Maven-shade-plugin vs Maven-assembly plugin
dependencies 가 있고 실행 가능한 `jar` 파일을 만들고자 할때, `maven-assembly-plugin` 이나 `maven-shade-plugin` 을 사용할 수 있다. 
둘 다 의존성을 포함한 하나의 `jar` 실행 파일을 생성하지만, 약간의 차이가 있다.

### maven-assembly-plugin
`maven-assembly-plugin`은 필요한 모든 의존성을 `jar` 파일에 자동으로 복사한다. `jar` 파일 내부에 의존성 파일이 존재 하므로 하나의 `jar` 
실행파일만으로 프로세스를 실행할 수 있다. 그렇지만 클래스 **relocation** 을 지원하지는 않는다. 다시말해, 동일한 파일명의 리소스 파일이 존재할 때, 
파일이 덮여쓰여지는 문제가 있을 수 있다.
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
            <configuration>
                <archive>
                <manifest>
                    <!-- information of main class -->
                    <mainClass>
                        com.grace.Main
                    </mainClass>
                </manifest>
                </archive>
                <descriptorRefs>
                    <!-- example output name is `core-java-jar-with-dependencies.jar`. -->
                    <descriptorRef>jar-with-dependencies</descriptorRef> 
                </descriptorRefs>
            </configuration>
        </execution>
    </executions>
</plugin>
```

### maven-shade-plugin
`maven-shade-plugin`도 마찬가지로 모든 dependencies 파일을 하나의 `jar` 로 패키징할 수 있다. 또한 특정 파일의 내용을 덮어쓰지 않고 merge 하므로 
`jar` 에서 동일한 이름을 가진 리소스 파일이 있고, 모든 리소스 파일을 패키징하려고 할 때 유용하게 쓸 수 있다. 
대신 configuration 이 약간 복잡해질 수 있는 단점이 있다. 
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <executions>
        <execution>
            <goals>
                <goal>shade</goal>
            </goals>
            <configuration>
                <!-- all dependencies to be packaged into the jar -->
                <shadedArtifactAttached>true</shadedArtifactAttached>
                <transformers>
                    <transformer implementation=
                      "org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <!-- specify the main class of our application -->
                        <mainClass>com.grace.Main</mainClass>
                </transformer>
            </transformers>
        </configuration>
        </execution>
    </executions>
</plugin>
```
