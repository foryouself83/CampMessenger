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
    ```csharp   
    XMPPManager
    
	if (!xmpp.XmppConnect())
	{
		for (int i = 0; i < 2; i++)
		{
			if (!xmpp.XmppRetryConnect())
			{
				Messenger.Default.Send(new NotificationMessage("Logout"));
				System.Windows.Application.Current.Dispatcher.Invoke(() =>
				{
					log.WriteLog(LogType.Error, System.Reflection.MethodBase.GetCurrentMethod(), "XmppConnect failed.");
					sys.OpenMessageBox("채팅 서버 연결에 실패하였습니다.", "", MessageBoxButton.OK);
				});
				return;
			}
		}
	}
    ```
## 메시지
  - 이모티콘
	``` xml
< message type = "groupchat"
to = "91b3730a-cfc6-4aec-b292-7090e2a89652@lubigmuc.xmpp.lubig.co.kr"
msgtype = "file"
id = "F9158CFA-E77D-4892-9A3D-9372DBB018FE"
nick = "다리우스skskdkdjdjd"
filetype = "emoticon"
jid = "c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
roomid = "518AA056-7CBB-4633-8BAF-4707D6926466"
date = "2020-06-02T08:10:44.801Z"> <body>emoticon<body/><emoticon>23404</emoticon><emoticonurl>https://duf7tkmfho2jb.cloudfront.net /emoji/20191007175815/detail/04.png < /emoticonurl><emoticontext>텍스트 내용을 이 곳에 추가</emoticontext></message>
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
    ```csharp   
    var login = LoginManager.Instanse();
    var api = LubigAPIManager.Instanse();
    
    login.Login = api.SendLoginAPI(LoginID, strPassword, out errMsg);
    if (login.Login == false)
    {
		AlarmText = errMsg;
    }
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
    ```csharp   
    AWSManager aws = AWSManager.Instanse();
    
    AwsUploadDataInfo info = new AwsUploadDataInfo();
    info.ImageStream = outMs;
    info.BucketName = buckeFlodertName;
    info.FileName = fileName;
    
    aws.AddUploadFile(info);
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
    ```csharp   
    var sql = SqlManager.Instanse();
    
    CampMemberListInfo memberInfo = null;
    if (!sql.SelectCampMemberInfoData(out memberInfo, userKey, campRegNo.ToString()))
    {
		return false;
    }
    ```
    
## 자동 업데이트      
  - 설명   
    ```
    프로그램 버전정보를 비교하여 낮을 경우 Login시 프로그램을 업데이트한다.
    ```
  - 참조 코드   
    ```csharp        
    if (!UpdateManager.Instanse().AutoUpdate(out errorMsg))
    {
		login.Login = false;
		AlarmText = errMsg = "프로그램 업데이트가 필요합니다.";
		return;
    }
    ```
    
# 미비 사항
### RedMine
  - 신규, 진행, 재열람 건 22건에 대한 수정이 안되어 있습니다.
  - 재현 불가등 사유로 보류 43건에 대한 수정이 안되어 있습니다.
  
### 속도 개선
  - 대화방 목록 Template화
  - 중복된 Image 랜더링 제거(Image manager class 필요)
  - API 호출시 비동기 처리 추가
    
# 최근 개발 항목
2021년 1월 13일기준으로 가장 최근에 개발한 항목으로
세부 사항의 경우 Git Commit 이력을 참고하시기 바랍니다.
|항목|담당자|배포일|비고|
|:---:|:---:|:---:|:---:|  
|캠프 1:1대화|유병석|2020.01.11|프로그램 버전:0.8.99|
|프로그램 안정화|안동현|2020.01.11|프로그램 버전:0.8.99|
|대화방 공지사항 게시판|이재웅|-|Master branch에 Commit|

# 참고사항
## Git
### 브런치 정의
  - Master
    개발용 원격 브런치로 로컬 브런치에서 작업한 기능을 테스트하는 목적으로 사용한다.
  - Pre-Production
    Master 브런치에서 테스트가 완료된 항목을 Commit하며, 배포 전 테스트하는 목적으로 사용한다.
  - Production
    Pre-Production 브런치에서 테스트가 완료된 항목을 Commit하며, 최종 또는 긴급 등 배포되는 최종 소스를 관리하는 목적으로 사용한다.
    
### 기능 개발
  1. Pre-Production에서 로컬 브런치 생성
  1. 기능 개발 후 Master 브런치에 Commit 후 테스트
  1. 테스트 완료 후 Pre-Production 브런치에 Commit
  
### 배포 버전 버그 픽스
  1. Production에서 로컬 브런치 생성
  2. 버그 픽스 후 Master 브런치에 Commit 후 테스트
  3. 테스트 완료 후 Pre-Production, Production 브런치에 Commit
  4. Production 브런치에 버전정보 Tag 추가 후 배포
  
### 배포
  1. Prouction 브런치에 버전정보 Tag 추가 후 배포
  
## 배포
### 서명 방법
  1. AssemblyInfo.cs 에 AssemblyVersion, AssemblyFileVersion 갱신   
  1. \192.168.201.60, PW: 1(공유 PC 접근)   
  1. C:\work\signtool\AppExeSignCode.bat을 이용하여 서명   
  1. 인스톨쉴드를 이용하여 설치 파일 생성   
  1. AppSetupSignCode.bat을 이용하여 설치파일 서명   
  
### 배포 방법  
  1. <https://admin.lubig.co.kr/login>, <https://adminstage.lubig.co.kr/login> 접속
  1. 계정 정보 입력(CampAdmin / campadmin2019!)
  1. 설정 > PC 버전관리 > 신규 버전 등록 선택
  - 버전 관리 항목  
    |항목|내용|
    |:---:|:---:|  
    |버전|프로그램 버전|
    |강업 여부|강제 업데이트 여부|
    |설명|업데이트 내역|
    |x86 exe|설치파일|
    |x86 zip|자동 업데이트 파일|
    |x64|설치 파일|
    |x64|자동 업데이트 파일|
    |x86 upload|업데이트 프로그램|
    |x64 upload|업데이트 프로그램|    

### 시퀀스 다이어그램
  * 로그인  
    <img style="width: 300px; height: 500px" src="https://github.com/foryouself83/testrepo/blob/main/src/Images/Login_seq.png?raw=true"/>
  
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

