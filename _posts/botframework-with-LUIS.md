# Microsoft Bot Framework & LUIS를 활용한 챗봇 따라하기

Bot framework는 Microsoft에서 제공하는 Bot서비스이다. Bot framework를 사용하면 Bot을 비교적 간단하게 개발하고 배포할 수 있다. 얼마나 쉽고 간단한지 한번 구현을 해보도록 하겠다.
거기에 더해서 [LUIS](https://www.luis.ai/)(Langues Understanding Intelligent Service)서비스와 결합해보려고 한다. 이 글에서는 연동에 초점을 맞췄다. 그냥 Bot서비스의 Hello world 정도라고 보면 될 것 같다. 프로젝트 생성 및 개발 환경까지의 설정을 기록한다.
들어가기에 앞서 [Azure서비스](https://azure.microsoft.com/ko-kr/)에 가입이 되어있야 한다. 신규가입시 무료로 사용할 수 있도록 무료평가판을 준다. 이를 활용해 어느정도 테스트를 충분히 해볼만 하겠다.

## LUIS 설정하기
루이스는 자연어 처리 서비스로 언어의 의도(intent)와 실체(Entity)파악해 준다. 예를 들어 '서울행 티켓을 예약해줘'라는 문장에서 서울, 예약 이라는 실체와 의도를 알 수 있게 해준다. 이러한 기능으로 bot 시스템 대화에서 사용자의 의도를 알아내는데 사용될 것이다.
더 자세한 정보는 [여기](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/)에서 천천히 글을 읽어보면 더 잘 알 수 있을것이다.

#### MyApp 생성
[LUIS](https://www.luis.ai)에 접속해 로그인 후 MyApp을 만들어보자.
![image](/Images/botFramework&Luis/create_luis_Myapp.png)

Create new app을 클릭해 생성한다.

![image](/Images/botFramework&Luis/create_luis_Myapp-2.png)

MyApp명과 언어, 설명을 작성한다. 사용자의 의도를 해석할 명확한 목표를 가지고 있도록 설계하는것이 좋다.

#### Intent 생성

행위에 대한것

먼저 Intent를 생성해 본다. intent 의도라는건 사용자가 수행하고자 하는 목적, 목표를 말한다. 챗봇에서 사용자가 입력하는 말의 의미을 파악한다는 말이다.
![image](/Images/botFramework&Luis/intents-list.png)

기본적으로는 아무것도 등록된것이 없다. 등록을 해주어야 하는데 영어나 중국어의 경우는 Add prebuilt domain intent를 클릭해보면 미리 준비된 domain들이 많이 있는것을 볼 수 있다. 한글은 미리 준비되어 있는것이 없다.

#### 언어 지원정보
지원되는 언어를 살펴보면 아래와 같다.
![image](/Images/botFramework&Luis/languae_support.png)

생각보다 많은 언어를 지원한다.
언어별로 각각의 미리 준비된 domain이나 entity가 제공된다. 하지만 아쉽게도 한글은 미리준비된 entity들이 없다. 모두 설정해줘야한다. [MS Translator API](https://docs.microsoft.com/en-us/azure/cognitive-services/translator/translator-info-overview)를 사용하라고 한다.

#### Intent 등록
Intent를 등록해준다. 간단한 예제를 위해 인사말을 등록해 줬다. 등록해둔 안녕, 안녕하세요, hi등의 말들을 훈련해 이와 비슷한 말들이나 같은 말이 들어오면 이 말의 뜻은 인사하는거구나를 파악하게 될것이다.
![image](/Images/botFramework&Luis/intent_greeting.png)

#### Entity 생성

왜쓰나?
안써도 잘 돌아간다?

ex)
인터넷이 고장났어
전화 안돼

무조건 필 수 적인건 아니다.



## Bot Framework

변경이되서 봇프레임워크 생성시 기본 템플릿이 제공된다.
비주얼 스튜디오 2017에서 테스트 베이스를 띄워야 하는데 잘안됨
패키지를 설치해줘야 하는것 같음.

* NuGet Package 근데 잘 안됨

ngrok설명도 써두자
로컬에 있는 자원을 외부에서 접속하기 위한~

* [Bot Framework Emulator 다운로드](https://github.com/Microsoft/BotFramework-Emulator/releases)


***로컬에서 실행 안됨***

#### Azure
온라인 코드 편집기를 활용해 소스 수정했음.

빌드할때 주의 사항
>Tip
An alternative method to build the bot is to select the bot name in the top blue bar, and select Open Kudu Console. The console opens to D:\home.
Change the directory to *D:\home\site\wwwroot* by typing: *cd site\wwwroot*
Run the build script by typing: *build.cmd*



------
# 챗봇 세미나 관련 정보

Azure potal접속시
먼저 리소스 그룹을 먼저 생성한다.
ex) bot-test-wus-rg
ey) bot-test-wus-wab
ez)

미국 서부로 하는게 좋다.
같은 데이터 센터내에 있는게 좋다. 데이터 지연이 없다.


정확도를 보장하기 위해서는
한 intent당 최소 200문장 정도는 넣어줘야 한다.
한글은 좀 더 많이 넣어줘야 할것이다.



# Slack 연동하는거 해보자


# 원칙
사람들은 ai가 똑똑한줄아니까 잘 대응해줘야해
그럴려면 디자인을 잘해야되는데
디자인을 잘한다는건 생각한 시나리오만을 질의하진 않는다.
사실 시나리오를 만드는게 가장 좋다.





### 날씨 API
날씨에 대한 정보는 [SK planet DEVELOPERS](https://developers.skplanetx.com/apidoc/kor/weather/living/#doc1400)의 API를 활용하여 날씨 정보를 얻어 온다.

* [날씨 API 관련 예제 블로그](http://rhammer.tistory.com/124)


## 마치며
머신러닝을 기반으로 하다보니 충분한 양질의 데이터로 훈련을 시켜줄수록 더 좋을 결과를 뽑아낼 수 있다. 어떻게 하면 양질에 훈련을 할 수 있는가는 더 알아봐야겠다.


## 참고
[https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/Home](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/Home)<br>
[Bot Service https://docs.microsoft.com/ko-kr/bot-framework/](https://docs.microsoft.com/ko-kr/bot-framework/)
