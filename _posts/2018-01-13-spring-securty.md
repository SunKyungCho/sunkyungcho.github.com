---
layout: post
title: "스프링부트-시큐리티(Springboot - security) 적용하기"
description: "스프링부트와 스프링 시큐리티를 활용해 로그인 기능과 리멤버미(remember-me)기능을 구현한다."
date: 2018-01-13
tags: [spirngboot, security]
comments: true
share: true
---

Springboot security를 활용해 쉽고 간단하게 로그인(Login) 기능과 리멤버미(remember-me) 기능을 구현한다.

---

# Spring boot Security 적용하기

스프링 시큐리티를 사용하면 간단한 몇가지 설정만으로 로그인 기능을 구현 할 수 있다. 이곳에서는 스프링 시큐리티의 기본구성을 어떻게 하면 되는지 간단한 구현과 사용방법에 대해 기록 하겠다.

## 1. 로그인 기능

### 기본설정
Account라는 계정정보를 관리하는 테이블이 있고 JPA를 통해 관리되는 프로세스를 가지고 있다고 가정하고 일부 소스는 생략했다. 설정에 관한 것 위주로 설명한다.

#### Gradle dependencies 추가
```ruby
dependencies{
  compile('org.springframework.boot:spring-boot-starter-data-jpa')
	compile('org.springframework.boot:spring-boot-starter-security')
}
```
아래의 구현을 위해서 jpa와 security를 선언한다.

#### Account.java
```java
@Getter //lombok
@Setter //lombok
@Entity
public class Account {

    @Id
    @Column(name = "account_id")
    private String accountId;
    @Column(name = "password")
    private String password;
    @Column(name = "role")
    private String role;
}
```
간단한 계정정보를 관리한다. 계정ID, 비밀번호, 권한정보를 가지고 있다.

```java
@Repository
public interface AccountRepository extends JpaRepository<Account, String>{
    Account findByAccountId(String accountID);
}
```
DB에서 계정정보를 가지고 올 Repository 작성.

#### UserDetailsImpl.java
```java
public class UserDetailsImpl extends User {

    public UserDetailsImpl(Account account) {
        super(account.getAccountId(), account.getPassword(), getAuthorities(account));
    }

    private static Collection<GrantedAuthority> getAuthorities(Account account) {
        List<GrantedAuthority> authorities = new ArrayList<>();
        authorities.add(new SimpleGrantedAuthority(account.getRole()));
       return authorities;
    }
}
```
#### UserDetailServiceImpl.java
UserDetailService는 아래와 같이 loadUserByUsername에 대한 메소드만 구현해주면 된다.
```java
@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    AccountRepository accountRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {

        Account account = accountRepository.findByAccountId(username);
        if (account == null){
            throw new UsernameNotFoundException(username);
        }
        return new UserDetailsImpl(account);
    }
}
```
위 두 클래스의 목적은 간단하다. 사용자가 입력한 username으로 DB에서 계정정보를 조회하고 만약 값이 없다면 UsernameNotFoundException을 날리게 될것이고 존재한다면 사용자 계정정보(ID, password, 권한정보)갖게 된다. 이정보를 가지고 내부적으로 login기능을 구현하는데 사용할 것이다.


### SecurityConfig 작성
java configuration으로 설정을 모두 처리한다.
```java
@Configuration
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    UserDetailsService userDetailsService;

    //------(1)
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http.csrf().disable()
            .authorizeRequests()
                .antMatchers("/", "/home", "/join","/join-process").permitAll()
                .antMatchers("/user/**").hasAnyRole("USER")
                .anyRequest().authenticated()
            .and()
            .formLogin()
					         .loginPage("/login")
		               .permitAll()
            .and()
            .logout()
					         .permitAll();
    }

    //-----(2)
    @Autowired
    public void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```
(1) antMatchers를 통해 URL별 권한을 설정해 줄 수 있다. `permitAll()`은 권한 없이 누구나 다 접근 가능한 결로가 될 것이다. /user/** 경로는 USER권한을 갖은 사용자만 접근이 가능할 것이다. 이 페이지에 접근하지 위해서는 login을 해야한다. 권한이 필요하기 때문에 자동적으로 formLogin에 설정되어 있는 /login페이지로 이동하게 된다.
추가로 `.hasAnyRole("USER")`은 내부적으로 앞에 **ROLE_** 가 붙게 된다. 즉, `.hasAuthority("ROLE_USER")`와 동일하게 사용할 수 있다.

(2) 앞서 생성해 놓은 `userDetailsService` 와 `passwordEncoder` 를 등록해 패스워스를 암호화 시킨다.
이렇게 설정을 해 놓으면 security의 간단한 로그인 기능 설정은 끝이다. 추가로 세세하게 설정할 수도 있다. login시 계정정보를 Spring security가 알아서 비교하여 로그인 실패와 성공을 알려준다.

추가로 login form의 파라미터 정보를 지정해줄수 있는데 디폴트설정으로는 'username'과 'password'로 파라미터를 받는다.

```
    .formLogin()
         .loginPage("/login")
         .loginProcessingUrl("/authentication")
         .usernameParameter("userId")
         .passwordParameter("password")
```

/authentication 으로 userId와 password를 던져서 로그인 처리를 한다.

## 2. Remember-me

remember-me 기능은 로그인정보를 기억하여 사용자가 재접속시 별도의 별그인과정을 거치지 않고 로그인 인증을 해주는 기능. 쿠키 방식과 영속성(persistence)방식이 있다. 여기에서는 좀 더 활용성 있는 구현을 위해 영속성 방식으로 구현 했다.

위에 구현했던 로그인 기능에 remember-me를 추가한다.
```java

@Configuration
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {


    @Autowired
    UserDetailsService userDetailsService;

    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http.csrf().disable()
                .authorizeRequests()
					.antMatchers("/", "/home", "/join","/join-process").permitAll()
					.antMatchers("/user/**").hasAnyRole("USER")
					.anyRequest().authenticated()
                .and()
                .formLogin()
					.loginPage("/login")
					.permitAll()
					.and()
                .logout()
					.permitAll()
					.and()
          .rememberMe()
              .key("security-example")
              .rememberMeServices(persistentTokenBasedRememberMeServices())
              .tokenValiditySeconds(8640000);
    }

    // create two users, admin and user
    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
    }

    @Bean
    public RememberMeServices persistentTokenBasedRememberMeServices() {

        PersistentTokenBasedRememberMeServices persistentTokenBasedRememberMeServices =
                new PersistentTokenBasedRememberMeServices("security-example", userDetailsService, persistentTokenRepository());
        persistentTokenBasedRememberMeServices.setParameter("remember_me");
        return persistentTokenBasedRememberMeServices;
    }
    @Bean
    public PersistentTokenRepositoryImpl persistentTokenRepository() {
        PersistentTokenRepositoryImpl persistentTokenRepository = new PersistentTokenRepositoryImpl();
        return persistentTokenRepository;
    }


    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

* security-example 이라고 된 remember-key값은 원하는 값으로 변경해도 두 곳 모두 되지만 동일하게 유지해야한다.
* `setParameter()`의 값은 view 페이지에서 로그인 상태여부를 체크하는 파라미터의  `name` 이다.

다음은 `PersistentTokenRepositoryImpl`를 구현해보겠다.

```java
@Repository
@Transactional
public class PersistentTokenRepositoryImpl implements PersistentTokenRepository {

    @Autowired
    TokenRepository tokenRepository;

    @Override
    public void createNewToken(PersistentRememberMeToken token) {
        Token newToken = new Token();
        newToken.setUsername(token.getUsername());
        newToken.setSeries(token.getSeries());
        newToken.setToken(token.getTokenValue());
        newToken.setLastUsed(token.getDate());
        tokenRepository.save(newToken);
    }

    @Override
    public void updateToken(String series, String tokenValue, Date lastUsed) {
        Token token = tokenRepository.findOne(series);
        token.setToken(tokenValue);
        token.setLastUsed(lastUsed);
        tokenRepository.save(token);
    }

    @Override
    public PersistentRememberMeToken getTokenForSeries(String seriesId) {
        System.out.println("seriesId : " + seriesId);
        Token token = tokenRepository.findOne(seriesId);

        PersistentRememberMeToken persistentRememberMeToken =
                new PersistentRememberMeToken(token.getUsername(), seriesId, token.getToken(), token.getLastUsed());
        return persistentRememberMeToken;
    }

    @Override
    public void removeUserTokens(String username) {
        tokenRepository.deleteByUsername(username);
    }
}
```

`PersistentTokenRepository` 를 직접 구현해줬다.

`TokenRepository`는 아래와 같다.
```java
@Repository
public interface TokenRepository extends JpaRepository<Token, String>{

    int deleteByUsername(String username);
}
```

 `Token`의 객체를 정의한다.
```java
@Getter
@Setter
@Entity
@Table(name="remember_me_token", catalog = "toy_db")
public class Token {

    @Column(name = "username")
    private String username;
    @Id
    @Column(name = "series")
    private String series;
    @Column(name = "token")
    private String token;
    @Column(name = "last_used")
    private Date lastUsed;

}
```

이렇게 해주면 구현된다.
이 구조는 조금은 커스텀된 형태로 구현을 한것인데 더 간단히 사용하기 위해서는 `JdbcTokenRepositoryImpl`를 사용해도 된다.


```java
@Autowired
DataSource dataSource; //추가한다.

@Bean
public PersistentTokenRepository jdbcTokenRepository() {
	JdbcTokenRepositoryImpl repository = new JdbcTokenRepositoryImpl();
	repository.setCreateTableOnStartup(false);
	repository.setDataSource(dataSource);			
	return repository;
}
```

`PersistentTokenRepositoryImpl` 대신 `JdbcTokenRepositoryImpl` 를 구현해주고 dataSource를 설정해주면 별도의

```sql
CREATE TABLE `persistent_logins` (
  `username` varchar(64) DEFAULT NULL,
  `series` varchar(64) NOT NULL,
  `token` varchar(64) DEFAULT NULL,
  `last_used` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`series`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
```

tokenRepository를 구현하지 않아도 `persistent_logins` 테이블에 token정보가 저장된다.

## 마치며
로그인을 구현하는데 있어서 쉽게 구현할 수 있었다. 로그인을 구현한다는것은 여간 까다롭고 귀찮은 일이 아니다. spring-security가 알아서 편리하게 해주니 좋다. 한번 구현해놓으면 잘 안보게 되어서 다시 하려고 금세 까먹는다. 고로 저장.
