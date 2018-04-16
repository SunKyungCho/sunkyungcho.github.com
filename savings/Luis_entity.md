# LUIS Entity 정리

엔티티란 사용자에 의해 입력된 문장에서 중요한 단어를 뜻한다. 엔티티를 구성해 놓고 여러 intent(의도)에서 사용할 수 있다.

## Entity Types

### Prebuilt
흔히 사용되는 대표적인 컨셉들(숫자, E-mail, 날짜, 등)을 미리 생성해논 타입이다. 안타깝게도 한글은 준비되어 있지 않다.

### List()
연관된 단어들을 대표한다. 각 리스트에는 하나 이상으로 구성되어 있다. 이건 머신러닝되지 않는다. 그리고
같은 뜻으로 사용되지만 다른 표현을 잡아내는데 많이 사용된다.

리스트는 다른 엔티티 타입들과 다르게 ~~ 이다.
LUIS 훈련을 하면서 따로 추가를 하지 않는다. 측 머신러닝을 통해 학습이 되지 않는다. 필요하자면 직접 추가해줘야한다.



만약

동일한 개념을 표현하는
are best used for a known set of variations on ways to represent the same concept
가장 많이 사용되어 진다/ 아는 셋 변화하는 것들중

알려진 변형된

### Simple
일반적인 엔티티이다. 하나의 컨셉을 가진
하나의 의미를 설명하는 컨셉을 가진 일반적인 엔티티이다. 훈련되고 학습된다.

### Hierarchical(계급, 계층)
계층 엔티티는 특별한 타입이다.
카테고리를 정의한다. 이 멤버들은 부모-자식관계를 갖는다. 예를 들어 장소에 대한 계층 엔티티가 주어진다면 자식은 `출발지` 그리고 `도착지`를 갖는다.
각 자식들은

만약 좀 더 정밀한 매칭을 원한다면 리스트 엔티티를 사용해야 된다.

### Composite(복합)

합성 타입은 다른 엔티티들을 결합하여 만들다.
예를 들어 composiste 엔티티의 이름이 PlanTicketOrder 이라고 한다면 자식 엔티티는 prebuilt `number`와 `ToLocation`를 갖는다.

기존의 simple 엔티티, 계층 엔티티, 미리 작성된 엔티티에서 복합 엔티티를 생성한다.


### 머신러닝 학습
엔티티들은

---

<add key="LuisAppId" value="363005b7-7eaa-48b2-acbe-965979ca2362" />
<add key="LuisAPIKey" value="72d8865b7bf9468187adf70b6372cf52" />
<add key="LuisAPIHostName" value="westus.api.cognitive.microsoft.com" />

echo "# DayOffManagementChatBot" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/SunKyungCho/DayOffManagementChatBot.git
git push -u origin master


#### context 정보 관리

bot state 데이터
data 최대 32KB 저장가능

```CSharp
//입력
context.ConversationData.SetValue("meetingRoom", meetingRoom);
context.ConversationData.SetValue("time", time);

//가져오기
context.ConversationData.TryGetValue("RoomReserved", out reservations)
```

#### 입력된 Entity 조회
이건 List Entity의경우 resolution을 가져오는것
```CSharp
bool res = result.TryFindEntity("MeetingRoom", out meetingRoomEntity)
```
