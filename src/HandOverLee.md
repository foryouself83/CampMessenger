# ENLIPLE Camp for PC
본 문서는 `(캠프 PC 프로젝트 개발 건)` 업무 설명, 범위 및 인수인계 관련 내용을 담고 있습니다.
   
#### ***인수인계 작성자:*** ####
**이재웅** 과장   
서비스사업본부 > 캠프사업부 > 개발팀 > PC파트   
> 사원번호: 09-043

## 인수인계 범위와 설명
캠프 PC 프로젝트의 인수인계 관련 내용 선정은 `WPF 클라이언트의 Based 구조` 그리고 `공지사항 구조`를 각각 세부으로 분류하여 학목별로 자세히 설명합니다.   

#### 1. 기반 기술 키워드
***C#, WPF, ControlTemplate, DataTemplate, Trigger, DataTrigger, Binding, RelativeBinding*** 등의 WPF주요 기반 기술들을 포함하고 있습니다.

#### 2. 세부 구조
각각의 카테고리 별 세부 구조는 다음과 같습니다.
> *Git repo > {root}/AppCampMessenger/Based/*   

  - **Based Struct**
    - ResourceDictionary
    - Template
    - Converter
  - **공지 (Notice)**
    - NoticeBar
    - NoticeWindow
## Based Struct
향후 캠프 개발의 유지/추가 개발에 앞서 Based Struct 항목의 Resource, DataContext 항목들의 규칙과 방향성을 반드시 인지하고 주의사항을 확인하는 것을 권장합니다.
### 1. ResourceDictionary
ResourceDictionary는 이 앱에 사용되고 있는 모든 리스소 관리에 대한 정책, 개발 확장 방법에 대한 구조를 설명합니다.   
#### 리소스 관리대상 주요 위치   

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
### 2. Geometry   

**Zeplin** 디자인 팀간 협업   

캠프 프로젝트는 기본적으로 Zeplin 플랫폼에서 `.svg`와 `.png` 모두 전달받고 있으며 누락되는 리소스는 디자인팀에 요청하고 있습니다.   


디자인 리소스는 기본적으로 `Geometry` Path Data를 권장하며 그 외에 `Vector` 기반이 아닌 이미지인 경우 `.png` 사용합니다. 그리고 Geometry 관련 리소스는 이 파일`PathStyles.xaml`에서 관리합니다.   

**Svg 변환**    

[SvgToXaml](https://github.com/BerndK/SvgToXaml) - 대중적으로 가장 많이 쓰이고 있는 오픈소스 변환 프로그램입니다.   

> Geometry는 일러스트레이터를 통해 작업된 최종 결과물을 `.png`가 아닌 `.svg` 형태로 넘겨받아 변환작업 할 수 있도록 합니다.   



#### Geometry Resource 위치
```
AppCampMessenger/Based/Template/PathStyles.xaml
```
- #### DrawingImage
  1개 이상의 복합 Geomatry 리소스 `DrawingImage` 형식
  ###### PathStyles.xaml   
  
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
  
  **끝으로**   
  
  > 상황에 따라 Geometry를 적용하는 3가지 방식을 기반으로 리소스를 타이트하게 관리해 나가는 것은 디자인의 일관성과 관리성 측면에서 매우 중요한 요소입니다.

### 3. Converter
IValueConverter를 관리 운영 및 확장하기 위해 아래 생성 요건과 확장에 대한 유의사항, 그리고 기존 목록과 중복되지 않도록 관련 내용들을 확인이 반드시 필요합니다.
#### 네임스페이스   
```csharp
using AppCampMessenger.Based.Converters;
```

#### 생성 요건   
*다음 생성 요건에 충족할 경우 Converter를 생성합니다.*   
  1. Property 값의 변경이 Display에서만 적용해야 하는 경우.
  2. Binding Mode가 OneWay 타입일 경우.
  3. 반복되는 데이터 양이 많지 않으며 성능에 영향을 주지 않을 정도(로드)의 처리량.   
  
**컨버터 확장**   
> Xaml Display에 출력되는 Data Modlel의 원본`Raw`을 유지하는 것은 매우 중요합니다. Model 또는 ViewModel을 통해 연계되는 DataContext Binding을 가능하도록 유지하기 위해 Replace성 Converter를 적극 활용하는것을 권장합니다.
  
#### Converter 목록   
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

#### Converter 작성 요령
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
#### Converter Resource Pack   
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

#### Converter 적용 방법   
```xaml
<TextBox Text="{Binding Created, Converter={StaticResource DateTimeToDisplayTextConverter}}"/>
```

**끝으로**   
> API를 통해 전달된 최종 데이터를 ViewModel 또는 Model 단계에서 변환하는 것은 WPF의 UI Load 체계와 순서를 고려해봤을때 좋은 방법이 아닙니다. 원본 데이터를 보존하면서 Converter를 수십 수백개 확장하는 것은 올바른 방법입니다.

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
  
## 끝으로 
추가적으로 더 설명이 필요한 부분이 있거나 범위 내에서 요청할 부분이 있을 시 아래 이메일 또는 휴대전화로 연락 주시기 바라며 이상으로 문서를 마치겠습니다.
> 작성자 이재웅   
> 이메일 [ijaewung@outlook.com](ijaewung@outlook.com)   
> 연락처 010-6693-4033   

