# 개요
캠프 PC 버전의 인수인계 자료

# 목차
> * 시스템 구성   
> * 주요 기능   
> * 미비 사항    
> * 최근 개발 항목    
> * 참고 사항   
# 개발 환경
본 프로그램을 개발하기 위한 환경에 대한 정보입니다.
|항목|내용|
|:---:|:---:|  
|개발 언어|C# / WPF / .NetFramework 4.6.1|
|DB|System.Data.Sqlite|
|IDE|VisualStudio 2019 Pro|
|Repository|<http://192.168.201.158/git/AppCampMessenger_PC.git>|
|Design|zeplin platform  <br>ID:yjchoi@enliple.com  <br>PW: camp12345|  
# 주요 기능   
## 채팅   
  - 주요 항목    
    |항목|내용|
    |:---:|:---:|  
    |Protocol|XMPP|
    |Library|Sharp.Xmpp|
  - 설명   
    ```
    Sharp.Xmpp 라이브러리의 함수 및 이벤트를 기본으로 커스텀된 메시지를 수/발신한다.   
    채팅방 초대, 메시지 수/발신, 채팅방 정보, 채팅방 참여자 정보 등 채팅의 전반적인 부분을 담당한다.
    ```
  - 참조 코드   
    ```
    XMPPManager
    ```
## API  
  - Address
    |항목|Url|
    |:---:|:---:|  
    |Lubig|<https://api.lubig.co.kr>|
    |Store|<https://storeapi.lubig.co.kr>|
    |Admin|<https://admin.lubig.co.kr>|    
  - 설명    
    ```
    HttpWebRequest를 이용하여 Json형식으로 데이터를 수/발신한다.
    각 서버에서 지원하는 API를 이용하여 해당 기능을 수행한다.(ex. 친구 목록, 기존 대화 목록 등)   
    ```
  - 참조 코드   
    ```
    LubigAPIManager
    ```
    
## AWS
  - 주요 항목
    |항목|내용|
    |:---:|:---:|  
    |S3|업로드 할수 있는 스토리지|
    |EC2|Url주소를 이용한 다운로드|
  - 설명
    ```
    AWS의 S3/EC2를 이용하여 이미지, 파일등을 업로드 및 다운로드한다.         
    ```
  - 참조 코드   
    ```
    AWSManager
    ```
## DB
  - 주요 항목
    |항목|내용|
    |:---:|:---:|  
    |PW|pccamp2019|
    |저장위치|C:\Users\User\AppData\Roaming\Enliple\Camp\AppCampRes\Database|
  - 설명   
    ```
    유저별 대화방, 참여자 정보, 상단고정, 알림설정등의 정보를 저장한다. 
    ```
  - 참조 코드   
    ```
    SqlManager
    ```
## 자동 업데이트      
  - 설명   
    ```
    프로그램 버전정보를 비교하여 낮을 경우 Login시 프로그램을 업데이트한다.
    ```
  - 참조 코드   
    ```
    UpdateManager
    ```
# 미비 사항
TBD ...

# 최근 개발 항목
2021년 1월 13일기준으로 가장 최근에 개발한 항목입니다.
|항목|담당자|배포일|비고|
|:---:|:---:|:---:|:---:|  
|캠프 1:1대화|유병석|2020.01.11|프로그램 버전:0.8.99|
|프로그램 안정화|안동현|2020.01.11|프로그램 버전:0.8.99|
|대화방 공지사항 게시판|이재웅|-|Master branch에 Commit|

# 참고사항
## 서명 방법
  1. AssemblyInfo.cs 에 AssemblyVersion, AssemblyFileVersion 갱신   
  1. \192.168.201.60, PW: 1(공유 PC 접근)   
  1. C:\work\signtool\AppExeSignCode.bat을 이용하여 서명   
  1. 인스톨쉴드를 이용하여 설치 파일 생성   
  1. AppSetupSignCode.bat을 이용하여 설치파일 서명   
  
## 배포 방법  
  1. <https://admin.lubig.co.kr/login, https://adminstage.lubig.co.kr/login> 접속
  1. 계정 정보 입력(CampAdmin / campadmin2019!)
  1. 설정 > PC 버전관리 > 신규 버전 등록 선택
  1. 버전 관리 항목 입력   
  |항목|내용|
  |:---:|:---:|  
  |버전|프로그램 버전|
  |강업 여부|강제 업데이트 여부|
  |설명|업데이트 내역|
  |x86 exe|설치파일|
  |x86 zip|자동 업데이트 파일|
  |x64|설치 파일|
  |x64|자동 업데이트 파일|
  |x86 upload|자동 업데이트 파일|
  |x64 upload|자동 업데이트 파일|    

  
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

