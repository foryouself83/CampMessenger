# 개요
캠프 PC 프로그램의 시스템 구성, 주요기능부터 미비사항까지 정리하여   
프로그램 개발에 적응하기 위한 편의성을 높이고, 개발 진행 방향에 대해 도움을 주고자 합니다.

# 목차
> * [개발 환경](<https://github.com/foryouself83/testrepo/blob/main/README.md#%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD>)   
> * [Base Struct](<https://github.com/foryouself83/CampMessger/blob/main/README.md#base-struct>)
> * [주요 기능](<https://github.com/foryouself83/testrepo/blob/main/README.md#%EC%A3%BC%EC%9A%94-%EA%B8%B0%EB%8A%A5>)   
> * [미비 사항](<https://github.com/foryouself83/testrepo/blob/main/README.md#%EB%AF%B8%EB%B9%84-%EC%82%AC%ED%95%AD>)    
> * [최근 개발 항목](<https://github.com/foryouself83/testrepo/blob/main/README.md#%EC%B5%9C%EA%B7%BC-%EA%B0%9C%EB%B0%9C-%ED%95%AD%EB%AA%A9>)    
> * [참고 사항](<https://github.com/foryouself83/testrepo/blob/main/README.md#%EC%B0%B8%EA%B3%A0%EC%82%AC%ED%95%AD>)   
> * [이재웅 인수인계서](<https://github.com/foryouself83/CampMessger/blob/main/README.md#%EC%9D%B4%EC%9E%AC%EC%9B%85-%EC%9D%B8%EC%88%98%EC%9D%B8%EA%B3%84%EC%84%9C>)

# 개발 환경   
|항목|내용|
|:---:|:---:|  
|개발 언어|C# / WPF / .NetFramework 4.6.1|
|DB|System.Data.Sqlite|
|IDE|VisualStudio 2019 Pro|
|Repository|<http://192.168.201.158/git/AppCampMessenger_PC.git>|
|Design|zeplin platform  <br>ID:yjchoi@enliple.com  <br>PW: camp12345|   

# Base Struct
향후 캠프 개발의 유지/추가 개발에 앞서 Based Struct 항목의 Resource, DataContext 항목들의 규칙과 방향성을 반드시 인지하고 주의사항을 확인하는 것을 권장합니다.
## ResourceDictionary
ResourceDictionary는 이 앱에 사용되고 있는 모든 리스소 관리에 대한 정책, 개발 확장 방법에 대한 구조를 설명합니다.   
### 리소스 관리대상 주요 위치   

> *AppCampMessenger/App.xaml*   
> *AppCampMessenger/Based/Template/*

- **분류된 리소스 항목**   
  그 외에 분류되지 않은 리소스 객체들은 `ApplicationStyles.xaml`에 위치하고 있습니다.
  
  - **Template**
    - ApplicationStyles.xaml    
    - ColorStyles.xaml    
    - CommonStyles.xaml
    - ConverterStyles.xaml
    - FriendViewStyles.xaml
    - LoginViewStyles.xaml
    - NoticeStyles.Bar.xaml
    - NoticeStyles.Circle.xaml
    - NoticeStyles.Detail.xaml
    - NoticeStyles.Hierachy.xaml
    - NoticeStyles.List.xaml
    - NoticeStyles.Window.xaml
    - NoticeStyles.xaml
    - PathStyles.xaml
    - StorySearchStyles.xaml
    - TalkListStyles.xaml   
    
  리소스를 화면 또는 컨트롤, 기능별로 상황에 맞게 분류하도록 귀칙을 정하고 있으므로 향후 개발, 확장에 있어서도 리소스를 최대한 파일별로 나누어 관리하도록 권장합니다.
  *(리소스 파일의 순서와 순환참조에 관하여 아래에서 설명)*
- **MergedDictionaries**   
  추후 분류되는 리소스 파일은 아래 `App.xaml` MergedDictionaries 컬렉션에 추가해야 합니다.
  > 먼저 호출되는 리소스를 기준으로 순서를 배치하는 것이 중요합니다. Converter, Color, Path와 같은 리소스는 다른 리소스의 참조형이므로 상위에 위치하도록 하며 참조 흐름을 잘 확인하여 배치하는 것이 중요합니다.   
  
  ```xaml
  <Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/ConverterStyles.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/ColorStyles.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/PathStyles.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/ApplicationStyles.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/LoginViewStyles.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/FriendViewStyles.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/NoticeStyles.Circle.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/NoticeStyles.List.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/NoticeStyles.Hierachy.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/NoticeStyles.Detail.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/NoticeStyles.Window.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/NoticeStyles.Bar.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/NoticeStyles.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/StorySearchStyles.xaml"/>
            <ResourceDictionary Source="/AppCampMessenger;component/Based/Template/TalkListStyles.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>            
  </Application.Resources>
  ```
- **Startup 방식**
  애플리케이션 시작 방식은 App.xaml(Application)에서 직접 메인 화면을 StartupUri를 통해 실행합니다.   

  **ShutdownMode 기본 값**   
  > *ShutdownMode = ShutdownMode.OnLastWindowClose;*   
  ```xaml
  StartupUri="MainWindow.xaml"
  ```
  *(MainWindow.xaml: 로그인과 메인화면을 `MainWindow`에서 함께 Switching 처리되고 있습니다.)*
## Geometry   

**Zeplin** 디자인 팀간 협업   

캠프 프로젝트는 기본적으로 Zeplin 플랫폼에서 `.svg`와 `.png` 모두 전달받고 있으며 누락되는 리소스는 디자인팀에 요청하고 있습니다.   


디자인 리소스는 기본적으로 `Geometry` Path Data를 권장하며 그 외에 `Vector` 기반이 아닌 이미지인 경우 `.png` 사용합니다. 그리고 Geometry 관련 리소스는 이 파일`PathStyles.xaml`에서 관리합니다.   

**Svg 변환**    

[SvgToXaml](https://github.com/BerndK/SvgToXaml) - 대중적으로 가장 많이 쓰이고 있는 오픈소스 변환 프로그램입니다.   

> Geometry는 일러스트레이터를 통해 작업된 최종 결과물을 `.png`가 아닌 `.svg` 형태로 넘겨받아 변환작업 할 수 있도록 합니다.   

### Geometry Resource 위치
```
AppCampMessenger/Based/Template/PathStyles.xaml
```
- #### DrawingImage
  1개 이상의 복합 Geomatry 리소스 `DrawingImage` 형식
  ##### PathStyles.xaml   
  
  ```xaml
  <DrawingImage x:Key="DRAWING_NOTICE">
    <DrawingImage.Drawing>
        <DrawingGroup ClipGeometry="M0,0 V20 H20 V0 H0 Z">
            <DrawingGroup Opacity="1" Transform="1,0,0,1,-471.67,-355.503">
                <GeometryDrawing Brush="#FF818BB9" Geometry="F1 M20,20z M0,0z..."/>
                <DrawingGroup Transform="1,0,0,1,1.526,0.868">
                    <GeometryDrawing Brush="#FF818BB9" Geometry="F1 M20,20z M0..."/>
                </DrawingGroup>
            </DrawingGroup>
        </DrawingGroup>
    </DrawingImage.Drawing>
  </DrawingImage>
  ```
  
  **적용 방법**   
  
  ```xaml
  <Image Source="{StaticResource DRAWING_NOTICE}"/>
  ```
  DrawingImage 객체는 `ImageSource`를 상속받는 객체이므로 복합 Geometry를 화면에 표현할때 Image 컨트롤 사용하도록 합니다.
  
  **적용 사례**   
  
  > 캠프 메인 로고, 색상이 다른 복합 Geometry 기반 이미지 등
- **Path, Style**
  단일 Geometry를 포함하는 `Path` Style 형식
  
  **PathStyles.xaml**   
  
  ```xaml
  <Style TargetType="Path" x:Key="PATH_CLOSE">
      <Setter Property="Data" Value="M12,2C17.53,2 22,6.47 22,12C22,17.53 17.53..."/>
  </Style>
  ```
  스타일을 지정하는 방식이므로 `Fiil`, `Stroke` 등의 프로퍼티를 더 추가할 수 있습니다.
  
  **적용 방법**   
  
  ```xaml
  <Path Style="{StaticResource PATH_CLOSE}"/>
  ```
- **Path, Geometry**
  단일 Geometry 데이터를 직접 사용하는 `Data` 형식   
  
  **PathStyles.xaml**   
  ```xaml
  <Geometry x:Key="G_USER">M12,2C17.53,2 22,6.47 22,12C22,17.53 17.53...</Geometry>      
  ```
  **적용 사례**   
  
  ```xaml
  <Path Data="{StaticResource G_USER}"/>
  ```
  Geometry를 다이렉트로 적용하는 경우 Data 속성에 직접 StaticResource 바인딩을 합니다.   
  
### 팁  
> 상황에 따라 Geometry를 적용하는 3가지 방식을 기반으로 리소스를 타이트하게 관리해 나가는 것은 디자인의 일관성과 관리성 측면에서 매우 중요한 요소입니다.

## Converter
IValueConverter를 관리 운영 및 확장하기 위해 아래 생성 요건과 확장에 대한 유의사항, 그리고 기존 목록과 중복되지 않도록 관련 내용들을 확인이 반드시 필요합니다.
### 네임스페이스   
```csharp
using AppCampMessenger.Based.Converters;
```

### 생성 요건   
*다음 생성 요건에 충족할 경우 Converter를 생성합니다.*   
  1. Property 값의 변경이 Display에서만 적용해야 하는 경우.
  2. Binding Mode가 OneWay 타입일 경우.
  3. 반복되는 데이터 양이 많지 않으며 성능에 영향을 주지 않을 정도(로드)의 처리량.   
  
**컨버터 확장**   
> Xaml Display에 출력되는 Data Modlel의 원본`Raw`을 유지하는 것은 매우 중요합니다. Model 또는 ViewModel을 통해 연계되는 DataContext Binding을 가능하도록 유지하기 위해 Replace성 Converter를 적극 활용하는것을 권장합니다.
  
### Converter 목록   
- **IValueConverter**
  - **AngleToIsLargeConverter** *다운로드 프로그래스 바 Circle 100분율 계산*
  - **AngleToPointConverter** *값의 Circle 위치 좌표 계산*
  - **BooleanToCollapsedConverter** *Boolean 값이 True일 때 Collapsed or Visible 반환*
  - **BooleanToReverseConverter** *Boolean 값이 True일 때 False or True 반환*
  - **BooleanToVisibilityConverter** *Boolean 값이 True일 때 Visible or Collapsed 반환*  
  - **BytesToFormatConverter** *Bytes 값을 KB, MB, GB 등의 용량 단위로 반환*
  - **DateTimeToDisplayTextConverter** *DateTime 타입을 3일전 형식의 포멧으로 반환*
  - **EmptyToCollapsedConverter** *값이 Empty일 때 Collapsed or Visible 반환*
  - **EmptyToVisibilityConverter** *값이 Empty일 때 Visible or Collapsed 반환*
  - **ExistToVisibilityConverter** *값이 존재할 때 (!string.IsEmpty(vlaue)) Visibility or Collapsed 반환
  - **FriendDescriptionWidthConverter** *값의 길이에 따라 Width를 반환* *비 범용성*
  - **TextBlockHyperLinkConverter** *값을 hyperlink로 반환*
  - **ZeroToCollapsedConverter** *값이 0일 때 Collapsed or Visible 반환*
  - **ZeroToVisibilityConverter** *값이 0일 때 Visible or Collapsed 반환*   

#### Converter 클래스 규칙
**클래스 이름**, Converter의 네이밍 규칙은 변환되는 대상의 타입 또는 성격에 맞는 직관적인 단어를 시작으로 To **{변환되는 결과 형태의 타입}** 을 연결하여 클래스명을 마무리 합니다.   

예를 들어 Bool 타입의 Value 값에 따라 Visibility 값을 반환하는 경우   

**Boolean** + To + **Visibility** + Converter
```
BooleanToVisibilityConverter
```

**ConverterParameter** 사용에 관하여   

> ConverterParameter를 사용하여 Reverse 로직을 함께 처리하는 방식 보다 Converter를 두개로 나누어 관리하는 것이 훨씬 더 효율적이고 직관적입니다. 그러므로 이 프로젝트에서 ConverterParameter를 사용하는 것을 지양하는 바입니다.

### Converter 작성 요령
기본적으로 자주 쓰이는 Visibility 처리 등의 단순 컨버터들은 이미 다양하게 제공되고 있습니다.   

**예를들어** BoolToVisibilityConverter   

```csharp
public class DateTimeToDisplayTextConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value.Equals(this) ? Visibility.Visible : Visibility.Collapsed;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

**처리 로직이 필요한 컨버터의 경우** DateTimeToDisplayTextConverter
```csharp
public class DateTimeToDisplayTextConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is string strDt)
        {
            DateTime dt = DateTime.Parse(strDt);
            DateTime now = DateTime.Now;
            var tick = now.Subtract(dt);

            if (tick.TotalSeconds < 60)
                return $"방금";
            if (tick.TotalMinutes < 61)
                return $"{(int)tick.TotalMinutes}분전";
            else if ((int)tick.TotalHours < 25)
                return $"{(int)tick.TotalHours}시간전";
            else if ((int)tick.TotalDays < 2)
                return $"어제";
            else 
                return dt.ToString("yyyy.MM.dd");
        }
        return value;
    }
}
```
### Converter Resource Pack   
다음은 Converter를 사용하기 위해 선언되는 Resource 장소입니다.

파일 위치 **AppCampMessenger/Based/Template/ConverterStyles.xaml**   
```xaml
<ResourceDictionary xmlns="http://schemas.microsoft..."
                    xmlns:x="http://schemas.microsoft..."
                    xmlns:cvt="clr-namespace:AppCampMes...">
    
    <cvt:BooleanToReverseConverter x:Key="BooleanToReverseConverter"/>
    <cvt:BooleanToCollapsedConverter x:Key="BooleanToCollapsedConverter"/>
    <cvt:BooleanToVisibilityConverter x:Key="BooleanToVisibilityConverter"/>
    
    <cvt:TextBlockHyperLinkConverter x:Key="TextBlockHyperLinkConverter"/>
    <cvt:EmptyToVisibilityConverter x:Key="EmptyToVisibilityConverter"/>
    <cvt:ZeroToVisibilityConverter x:Key="ZeroToVisibilityConverter"/>
    <cvt:BoolToVisibilityConverter x:Key="BoolToVisibilityConverter"/>
    <cvt:BoolToCollapsedConverter  x:Key="BoolToCollapsedConverter"/>
    <cvt:ExistToVisibilityConverter x:Key="ExistToVisibilityConverter"/>
    <cvt:ZeroToCollapsedConverter x:Key="ZeroToCollapsedConverter"/>
    <cvt:EmptyToCollapsedConverter x:Key="EmptyToCollapsedConverter"/>
    <cvt:FriendDescriptionWidthConverter x:Key="FriendDescriptionWidthConverter"/>
    <cvt:LeftMarginConverter x:Key="LeftMarginConverter"/>

    <cvt:DateTimeToDisplayTextConverter x:Key="DateTimeToDisplayTextConverter"/>
    <cvt:AngleToIsLargeConverter x:Key="AngleToIsLargeConverter"/>
    <cvt:BytesToFormatConverter x:Key="BytesToFormatConverter"/>
</ResourceDictionary> 
```
이 영역에 새로 생성된 Converter를 등록해야만 `.xaml` 전역에서 Converter를 사용할 수 있습니다.

### Converter 적용 방법   
```xaml
<TextBox Text="{Binding Created, Converter={StaticResource DateTimeToDisplayTextConverter}}"/>
```

### 팁 
> API를 통해 전달된 최종 데이터를 ViewModel 또는 Model 단계에서 변환하는 것은 WPF의 UI Load 체계와 순서를 고려해봤을때 좋은 방법이 아닙니다. 원본 데이터를 보존하면서 Converter를 수십 수백개 확장하는 것은 올바른 방법입니다.   

## 브런치 전략
### 브런치 정의
  - **Master**
    개발용 원격 브런치로 로컬 브런치에서 작업한 기능을 테스트하는 목적으로 사용한다.
  - **Pre-Production**
    Master 브런치에서 테스트가 완료된 항목을 Commit하며, 배포 전 테스트하는 목적으로 사용한다.
  - **Production**
    Pre-Production 브런치에서 테스트가 완료된 항목을 Commit하며, 최종 또는 긴급 등 배포되는 최종 소스를 관리하는 목적으로 사용한다.
    
### 기능 개발
  1. `Pre-Production`에서 로컬 브런치 생성
  1. 기능 개발 후 `Master` 브런치에 Commit 후 테스트
  1. 테스트 완료 후 `Pre-Production` 브런치에 Commit
  
### 배포
  1. `Prouction` 브런치에 버전정보 `Tag` 추가 후 배포   
  
### 배포 버전 버그 픽스
  1. `Production`에서 로컬 브런치 생성
  2. 버그 픽스 후 `Master` 브런치에 `Commit` 후 테스트
  3. 테스트 완료 후 `Pre-Production`, `Production` 브런치에 `Commit`
  4. `Production` 브런치에 버전정보 `Tag` 추가 후 배포
    
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


# 주요 기능   
## 채팅   
  - 주요 항목    
    |항목|내용|
    |:---:|:---:|  
    |Protocol|[XMPP](<https://xmpp.org/>)|
    |Library|[Sharp.Xmpp](<https://github.com/pgstath/Sharp.Xmpp>)|
  - 설명   
    `Sharp.Xmpp` 라이브러리의 함수 및 이벤트를 기본으로 커스텀된 메시지를 수/발신합니다.   
    채팅방 초대, 메시지 수/발신, 채팅방 정보, 채팅방 참여자 정보 등 채팅의 전반적인 부분을 담당합니다.
    채팅 관련 이벤트, 오류 처리등이 구현되어 있습니다.
    Message 수신시에는 `Queue`에 추가되어 처리되므로, 하나씩 처리됩니다.
  - 참조 코드   
    ```csharp   
    var xmpp = XMPPManager.Instanse();
    
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

## Message Stanza  
  텍스트, 이미지, 동영상등 다양한 메시지가 있으며 자세한 사항은 아래를 참고하여 주시기 바랍니다.
  - 텍스트    
    ```xml      
	<message type="groupchat" to="545d2831-d85a-4b0ba1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr" msgtype="text" 
		 id="A45AB32E-A48D-4190-9389-A2161FBEE46D" nick="아이폰10" filetype="none" 
		 jid="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr" 
		 roomid="0A9E62BD-19CB-45D6-9133-3703504237AB" date="2019-09-26T06:00:55.733Z">
		<body>test</body>
	</message>
    ```  
  - 대화방 진입   
    ```xml      
	<message xmlns="jabber:client" type="groupchat" to="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr/CAMP_MOBILE" 
		 id="69C76EEADF28-4330-8FAC-45786C2CF2EE" nick="일이삼사오"msgtype="none" filetype="none"
		 jid="f0f5eab6ae4c47aa8d1d709cdaab98b9@xmpp.lubig.co.kr" roomid="F9ABBCC8-3BE7-4C12-9E0C-7A256BE75045" 
		 date="2019-09-26T06:09:00.809Z" from="ecbf173e-0b78-4abaa1c6-9ecf8eb93425@lubigmuc.xmpp.lubig.co.kr/f0f5eab6ae4c47aa8d1d709cdaab98b9">
		<body>join</body><joining to="f0f5eab6ae4c47aa8d1d709cdaab98b9@xmpp.lubig.co.kr" nick="일이삼사오"/>
	</message>
    ```  
  - 대화방 초대   
    ```xml      
	<message type="groupchat" to="545d2831-d85a-4b0ba1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr" id="C4CB003D-1D03-46D5-A76C-AFF17761E100" 
		 nick="아이폰10" msgtype="none" filetype="none" jid="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr" 
		 roomid="0A9E62BD-19CB-45D6-9133-3703504237AB" date="2019-09-26T06:06:05.929Z">
		<body>invite</body><invitation to="79f6481ce5d44d14b152e76831c3295f@xmpp.lubig.co.kr" nick="캠프사업부QA 강성태 대리"/>
	</message>
    ```  
  - 대화방 나가기   
    ```xml      
	<message xmlns="jabber:client" type="groupchat" to="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr/CAMP_MOBILE" 
		 id="35C901AE-7559-45BC-9699-E0609DC2EE8C" nick="김동언13" msgtype="none" filetype="none" jid="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr"
		 roomid="5895A694-E859-4E43-848A-6AD199B5D4AA" date="2019-09-26T06:07:22.422Z" 
		 from="545d2831-d85a-4b0b-a1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr/8f78d4642cf44bee9dfad5ca086fcef3">
		<body>exit</body>
		<exit to="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr"/>
	</message>
    ```  
  - 메시지 읽음   
    ```xml      
	<message xmlns="jabber:client" type="groupchat" to="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr/CAMP_MOBILE" 
		 id="2145AF6F-A890-431E-9AC2-C3E494BE9355" nick="김동언13" lastid="A45AB32EA48D-4190-9389-A2161FBEE46D" msgtype="read"
		 jid="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr" roomid="5895A694-E859-4E43-848A-6AD199B5D4AA" 
		 date="2019-09-26T06:00:56.351Z" from="545d2831-d85a-4b0b-a1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr/8f78d4642cf44bee9dfad5ca086fcef3">
		<readchat/>
	</message>
    ```  
  - 이미지   
    ```xml      
	<message type="groupchat" to="545d2831-d85a-4b0ba1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr" msgtype="file"
		 id="959808FB-7BAC-400F-84DB-02E93522302E" nick="아이폰10" filetype="image" 
		 jid="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr" roomid="0A9E62BD-19CB-45D6-9133-3703504237AB"
		 date="2019-09-26T06:03:42.470Z">
		<body>image</body>
		<image width="640" height="1386" size="2850797" 
		       url="https://duf7tkmfho2jb.cloudfront.net/chat/b71bc87bc51ff3008e56b0362f0e19ef/cfa5301358b9fcbe7aa45b1ceea088c6/image/9C6C89E0-49E6-4CC7-B688-EEF4D61EA7AEL0001.jpg"
		       thumbnailurl="https://duf7tkmfho2jb.cloudfront.net/chat/b71bc87bc51ff3008e56b0362f0e19ef/cfa5301358b9fcbe7aa45b1ceea088c6/image/thumbnail/9C6C89E0-49E6-4CC7-B688-EEF4D61EA7AEL0001_thumb.jpg"/>
	</message>
    ```  
  - 동영상   
    ```xml      
	<message type="groupchat" to="545d2831-d85a-4b0ba1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr" msgtype="file" 
		 id="A163DE9AB371-4313-879A-011960F1B9D6" nick="아이폰10" filetype="vod" jid="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr" 
		 roomid="0A9E62BD-19CB-45D6-9133-3703504237AB" date="2019-09-26T06:04:03.194Z">
		<body>vod</body>
		<vod width="404" height="720" size="1091187" 
		     url="https://duf7tkmfho2jb.cloudfront.net/chat/b71bc87bc51ff3008e56b0362f0e19ef/cfa5301358b9fcbe7aa45b1ceea088c6/vod/C32AAF39-A4EE-46A3-9B96-7C958799611EL0001_1569477843.mp4"
		     thumbnailurl="https://duf7tkmfho2jb.cloudfront.net/chat/b71bc87bc51ff3008e56b0362f0e19ef/cfa5301358b9fcbe7aa45b1ceea088c6/vod/thumbnail/C32AAF39-A4EE-46A3-9B96-7C958799611EL0001_1569477843.jpg" duration="11000"/>
	</message>
    ```  
  - 파일   
    ```xml      
	<message type="groupchat" to="545d2831-d85a-4b0ba1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr" msgtype="file" 
		 id="5176837B-CA79-47C9-A028-5AD8E3A21BE4" nick="아이폰10" filetype="file" jid="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr" 
		 roomid="0A9E62BD-19CB-45D6-9133-3703504237AB" date="2019-09-26T06:04:48.801Z">
		<body>file</body>
		<filesize="5654182" name="doc_1541073348685.zip" 
			  url="https://duf7tkmfho2jb.cloudfront.net/chat/b71bc87bc51ff3008e56b0362f0e19ef/cfa5301358b9fcbe7aa45b1ceea088c6/files/doc_1541073348685.zip"/>
	</message>
    ```  
  - 연락처   
    ```xml      
	<message type="groupchat" to="545d2831-d85a-4b0ba1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr" msgtype="file" 
		 id="23CE62FF-4462-4CCABEEE-5412550CAEA7" nick="아이폰10" filetype="contact" 
		 jid="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr" roomid="0A9E62BD-19CB-45D6-9133-3703504237AB" 
		 date="2019-09-26T06:05:37.605Z">
		<body>contact</body>
		<contact name="김나훈 과장님" memo="">
			<number>
				<item type="2"number="01022227229" label=""/>
			</number>
			<email/></contact>
	</message>
    ```  
  - 회수   
    ```xml      
	<message type="groupchat" to="545d2831-d85a-4b0ba1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr" id="BB80D2F8-55BD-4583-9CCC-01270ACE5B9F" 
		 nick="아이폰10" msgtype="none" filetype="none" jid="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr" 
		 roomid="0A9E62BD-19CB-45D6-9133-3703504237AB" date="2019-09-26T06:46:32.340Z">
		<body>del</body>
		<del msgid="23CE62FF-4462-4CCA-BEEE-5412550CAEA7" msgtime="2019-09-26T06:05:37.605Z"/>
	</message>
    ```  
  - 지도   
    ```xml      
	<message type="groupchat" to="545d2831-d85a-4b0ba1cf-acf2ee803e76@lubigmuc.xmpp.lubig.co.kr" msgtype="file"
		 id="93E1E919-78B4-404A-9477-3DAB92F9D24A" nick="아이폰10" filetype="map" jid="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr" 
		 roomid="0A9E62BD-19CB-45D6-9133-3703504237AB" date="2019-09-26T06:04:25.331Z">
		<body>map</body>
		<map latitude="37.482317" longitude="126.893794" address="서울특별시 구로구 구로동 235-5"/>
	</message>
    ```  
  - 이모티콘   
    ```xml      
	<message type="groupchat"
		 to="91b3730a-cfc6-4aec-b292-7090e2a89652@lubigmuc.xmpp.lubig.co.kr"
		 msgtype="file"
		 id="F9158CFA-E77D-4892-9A3D-9372DBB018FE"
		 nick="다리우스skskdkdjdjd"
		 filetype="emoticon"jid="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
		 roomid="518AA056-7CBB-4633-8BAF-4707D6926466"
		 date="2020-06-02T08:10:44.801Z"> 
		<body>emoticon<body/>
		<emoticon>23404</emoticon>
		<emoticonurl>https://duf7tkmfho2jb.cloudfront.net /emoji/20191007175815/detail/04.png </emoticonurl>
		<emoticontext>텍스트 내용을 이 곳에 추가</emoticontext>
	</message>
    ```
  - 투표 등록   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" date="2020-06-17T05:10:51.943Z"
		 nick="김동언 : 김동언" jid="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr"
		 to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="72e2e98c-9b52-4570-a763-21e010b38cf8"
		 from="95995593-0bd0-40de-b73e-73c22be07e46@lubigmuc.xmpp.lubig.co.kr/8f78d4642cf44bee9dfad5ca086fcef3"
		 resource="CAMP_MOBILE">
		<body>[투표] : 2</body>
		<vote status="registration" seq="973" notice="N" type="text" title="2">
			<item imgurl="">1</item>
			<item imgurl="https://duf7tkmfho2jb.cloudfront.net/vote/DA2FBE01-1CDD-4620-AFCC-EB7887E48294_1592370651513.jpg">2</item>
		</vote>
	</message>
    ```
  - 투표 취소   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" date="2020-06-17T05:10:56.788Z"
            nick="김동언 : 김동언" jid="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="3037ee48-cb2a-45bd-99d8-ef12d254e771"
            from="95995593-0bd0-40de-b73e-73c22be07e46@lubigmuc.xmpp.lubig.co.kr/8f78d4642cf44bee9dfad5ca086fcef3"
            resource="CAMP_MOBILE">
            <body>[투표취소] : 2</body>
            <vote seq="973" status="delete" title="2" />
	</message>
    ```
  - 투표 종료   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" date="2020-06-17T05:10:38.517Z"
            nick="김동언 : 김동언" jid="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="95224df9-65eb-4764-8be2-201b0e0ed0fd"
            from="95995593-0bd0-40de-b73e-73c22be07e46@lubigmuc.xmpp.lubig.co.kr/8f78d4642cf44bee9dfad5ca086fcef3"
            resource="CAMP_MOBILE">
            <body>[투표종료] : 1</body>
            <vote status="close" seq="972" notice="N" type="text" title="1">
                <item rank="0" imgurl="">1</item>
                <item rank="0" imgurl="">2</item>
                <item rank="0" imgurl="">3</item>
            </vote>
	</message>
    ```
  - 부캠프장 설정   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" nick="다리우스skskdkdjdjd"
            jid="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="0b19b3c1-f9c1-47fa-955d-799bd14afad4"
            from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/다리우스skskdkdjdjd"
            date="2020-06-17T06:35:22.447Z" roomid="17C99E25-01A2-4838-8367-C2D43D1DE1E6"
            resource="c4edd7d2dac84135ad5a660739fa2a8c">
            <body>camptransfer</body>
            <force campregno="1334" roomtitle="shajsjsiopo" to="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr"
                nick="김동언 : 김동언" memberlevel="2" type="set" />
            <force campregno="1334" roomtitle="shajsjsiopo" to="46b81db63f3a433cb1f36afbf86c7b20@xmpp.lubig.co.kr"
                nick="QAtest70입니다." memberlevel="2" type="set" />
	</message>
    ```
  - 부캠프장 해제   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" nick="다리우스skskdkdjdjd"
            jid="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="c0a0425b-61b4-4c1f-a1f6-d611dfafc134"
            from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/다리우스skskdkdjdjd"
            date="2020-06-17T06:35:48.232Z" roomid="17C99E25-01A2-4838-8367-C2D43D1DE1E6"
            resource="c4edd7d2dac84135ad5a660739fa2a8c">
            <body>camptransfer</body>
            <force campregno="1334" roomtitle="shajsjsiopo" to="" nick="" memberlevel="2" type="remove" />
	</message>
    ```
  - 캠프장 이관  
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" nick="다리우스skskdkdjdjd"
	        jid="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
	        to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="4f043712-0f92-4643-bc1d-9d5b7a0fb192"
	        from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/다리우스skskdkdjdjd"
	        date="2020-06-17T07:07:45.948Z" roomid="17C99E25-01A2-4838-8367-C2D43D1DE1E6"
	        resource="c4edd7d2dac84135ad5a660739fa2a8c">
	        <body>camptransfer</body>
	        <force campregno="1334" roomtitle="shajsjsiopo" to="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr"
        	nick="김동언 : 김동언" memberlevel="1" type="change" />
	</message>
    ```
  - 캠프회원 강퇴   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" nick="다리우스skskdkdjdjd"
            jid="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr" roomid="10f1fd2d-f8b7-4b4a-9503-323bf74eb284"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="b4682666-e2e2-48ae-81ac-cab4dafde3c1"
            from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/다리우스skskdkdjdjd"
            date="2020-06-17T07:11:54Z" resource="c4edd7d2dac84135ad5a660739fa2a8c">
            <body>forceexit</body>
            <force to="46b81db63f3a433cb1f36afbf86c7b20@xmpp.lubig.co.kr" campregno="1334" roomtitle="shajsjsiopo"
                nick="QAtest70입니다." />
	</message>
    ```
  - 예약 메시지   
    ```xml      
	<message xmlns="jabber:client" to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
            id="420FA750-F31C-4EB9-9957-1B03C4ACC8E7" type="chat" msgtype="none" filetype="none"
            date="2020-06-17T06:10:00.005Z" from="8f78d4642cf44bee9dfad5ca086fcef3@xmpp.lubig.co.kr/CAMP_SERVER">
            <body>너ㅏㄴ</body>
            <reservemsg to="c4edd7d2dac84135ad5a660739fa2a8c" nick="다리우스skskdkdjdjd">너ㅏㄴ</reservemsg>
	</message>
    ```
  - 레포트   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat"
            from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/커머스 트렌드"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" nick="커머스 트렌드"
            jid="213544db2e5b4fa4affa2ce4c2c1df01@xmpp.lubig.co.kr" id="9426166a-fe49-46d0-bd6f-c12398c46068"
            date="2020-06-17T06:48:34.330Z" roomid="10f1fd2d-f8b7-4b4a-9503-323bf74eb284"
            resource="213544db2e5b4fa4affa2ce4c2c1df01">
            <body>안녕하세요! 모비트렌드입니다.😊
                🗓 5월 1째주 (5/04 ~ 5/10)
                커머스 주간리포트 보내드립니다.
                📌 선택 카테고리
                : 쇼핑 &gt; 식품 &gt; 농수산식품종합
                📈 전주대비 데이터
                ✔방문자 수 : 0.0% 상승 (+0.0%)
                ✔매출 : 14.52% 상승 (+14.52%)
                ✔전환율 : 8.06% 상승 (+8.06%)
                ✔객단가 : 2.29% 하락 (-2.29%)
                [※신뢰구간 오차범위 ±7%]</body>
            <report title="커머스 주간 리포트"
                iconurl="https://duf7tkmfho2jb.cloudfront.net/serviceInfo/Report/report_default.png"
                servicetype="commerce" graphgubun="Y" sDeptKey="181" />
	</message>
    ```
  - API 연동   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat"
            from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/커머스 트렌드"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" nick="커머스 트렌드"
            jid="213544db2e5b4fa4affa2ce4c2c1df01@xmpp.lubig.co.kr" id="6420a259-d73c-421a-b7cb-254203a3442a"
            date="2020-06-17T06:54:04.939Z" resource="213544db2e5b4fa4affa2ce4c2c1df01">
            <body>커머스 트렌드 연동이 설정 되었습니다.</body>
            <apiconnect to="213544db2e5b4fa4affa2ce4c2c1df01@xmpp.lubig.co.kr" nick="커머스 트렌드" serviceId=""
                profileimagethumbnailurl="https://duf7tkmfho2jb.cloudfront.net/serviceInfo/Commerce/commerce_thumbnail.jpg"
                campmemberprofileyn="N" servicetype="commerce" />
	</message>
    ```
  - API 연동 해제   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat"
            from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/커머스 트렌드"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" nick="커머스 트렌드"
            jid="213544db2e5b4fa4affa2ce4c2c1df01@xmpp.lubig.co.kr" id="50365d7b-660c-4e95-9b71-97b338a3cf65"
            date="2020-06-17T06:54:29.702Z" resource="213544db2e5b4fa4affa2ce4c2c1df01">
            <body>커머스 트렌드 연동이 해제 되었습니다.</body>
            <apidisconnect to="213544db2e5b4fa4affa2ce4c2c1df01@xmpp.lubig.co.kr" nick="커머스 트렌드"
                servicetype="commerce" />
	</message>
    ```
  - API 연동 수정   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat"
            from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/커머스 트렌드"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" nick="커머스 트렌드"
            jid="213544db2e5b4fa4affa2ce4c2c1df01@xmpp.lubig.co.kr" id="72d4a94f-f647-46f5-8f8a-d8087bcd20d7"
            date="2020-06-17T06:55:14.164Z" resource="213544db2e5b4fa4affa2ce4c2c1df01">
            <body>커머스 트렌드 연동이 수정 되었습니다.</body>
            <apirevision to="213544db2e5b4fa4affa2ce4c2c1df01@xmpp.lubig.co.kr" nick="커머스 트렌드" servicetype="commerce" />
	</message>
    ```
  - 캠프 가입   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" nick="QAtest70입니다."
            jid="46b81db63f3a433cb1f36afbf86c7b20@xmpp.lubig.co.kr"
            to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="bd18fc24-7aee-40bd-8029-85666e795218"
            from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/QAtest70입니다."
            date="2020-06-17T06:34:57.284Z" resource="46b81db63f3a433cb1f36afbf86c7b20">
            <body>join</body>
            <joining to="46b81db63f3a433cb1f36afbf86c7b20@xmpp.lubig.co.kr" nick="QAtest70입니다."
                profileimageurl="https://duf7tkmfho2jb.cloudfront.net/chat/20200420/0211/000331fdb32b4d90b19aebfc936ab5fd_.PNG"
                profileimagethumbnailurl="https://duf7tkmfho2jb.cloudfront.net/chat/20200420/0211/Thumbf5c73796aa38454fbe7b8a09335bf242_.PNG"
                campmemberprofileyn="N" servicetype="" />
	</message>
    ```
  - 일정 등록    
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" nick="다리우스skskdkdjdjd"
		 jid="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
		 to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="c0a0425b-61b4-4c1f-a1f6-d611dfafc134"
		 from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/다리우스skskdkdjdjd"
		 date="2020-06-17T06:35:48.232Z" roomid="17C99E25-01A2-4838-8367-C2D43D1DE1E6"
		 resource="c4edd7d2dac84135ad5a660739fa2a8c">	
		<body>일정이 등록되었습니다.</body>
		<schedule type="registration" seq="89" startDate="2020-09-09T05:35:58." endDate="2020-09-09T05:35:58" 
			  allDayYn="Y" color="4" to="1ccda7b18bad4e0787f915d80c0ee3e1">
			<title>제목</title>
			<desc>설명문</desc>
			<alarm date="2020-09-09T05:35:58" text="10분"/>
			<alarm date="2020-09-09T05:35:58" text="10분" />
		</schedule>
	</message>
    ```
  - 일정 공유   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" nick="다리우스skskdkdjdjd"
		 jid="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
		 to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="c0a0425b-61b4-4c1f-a1f6-d611dfafc134"
		 from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/다리우스skskdkdjdjd"
		 date="2020-06-17T06:35:48.232Z" roomid="17C99E25-01A2-4838-8367-C2D43D1DE1E6"
		 resource="c4edd7d2dac84135ad5a660739fa2a8c">	
		<body>일정이 공유되었습니다.</body>
		<schedule type="share" seq="90" startDate="2020-09-09T05:35:58" endDate="2020-09-09T05:35:58"  to="1ccda7b18bad4e0787f915d80c0ee3e1">
			<title>제목</title>
			<desc>설명문</desc>
			<alarm date="2020-09-09T05:35:58" text="10분"/>
			<alarm date="2020-09-09T05:35:58" text="10분" />
		</schedule>
	</message>
    ```
  - 일정 알림   
    ```xml      
	<message xmlns="jabber:client" msgtype="none" filetype="none" type="groupchat" nick="다리우스skskdkdjdjd"
		 jid="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr"
		 to="c4edd7d2dac84135ad5a660739fa2a8c@xmpp.lubig.co.kr/CAMP_MOBILE" id="c0a0425b-61b4-4c1f-a1f6-d611dfafc134"
		 from="10f1fd2d-f8b7-4b4a-9503-323bf74eb284@lubigmuc.xmpp.lubig.co.kr/다리우스skskdkdjdjd"
		 date="2020-06-17T06:35:48.232Z" roomid="17C99E25-01A2-4838-8367-C2D43D1DE1E6"
		 resource="c4edd7d2dac84135ad5a660739fa2a8c">	
		<body>10분 후 일정이 있습니다.</body>
		<schedule type="notice" seq="89" startDate="2020-09-09T05:35:58" endDate="2020-09-09T05:35:58" to="1ccda7b18bad4e0787f915d80c0ee3e1"
			  sysUserProfileUrl="">
			<title>제목</title>
			<desc>설명문</desc>
			<alarm date="2020-09-09T05:35:58" text="10분"/>
			<alarm date="2020-09-09T05:35:58" text="10분" />
		</schedule>
	</message>
    ```
  - 선물하기   
    ```xml      
	<message to='c9bc8010584b4580ac91405b0e4e706b@xmpp.lubig.co.kr' id='T2W2N-09301750' 
		 type='chat' msgtype='none' filetype='none' date='2019-10-07T03:27:42.102Z'  nick='LeeCheol ho'
		 from='9d6a86cebc224ab0ba3cd09c46b27a65@xmpp.lubig.co.kr/CAMP_MOBILE'>
		<body>present</body>
		<present presenttype='coupon' thumbnail='http://img.giftting.co.kr/goods/G00000117157/G00000117157_250.jpg' 
			 itemname='치킨세트' productid='G00000104532' to='c9bc8010584b4580ac91405b0e4e706b'>치킨 선물이야~ 맛있게 먹어~~ ^^
		</present>
	</message>
    ```
## API   
Lubig sever api를 이용하여 로그인, 친구목록, 대화방 기존 대화글 불러오기등 여러 기능을 수행한다.
  - Address
    |항목|Url|
    |:---:|:---:|  
    |Lubig|<https://api.lubig.co.kr>|
    |Store|<https://storeapi.lubig.co.kr>|
    |Admin|<https://admin.lubig.co.kr>|    
  - 설명        
    `HttpWebRequest` 를 이용하여 `Json` 형식으로 데이터를 수/발신한다.
    각 서버에서 지원하는 API를 이용하여 해당 기능을 수행한다.       
    
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
    AWS의 S3/EC2를 이용하여 파일을 업로드 및 다운로드한다.   
    비동기로 동작하며 업로드시에는 Thread, 다운로드시에는 event로 처리한다.
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
    유저별 대화방, 참여자 정보, 상단고정, 알림설정등의 정보를 저장한다. 
  - 참조 코드   
    ```csharp   
    var sql = SqlManager.Instanse();
    
    CampMemberListInfo memberInfo = null;
    if (!sql.SelectCampMemberInfoData(out memberInfo, userKey, campRegNo.ToString()))
    {
		return false;
    }
    ```
  - ERD   
    Entity Relationship Diagram으로 클릭하여 확대하거나 저장하면 자세히 볼 수 있습니다.
    <img style="width: 680px; height: 500px" src="https://github.com/foryouself83/testrepo/blob/main/src/Images/PC_Camp_ERD.png?raw=true"/>
    
## 자동 업데이트      
  - 설명   
    프로그램 버전정보를 비교하여 낮을 경우 Login시 프로그램을 업데이트한다.
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
2020년 1월 13일 기준으로 작성되었습니다.
## RedMine
  - 신규, 진행, 재열람 건 22건에 대한 수정이 안되어 있습니다.
  - 재현 불가등 사유로 보류 43건에 대한 수정이 안되어 있습니다.
  
## 속도 개선
  - 대화방 목록 Template화
  - 중복된 Image 랜더링 제거(Image manager class 필요)
  - API 호출시 비동기 처리 추가    
  
## Opensource library
  - opensource의 licence에 대한 확인이 필요하며, licence에 따라 library의 변경이 필요할 수 있습니다.
  - opensource의 licence에 대한 고지가 프로그램에 추가되어야 합니다.
  
## DB 최적화
  - 대화방, 메시지 관련 데이터 중복 사항이 많아 정규화가 필요합니다.   
  - 메시지의 경우 Stanza를 모두 저장하는 column을 추가하여 신규 메시지에 대한 확장성을 제공해야 합니다.
  
## 리뉴얼 디자인
  - zeplin에 등록된 리뉴얼된 디자인이 적용되어야 합니다.  
    
# 최근 개발 항목
2021년 1월 13일기준으로 가장 최근에 개발한 항목으로 작성되었습니다.
세부 사항의 경우 Git Commit 이력을 참고하시기 바랍니다.
|항목|담당자|배포일|비고|
|:---:|:---:|:---:|:---:|  
|캠프 1:1대화|유병석|2020.01.11|프로그램 버전:0.8.99|
|프로그램 안정화|안동현|2020.01.11|프로그램 버전:0.8.99|
|대화방 공지사항 게시판|이재웅|-|Master branch에 Commit|

# 참고사항

## 시퀀스 다이어그램
### 로그인     
  <img style="width: 300px; height: 500px" src="https://github.com/foryouself83/testrepo/blob/main/src/Images/Login_seq.png?raw=true"/>

# 이재웅 인수인계서   
## Notice (공지)
### Overview   
공지`Notice`는 캠프 대화창을 기준으로 관리되는 게시판 형태의 화면입니다.  

- **논리적 계층 구조**
  - **NoticeBar** *ToggleButton* `NoticeStyles.Bar.xaml`
    - **NoticeWindow** *Window* `NoticeStyles.Window.xaml`
      - **NoticeList** *ListBox* `NoticeStyles.List.xaml`
      - **NoticeComment** *TreeView* `NoticeStyles.Hierachy.xaml`

### NoticeBar   
해당 캠프 대화창의 공지가 존재할 경우 아래 참고 `camp01.png` 이미지 처럼 화면 상단 영역에 **NoticeBar**가 활성화됩니다.   

<img src="data:image/png;base64,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"></img>   
*camp01.png*   

이 컨트롤은 **토글 스위칭** 형태로 공지 내용이 펼쳐지거나 또는 NoticeWindow 창을 실행합니다.   

- **Notice** : *System.Windows.Controls.Primitives.ToggleButton*   
  ```csharp
  public class Notice : Control
  {
      public Notice()
      {
          DataContext = new NoticeViewModel();    
      }
  }
  ```
- **NoticeViewModel** *ObservableObject MVVM*   

  > NoticeViewModel 생성 시 **TalkRoomInfo** 파라메터가 필수로 필요합니다.   
  ```csharp
  public NoticeViewModel(TalkRoomInfo room)
  {
      Room = room;
      InitNoticeData(room);
  }
  ```
  *NoticeViewModel 파라메터 추가 시 다중 생성자 형식으로 확장을 권장합니다.*

- #### NoticeBar 인스턴스 생성 시점    

  ```csharp
  public class TalkWindowViewModel : WindowViewModelBase
  {
      ...
      public void InitControls(string roomId)
      {
          // 대화창이 생성되는 시점입니다.
          ...
          TalkRoomInfo room = LubigAPIManager.Instanse().DataInfo.FindTalkRoomInfo(roomId);
          NoticeInfo = new NoticeViewModel(room);
      }
  }
  ```
  > 추후 공지 영역을 다른 대화창 또는 기능에 연동할 경우 **NoticeViewModel** 생성자 부분의 파라메터 확장을 통해 처리하는 것이 좋습니다.
