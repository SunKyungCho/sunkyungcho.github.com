

이미지는 Hero card를 사용하면 될 수 있을듯 싶다. (확인 필요.)

## PromptDialog
The PropmptDialog choice method has different parameters. You can refer below for parameters and their usage.

* Context - user context message
* Resume - its Resume handler, what next process
* Options - list of prompt item
* Retry - What to show on retry.
* Attempts -The number of times to retry. default: 3
* PromptStyle - Style of the prompt Prompt Style
* Descriptions - Descriptions to display for choices.

```csharp
public virtual async Task ShowAnnuvalConferenceTicket(IDialogContext context, IAwaitable<IMessageActivity> activity)  
{  
    var message = await activity;  

    PromptDialog.Choice(  
        context: context,  
        resume: ChoiceReceivedAsync,  
        options: (IEnumerable<AnnuvalConferencePass>)Enum.GetValues(typeof(AnnuvalConferencePass)),  
        prompt: "Hi. Please Select Annuval Conference 2018 Pass :",  
        retry: "Selected plan not avilabel . Please try again.",  
        promptStyle: PromptStyle.Auto  
    );  
}  
```


## 참고 사이트 정리
[PromptDialog 따라하기](https://www.c-sharpcorner.com/article/getting-started-with-prompt-dialog-using-microsoft-bot-framework/)
