# 스프링 Arguments

스프링에서 Arguments를 이렇게 받을 수 있나보다. 어디에 쓸진모르겠지만 필요할 때가 있을것 같다.

```java

  @Autowired
  ApplicationArguments arguments;


  //----생략----

  /**
  *  --hello=Hello --hello=world => ["Hello", "world"]   이렇게 리턴
  **/
  List<String> values = arguments.getOptionValues("hello");

```


또는

```java

  @Value("${hello}")
  String[] helloValues;

  /**
  *  --hello=Hello --hello=world => ["Hello", "world"]  => "Hello, world" 이렇게 리턴
  **/
  Arrays.stream(helloValues).collect(Collectors.joining(", "));

```

동일하게 Arguments를 받아올 수 있겠다.


## CommandLineRunner & ApplicationRunner
두 인터페이스 모두 같은방법으로 동작한다. 차이는 아래와 같다. 

```java
public class MYBean implements CommandLineRunner {

  public void run(String... args) {
    // Do something...
  }
}
```

```java

@Autowired
public class MYBean implements ApplicationRunner {

  public void run(ApplicationArguments args) {
    // Do something...
  }
}
```

당연히 날것의 String보다는 아래의 ApplicationRunner을 사용하는것이 편리하겠다.
