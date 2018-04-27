# Microsoft Bot Framework & LUIS를 활용한 챗봇 따라하기

## Bot Framework 생성.
MS의 Azure 서비스를 사용해 Bot App을 생성한다.

#### 1. 리소스 그룹 생성
꼭 리소스 그룹을 생성해야 하는건 아니지만 프로젝트 단위로 리소스 그룹을 생성하는 것이 관리하기에 더 유용하다.

![image](/Images/botFramework&Luis/botframework/make_resource_group.png)

적당한 이름의 리소스 그룹의 이름을 정하고 상황에 맞는 구독을 정하면 되겠다. 리소스 그룹 위치의 경우는 어느곳이든 상관 없지만 LUIS와 연동을 고려해 같은 지역에 리소스 그룹 위치를 정하면 조금이라도 더 빠를 수 있다.

#### 2. Bot Framework 생성.
리소스 그룹 내부에 봇 서비스를 생성해보자. 추가를 누른뒤 **Web App bot** 만들기.
![image](/Images/botFramework&Luis/botframework/make_web_bot.png)

#### 3. Web App Bot 서비스 생성하기.
웹 앱 봇 만들기. 챗봇을 만들기 위한 App 서비스와 더불어 기본적인 템플릿을 제공한다.
![image](/Images/botFramework&Luis/botframework/make_setting.png)

여러 봇 템플릿이 있지만 LUIS를 사용할 것이기 때문에 항목에서 **Language understanding** 을 선택한 후 만들기.
![image](/Images/botFramework&Luis/botframework/bot_template.png)



#### 4. LUIS 정보 등록하기.
이전에 생성했던 리소스 그룹내에 Web App Bot 서비스를 위한 몇개의 서비스가 생성된 것을 볼 수 있다. 더불어 LUIS 템플릿을 통해 LUIS도 생성이 된것을 볼 수 있다.
![image](/Images/botFramework&Luis/botframework/luis_setting.png)

기존에 만들어 놓은 LUIS 사용할 경우 **App Service** 에서 응용 프로그램 설정에 `LuisAPIKey`와 `LuisAppId` 값을 변경해 주면 되겠다.

이렇게 하면 테스트를 해보면 아주 간단하게 웹 챗봇이 생성 되었다.
