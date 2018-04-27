
## Jconsole

Properties에 아래 내용을 추가하고
```
spring.application.admin.enabled=true
```

그냥 어플리케이션 실행 후
```
$ jconsole
```

이건 사용해보면 모니터링으로 좋을것 같다. remote로도 프로그램을 켜고 끄고 할 수 있다.


## properties


#### 우선순위
세세하게는 더 있지만 내가 직접 조정할 것으로 보이는 것만 정리를 해보면,

우선 별로의 테스트를 위한 properties 이것은 0순위다.
* Devtools global settings properties
  * `~/.spring-boot-devtools.properties`

1. Command line arguments `java -jar application.jar --name=sunkyung`
2. Java System properties `java -jar -Dname=springboot application.jar`
3. OS enviroment variables.
4. application.{profile}.properties
5. application.properties

그리고 흔히 사용하는 application.properties 를 읽을 때에는
1. `/config`
2. 메인 Application이 실행되는 현재 디렉토리
3. classpath `/config` package
4. 기본 설정으로 생성되는 `/resource` 여기가 제일 마지막으로 읽는다.

설정을 통해 application.properties을 파일 명이나 다른경로를 설정할 수 있지만 혼란을 줄 수 있으므로 파일명이나 다른경로 설정은 사용하지 않는걸 추천한다.


그리고 properties를 이렇게도 활용할 수 있는데

```
server.port=${port:8080}
```

위와 같이 설정을 해놓으면
`java -jar application.jar --port=9000`와 같이 port로 Arguments가 있을때는 포트가 9000으로 띄우겠고, 없으면 8080으로 띄워진다.

## YAML
YAML은 내부적으로는 properties로 변환을 해서 사용하게 된다. application을 띄울때 변환하기 때문에 성능상에 문제가 심하게 나지 않을 것 같다.
properties와 같이 있다면 YAML을 먼저 읽을것이다.
