# 개요
텍스트, 이미지, 이모티콘 등 메시지 전송 기능과 이미지, 동영상등을 업로드하여   
친구들과 같은 모임에 있는 사람들에게 노출시킬 수 있는 피드 기능을 제공하는 프로그램입니다.

# 목차
* [개발 환경](<https://github.com/foryouself83/CampMessenger#%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD>)   
* [기본 구조](<https://github.com/foryouself83/CampMessenger/blob/main/README.md#%EA%B8%B0%EB%B3%B8-%EA%B5%AC%EC%A1%B0>)   
* [주요 기능](<https://github.com/foryouself83/CampMessenger/blob/main/README.md#%EC%A3%BC%EC%9A%94-%EA%B8%B0%EB%8A%A5>)   
* [주요 화면](<https://github.com/foryouself83/CampMessenger/blob/main/README.md#%EC%A3%BC%EC%9A%94-%ED%99%94%EB%A9%B4>)   
* [참고 사항](<https://github.com/foryouself83/CampMessenger/blob/main/README.md#%EC%B0%B8%EA%B3%A0-%EC%82%AC%ED%95%AD>)     

# 개발 환경   
|항목|내용|
|:---:|:---:|  
|개발 언어|C# / WPF / .NetFramework 4.6.1|
|DB|System.Data.Sqlite|
|IDE|VisualStudio 2019 Pro|
|SCM|Git|
|Design|Zeplin|   

# 기본 구조
Resource, DataContext 항목들의 규칙과 방향성을 인지하고 개발의 일관성을 갖기위한 구조입니다.
## ResourceDictionary
모든 리소 관리에 대한 정책, 개발 확장 방법에 대한 구조를 설명합니다.   
### 리소스 관리대상 주요 위치   

> *XXX/App.xaml*   
> *XXX/Based/Template/*

- **분류된 리소스 항목**   
  그 외에 분류되지 않은 리소스 객체들은 `ApplicationStyles.xaml`에 위치하고 있습니다.
  
  - **Template**
    - XXXStyles.xaml   
    
  리소스를 화면 또는 컨트롤, 기능별로 상황에 맞게 분류하도록 귀칙을 정하고 있으므로 향후 개발, 확장에 있어서도 리소스를 최대한 파일별로 나누어 관리하도록 권장합니다.
  *(리소스 파일의 순서와 순환참조에 관하여 아래에서 설명)*
- **MergedDictionaries**   
  추후 분류되는 리소스 파일은 아래 `App.xaml` MergedDictionaries 컬렉션에 추가해야 합니다.
  > 먼저 호출되는 리소스를 기준으로 순서를 배치하는 것이 중요합니다. Converter, Color, Path와 같은 리소스는 다른 리소스의 참조형이므로 상위에 위치하도록 하며 참조 흐름을 잘 확인하여 배치하는 것이 중요합니다.   
  
  ```xaml
  <Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            ...
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
  <DrawingImage x:Key="...">
    <DrawingImage.Drawing>
        ...
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
  <Style TargetType="Path" x:Key="PATH_...">
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
using XXX.Based.Converters;
```

### 생성 요건   
*다음 생성 요건에 충족할 경우 Converter를 생성합니다.*   
  1. Property 값의 변경이 Display에서만 적용해야 하는 경우.
  2. Binding Mode가 OneWay 타입일 경우.
  3. 반복되는 데이터 양이 많지 않으며 성능에 영향을 주지 않을 정도(로드)의 처리량.   
  
**컨버터 확장**   
> Xaml Display에 출력되는 Data Modlel의 원본`Raw`을 유지하는 것은 매우 중요합니다. Model 또는 ViewModel을 통해 연계되는 DataContext Binding을 가능하도록 유지하기 위해 Replace성 Converter를 적극 활용하는것을 권장합니다.

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

**예를들어**    

```csharp
public class XXXConverter : IValueConverter
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

**처리 로직이 필요한 컨버터의 경우** 
```csharp
public class XXXConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is string strDt)
        {
            //...
        }
        return value;
    }
}
```
### Converter Resource Pack   
다음은 Converter를 사용하기 위해 선언되는 Resource 장소입니다.

파일 위치 **XXX/Based/Template/ConverterStyles.xaml**   
```xaml
<ResourceDictionary xmlns="http://schemas.microsoft..."
                    xmlns:x="http://schemas.microsoft..."
                    xmlns:cvt="clr-namespace:...">
    
    <cvt:...Converter x:Key="...Converter"/>
    ...
</ResourceDictionary> 
```
이 영역에 새로 생성된 Converter를 등록해야만 `.xaml` 전역에서 Converter를 사용할 수 있습니다.

### Converter 적용 방법   
```xaml
<TextBox Text="{Binding Created, Converter={StaticResource XXXConverter}}"/>
```

### 팁 
> API를 통해 전달된 최종 데이터를 ViewModel 또는 Model 단계에서 변환하는 것은 WPF의 UI Load 체계와 순서를 고려해봤을때 좋은 방법이 아닙니다. 원본 데이터를 보존하면서 Converter를 수십 수백개 확장하는 것은 올바른 방법입니다.   

## 브런치 전략
### 브런치 정의
  - **Master**
    개발용 원격 브런치로 로컬 브런치에서 작업한 기능을 테스트하는 목적으로 사용합니다.
  - **Pre-Production**
    Master 브런치에서 테스트가 완료된 항목을 Commit하며, 배포 전 테스트하는 목적으로 사용합니다.
  - **Production**
    Pre-Production 브런치에서 테스트가 완료된 항목을 Commit하며, 최종 또는 긴급 등 배포되는 최종 소스를 관리하는 목적으로 사용합니다.
    
### 기능 개발
  1. `Pre-Production`에서 로컬 브런치 생성
  1. 기능 개발 후 `Master` 브런치에 Commit 후 테스트
  1. 테스트 완료 후 로컬 브런치를 `Pre-Production` 브런치에 Commit
  
### 배포
  1. `Prouction` 브런치에 버전정보 `Tag` 추가 후 배포   
  
### 배포 버전 버그 픽스
  1. `Production`에서 로컬 브런치 생성
  1. 버그 픽스 후 `Master` 브런치에 `Commit` 후 테스트
  1. 테스트 완료 후 로컬브런치를 `Pre-Production`, `Production` 브런치에 `Commit`
  1. `Production` 브런치에 버전정보 `Tag` 추가 후 배포
    
## 배포
### 서명 방법
  1. AssemblyInfo.cs 에 AssemblyVersion, AssemblyFileVersion 갱신   
  1. 인증서와 `signtool.exe`을 이용하여 실행파일 서명   
  1. `Installshield`를 이용하여 설치 파일 생성   
  1. 인증서와 `signtool.exe`을 이용하여 설치파일 서명
  
#### 팁
  > 편의를 위해 `batch` 또는 `Installshield`의 빌드 전 처리를 사용해 자동화해놓으면 좋습니다.
  
### 배포 방법  
  1. 관리자페이지 접속
  1. 관리자페이지 로그인
  1. 설정 > PC 버전관리 > 신규 버전 등록 선택   

# 주요 기능   
## 채팅   
`Openfire Server` 기반의 [XMPP](<https://xmpp.org/>) 프로토콜을 사용하며, [Sharp.Xmpp](<https://github.com/pgstath/Sharp.Xmpp>) 라이브러리의 함수 및 이벤트를 기본으로 커스텀된 메시지를 수/발신합니다.   
채팅방 초대, 메시지 수/발신, 채팅방 정보, 채팅방 참여자 정보 등 채팅의 전반적인 부분을 담당합니다.   
채팅 관련 이벤트, 오류 처리등이 구현되어 있습니다.   
Message 수신시에는 `Queue`에 추가되어 하나씩 처리됩니다.   

## API   
`HttpWebRequest` 를 이용하여 `Json` 형식으로 데이터를 수/발신한다.
각 서버에서 지원하는 API를 이용하여 해당 기능을 수행한다.       
    
## AWS
AWS의 S3/EC2를 이용하여 파일을 업로드 및 다운로드한다.   
비동기로 동작하며 업로드시에는 Thread, 다운로드시에는 event로 처리한다.   

    
## DB
유저별 대화방, 참여자 정보, 상단고정, 알림설정등의 정보를 저장한다.    


## 자동 업데이트      
프로그램 버전정보를 비교하여 낮을 경우 Login시 프로그램을 업데이트한다.   

# 주요 화면
## 로그인
  > ID, Password 입력하여 로그인 및 자동업데이트됩니다.
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/LoginWnd.png?raw=true"/>   
  
## 친구 목록
  > 간략한 자신 및 친구 프로필이 노출되며, 가입된 캠프 목록이 최대 10개까지 노출됩니다.    
  > 친구 닉네임으로 검색할 수 있습니다.   
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/friendsTab.png?raw=true"/>
  
## 친구 프로필
  > 친구와 1:1 대화 또는 친구 닉네임을 변경할 수 있습니다.   
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/ProfileWnd.png?raw=true"/>
  
## 채팅방 목록
  > 채팅 가능한 채팅방 목록이 모두 노출되며 시간별, 안읽은 메시지 순으로 정할 수 있습니다.    
  > 채팅방 이름 변경, 나가기, 알림 설정, 상단 고정, 검색할 수 있습니다.   
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/talkListTab.png?raw=true"/>
  
## 소식
  > 친구의 프로필 변경 또는 게시글 작성 소식을 확인 할 수 있습니다.   
  > 친구 이름, 해시태그, 게시글로 검색이 가능합니다.   
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/FeedTab.png?raw=true"/>
  
## 가입 캠프 목록
  > 내가 가입한 모든 캠프 목록이 노출되며 검색할 수 있습니다.
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/CampListWnd.png?raw=true"/>
  
## 캠프 홈
  > 캠프 게시글이 최신 순으로 노출되며 `이미지`, `동영상`, `파일`, `해시태그`, `Url`등을 사용할 수 있습니다.   
  > `좋아요`, `댓글` 또는 `대댓글`을 작성할 수 있습니다.
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/CampHome.png?raw=true"/>
  
## 캠프 그룹방 목록
  > 캠프별로 있는 그룹방으로 공개, 비공개 방으로 분류되며 공개방의 경우 참여하지 않아도 노출됩니다.
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/CampGroupTalkList.png?raw=true"/>
  
## 캠프 대화(일반 대화)
  > `텍스트`, `이미지`, `동영상`, `이모티콘`, `연락처`, `지도`, `선물` 등 여러 메시지를 전송할 수 있습니다.   
  > 대화방의 친구 초대, 나가기, 알람 설정할 수 있습니다.
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/CampTalk.png?raw=true"/>
  
## 캠프 프로필
  > 캠프 친구의 소식을 구독할 수 있습니다.
  <img width="20%" height="20%" src="https://github.com/foryouself83/CampMessenger/blob/main/src/Images/CampProfileWNd.png?raw=true"/>
  
# 참고 사항  
## 모바일 어플리케이션 링크
  [google play](<https://play.google.com/store/apps/details?id=com.enliple.lubig>)
