# Camp Messenger

## 개요
캠프 PC 버전의 인수인계 자료

## 목차
* 시스템 구성  
  |항목|내용|
  |:---:|:---:|  
  |개발 언어|C# / WPF / .NetFramework 4.6.1|
  |DB|System.Data.Sqlite|
  |IDE|VisualStudio 2019 Pro|
  |Repository|<http://192.168.201.158/git/AppCampMessenger_PC.git>|
  |Design|zeplin platform  <br>ID:yjchoi@enliple.com  <br>PW: camp12345|  
* 주요 기능  
  + 채팅      
    - 주요 항목    
      |항목|내용|
      |:---:|:---:|  
      |Protocol|XMPP|
      |Library|Sharp.Xmpp|
    - 설명   
      Sharp.Xmpp 라이브러리의 함수 및 이벤트를 기본으로 커스텀된 메시지를 수/발신하여 Parsing 부분도 포함되어 있다.
      전반적인
  + API  
    - Address
    |Address|Url|
    |:---:|:---:|  
    |Lubig|<https://api.lubig.co.kr>|
    |Store|<https://storeapi.lubig.co.kr>|
    |Admin|<https://admin.lubig.co.kr>|    
    
    
    - 설명
      Http
    
  + AWS  
    |항목|내용|
    |:---:|:---:|  
    |Protocol|XMPP|
    |Library|Sharp.Xmpp|
    |Code|XmppManager.cs|  
  + DB
    |항목|내용|
    |:---:|:---:|  
    |Protocol|XMPP|
    |Library|Sharp.Xmpp|
    |Code|XmppManager.cs|  
  + Auto Update
* 미비 사항
* 최근 개발 항목
* 참고 사항  
  

---
ms.openlocfilehash: b90821d0d3495f6006d1d97b3d3377e984c74c5e
ms.sourcegitcommit: 27db07ffb26f76912feefba7b884313547410db5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83613592"
---
  
  
  
  
  <img style="width: 300px; height: 500px" src="https://github.com/foryouself83/testrepo/blob/main/src/Images/%EB%A1%9C%EA%B7%B8%EC%9D%B8%ED%99%94%EB%A9%B4_%EC%95%88%EB%82%B4%EB%AC%B8%EA%B5%AC.JPG?raw=true"/>
  
## Code
> this code is sample resource
```csharp
public class LoopTest
{
    public void LoopEnumerable(EnumType type, long loopCnt)    
    {    
       console.WriteLine(string.Format("Total Time: {0} sec", new LoopAction(type, loopCnt).Run());
    }
}
```
code: [here](https://github.com)

