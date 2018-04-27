# LUIS Action Binding
챗봇에 국한된 것은 아니지만 챗봇을 기반으로 이야기한다. 아직 공부중이므로 정확하게 맞는 내용은 아닐 수 있다. 혹시 적혀 있는 내용이 사실과 다르다면 알려주길 바란다. 혹은 오해


## LUIS Action Binding이란
LUIS는 사용자의 입력에서 의도(intent)와 객체(entity)를 알려준다. 의도를 알았으면 그에 따른 액션이 필요하다. 예를 들어 무엇인가를 예약한다던지 변경을 한다던지 하는 사용자의 의도에 따른 액션이 필요하게 된다. 사용자가 비행기를 예약한다고 해보자. '비행기 예약' 이라는 입력이 들어왔다. 하지만 예약을 하기엔 정보가 부족하다. 언제, 어디, 시간 등의 정보가 필요하다. Action Binding 에서는 이러한 '비행기 예약'이라는 액션을 하기 위해 추가로 필요한 정보가 없는지 더 나아가 정보가 유효한 정보인지에 대한 판단을 할 수 있도록 도구를 제공한다.
그런 이러한 과정이 어떻게 처리되고 구현 할 수 있는지 보자.


## 시나리오 대응
### 첫번째 시나리오. 대화중 다른 화제로 넘어갔을때

```
Bot: 뭘 하길 원하시나요?
User: 서울 근처에 호텔 좀 찾아줘
Bot : 언제 체크인 할건데?
User : 오늘
Bot : 언제 체크 아웃 한건데?
User : 마음이 바꼈어, 서울가는 비행기로 찾아줘
Bot : 언제 갈건데?
....
```

### 두번째 시나리오.

```
Bot: 뭘 하길 원하시나요?
User: 서울 근처에 호텔 좀 찾아줘
Bot : 언제 체크인 할건데?
User : 오늘
Bot : 언제 체크 아웃 한건데?
User : 미안해, 체크인 내일로 변경해줘
Bot : 알겠어, 내일로 체크인 변경됐어
Bot : 언제 체크 아웃 할건데?
...
```

### 3번째 시나리오
```
User : 내 체크인 날짜를 내일로 변경해줘
Bot : 알겠어, 내일로 체크인 변경 됐어.
Bot: I changed your reservation in Madrid from 03/25 to 03/27
```


`ILuisAction` 인터페이스를 구현한 형태로 간다.
```CSharp
public interface ILuisAction
{
    Task<object> FulfillAsync();

    bool IsValid(out ICollection<ValidationResult> results);
}
```

#
```csharp
[Serializable]
public abstract class BaseLuisAction : ILuisAction
{
    public abstract Task<object> FulfillAsync();

    public virtual bool IsValid(out ICollection<ValidationResult> validationResults)
    {
        var context = new ValidationContext(this, null, null);

        validationResults = new List<ValidationResult>();
        var result = Validator.TryValidateObject(this, context, validationResults, true);

        // do order properties
        validationResults = validationResults
            .OrderBy(r => LuisActionResolver.GetOrderForParam(this, r.MemberNames.First()))
            .ThenBy(r => r.MemberNames.First())
            .ToList();

        return result;
    }
}
```





## 마무리
LUIS Action Binding 예제는



예제는 Web, Bot, Console 예제가 있음
Node.js, C# 이있고
Web과 Console은 여기서 다루지는 않는다.






## 참고
[https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/Blog-LUISActionBinding](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/Blog-LUISActionBinding)
