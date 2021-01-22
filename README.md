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
  - 이모티콘   
    ```xml      
	<message xmlns="jabber:client" type="groupchat" to="daeba3a140e54f7ca5990bb953276cb6@xmpp.lubig.co.kr/CAMP_MOBILE" 
		 id="69C76EEADF28-4330-8FAC-45786C2CF2EE" nick="일이삼사오"msgtype="none" filetype="none"
		 jid="f0f5eab6ae4c47aa8d1d709cdaab98b9@xmpp.lubig.co.kr" roomid="F9ABBCC8-3BE7-4C12-9E0C-7A256BE75045" 
		 date="2019-09-26T06:09:00.809Z" from="ecbf173e-0b78-4abaa1c6-9ecf8eb93425@lubigmuc.xmpp.lubig.co.kr/f0f5eab6ae4c47aa8d1d709cdaab98b9">
		<body>join</body><joining to="f0f5eab6ae4c47aa8d1d709cdaab98b9@xmpp.lubig.co.kr" nick="일이삼사오"/>
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

