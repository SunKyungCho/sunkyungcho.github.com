# 스프링 Dependency Injection

스프링을 완변히 이해하고 사용하고 있지 않다. 꾸준히 구멍을 매꿔나가는 중이다.

스프링에서 DI(Dependency Injection)을 사용하기 위해 다음과 같은 방법을 사용했다.
```java
@Autowired
private DependencyA dependencyA;

@Autowired
private DependencyB dependencyB;

@Autowired
private DependencyC dependencyC;
```
이런 주입 방법을 ***Field injection*** 이라고 부른다. ***@Autowired annotation*** 을 사용한 아주 간단한 주입 방식이다. 하지만 이런식의 필드 주입은 피하는 것이 좋다.
가장 중요한 이유는 DI 컨테이너와 결합이 매우 강하게 되어 외부에서 사용하기 어렵다는 것이다. 그로인해 Spring framework에 대한 의존성이 높아 유닛 테스팅이 힘들어 진다.
실제 유닛테스트를 위해 mockito를 사용하거나 spring context를 따로 생성하거나 전체를 불러서 실행하곤 했는데 경우에 따라서는 오래 걸리기도 했다.

***생성자 주입(constructor injection)*** 을 사용하는 것이 좋다. (스프링 4.3버전 이상부터)
### constructor injection
```java
private DependencyA dependencyA;
private DependencyB dependencyB;
private DependencyC dependencyC;

@Autowired
public DI(DependencyA dependencyA, DependencyB dependencyB, DependencyC dependencyC) {
    this.dependencyA = dependencyA;
    this.dependencyB = dependencyB;
    this.dependencyC = dependencyC;
}
```
### setter injection
```java
private DependencyA dependencyA;
private DependencyB dependencyB;
private DependencyC dependencyC;

@Autowired
public void setDependencyA(DependencyA dependencyA) {
    this.dependencyA = dependencyA;
}

@Autowired
public void setDependencyB(DependencyB dependencyB) {
    this.dependencyB = dependencyB;
}

@Autowired
public void setDependencyC(DependencyC dependencyC) {
    this.dependencyC = dependencyC;
}
```
대부분의 필수적인 종속성, 의존성을 가지고 있는 경우에는 construct injection을 사용하는 것이 적합하다. setter의 경우는 선택적인 종속성의 경우에 맞다. 경우에 따라 둘을 혼합해서 사용하면 될 것 같다. 생성자에 너무 많은 파라미터가 존재한다면 지저분해 보일 수 있다. 이 경우엔 오히려 단일 책임의 원칙을 위반하고 있는 것일 수 있다. 리팩토링을 고려해 보면 좋겠다.

## 마치며
필드 주입의 큰 문제점은 DI컨테이너와의 강한 결합이 문제인데 스프링프레임워크를 버릴 사태가 벌어지지 않는다는 확신이 있다면 필드 주입을 사용 할 수도 있겠다. 그러나 테스트를 할 때 생성자를 통해 주입하는 것이 해당 클래스 또는 메서드만 한정해서 테스트할 수 있기 때문에 생성자 주입을 사용하는것을 지향 해야 겠다. 그렇지 않으면 모든 빈을 다 로드해야 할 수도 있다.

## 참고
[https://stackoverflow.com/questions/39890849/what-exactly-is-field-injection-and-how-to-avoid-it](https://stackoverflow.com/questions/39890849/what-exactly-is-field-injection-and-how-to-avoid-it)
[http://crossbreeze.github.io/blog/2014/04/08/field-injection-vs-constructor-injection-with-java-springs-at-autowired/](http://crossbreeze.github.io/blog/2014/04/08/field-injection-vs-constructor-injection-with-java-springs-at-autowired/)
[http://vojtechruzicka.com/field-dependency-injection-considered-harmful/](http://vojtechruzicka.com/field-dependency-injection-considered-harmful/)
[https://spring.io/blog/2007/07/11/setter-injection-versus-constructor-injection-and-the-use-of-required/](https://spring.io/blog/2007/07/11/setter-injection-versus-constructor-injection-and-the-use-of-required/)
[https://ohjongsung.io/2017/06/02/%ED%95%84%EB%93%9C-%EC%A3%BC%EC%9E%85-field-injection-%EC%9D%84-%ED%94%BC%ED%95%98%EC%9E%90](https://ohjongsung.io/2017/06/02/%ED%95%84%EB%93%9C-%EC%A3%BC%EC%9E%85-field-injection-%EC%9D%84-%ED%94%BC%ED%95%98%EC%9E%90)
