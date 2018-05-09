# Asiana ChatBot project 해석


## 시작
`Global.asax`파일에 `WebApiApplication`클래스의 `Application_Start()`을 제일 먼저 로딩된다.
DB의 연결 및 메모리캐쉬준비, 검색엔진 연결 확인등을 진행한다.

그리고 `InitDialog`를 호출한다.





## 채널 구분
현재는 facebook, kakao 두 개의 채널을 가지고 있다.

```csharp
 if (!activity.ChannelId.ToLower().Equals("directline") && !activity.ChannelId.ToLower().Equals("kakaotalk"));
```




## Activity.Text



## 언어 설정
기본적으로
1. 한국어
2. English


이런식으로 선택을 받는다. 그냥 `메뉴`, `수화물조회`를 입력하면 한글로 인식하고 답을 해준다.
내부적으로는
```
    botUserDataModel.UserDataCurrentLanguage = LanguageType.Kor;
``그리고 `InitDialog
를 호출한다.
언어값이 None라면 이렇게 Kor로 입력을 해주게된다.

여기서 질문
1. 영어로 입력을 했을때는?


## ActivityTypes 구분.
* message
* ConversationUpdate



## 메세지 노출
아래와 같은 방법으로 대화를 노출한다ㅓ.
```java
var replyMsg = connector.Conversations.SendToConversationAsync(replyMessage);
```


## 전처리
인사말이 나타나고 사용자는 대화를 입력한다. 한국어, 영어를 선택할 수도 있고 다른말을 입력할 수도 있다.
모든 사용자 입력에 대한 전처리 프로세스이다.

전처리를 진행하는 방식은 기본적으로 `GlobalHandlerService` 에서 `InterceptScorable`를 등록해 놓고 `InterceptScorable`의 `PrepareAsync`를 호출한다. 여기는 전체 입력에 대해 전처리를 진행하는것 같다.

#### 1. HomeKey
HomeKey 왜 이름이 홈키인지는 모름.  `Web.config`에 아래와 같이 등록 되어 있는데 왜 Configuration에 등록해봤는지 알 수 없다.
```
<add key="HomeKey" value="메뉴,도움,처음,처음으로,홈,도움말,이전,사용방법,사용설명서,사용 설명서,menu,help,begin,clear,home,list,get start,start,first page,go back,main,main page,top,above,don't know,I don't know,dont know,I dont know" />
```

추측하기로는 위와 같은 키가 매칭이 되면 `메뉴`를 보여주기 위해서 인것 같다.

#### 2. 욕
문의는 `욕`사전과 `변태`사전을 나누고 싶어한다. 여기서 수정을 하면 될 듯 싶다.
미리 로딩할때 DB에서 값을 가지고 메모리 캐쉬로 가지고 있다.

띄어쓰기로 구분해서 일치해야지만 걸러낼수 있는 듯 보인다.

#### 3. 의미 없는 단어의 반복
특수문자가 반복되어 있는 경우

`메뉴!!!!!!!` 이런 경우는 어떨까?
결론은 `메뉴`로 리턴

1. 특수문자 replaec()
2. `ㅎㅎㅎㅎㅎ`, `ㅋㅋㅋ` 같이 입력된걸 잡는다. `메뉴ㅎㅎㅎ`는 안잡는다.

#### 4. 관리자 명령어.
미리 약속된 답변 `/답변`으로 입력되면 관리자에 의해 입력되었다고 판단한다.


위의 4가지는 사용자가 입력을 할때마다 처리를 하게 된다.




## Main menu

sltMenu
#### menuList
전체 메뉴 리스트가 여기에 담긴다. `InitMenuDepth`객체의 depth를 가진 객체를 가지고 있다.

```
-		menuList	Count = 12	System.Collections.Generic.IList<AsianaBot.CSBot.Model.InitMenuDepth> {System.Collections.Generic.List<AsianaBot.CSBot.Model.InitMenuDepth>}
+		[0]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
-		[1]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
		appName	null	string
		intentName	null	string
		keyword1	null	string
		keyword2	null	string
		menuKey	"2"	string
		menuName	"운항 노선 및 스케줄 정보"	string
		resultType	Scenario	AsianaBot.CSBot.Model.ResultType
-		subMenuList	Count = 3	System.Collections.Generic.IList<AsianaBot.CSBot.Model.InitMenuDepth> {System.Collections.Generic.List<AsianaBot.CSBot.Model.InitMenuDepth>}
      - [0]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
      - [1]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
      - [2]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
      - Raw 뷰		
		  verb	null	string
+		[2]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[3]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[4]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[5]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[6]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[7]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[8]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[9]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[10]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth
+		[11]	{AsianaBot.CSBot.Model.InitMenuDepth}	AsianaBot.CSBot.Model.InitMenuDepth

```




# Dialog 호출 흐름.



## 전처리
사용자의 입력에서 Entity를 뽑아오는 과정. 이과정을 전처리하고 한다.
예를 들어, 출도착 정보를 조회한다고 하면,

1. 탑승일 체크
2. 출발지
3. 도착지 정보.

위 3가지의 입력이 필요하다.
`내일 출도착조회 정보 알려줘` 라고하면 탑승일의 정보를 충족이되어 출발지부터 물어볼것이고, 그냥 `출도착조회`를 하면 탑승일부터 물어볼것이다.

다른 Dialog도 비슷할듯싶다.
