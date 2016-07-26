# README #

## Unity5 Facebook 유니티 연동 작업 ##

페이스북 플러그인 사용 유니티와 연동.

DB = mysql
서버 = nodejs

## 페이스북 plugin 임포트 ##

version 7.2.2

[unity sdk](https://developers.facebook.com/docs/unity/)

먼저 테스트 할 앱을 만들어야됨. 

[페이스북 개발자 페이지](https://developers.facebook.com/apps/)

새앱추가. 이름은 알아서.. 저는 fatest

대시 보드 화면의 appId 기억.

초기 설정

Facebook-Edit Settings

에서 app name과 app Id 입력

build settings에 페이스북 example의 씬 모두 임포트 시키고 MainMenu를 가장 위로 위치시킨다.

그리고 실행 후 테스트.. 안드로이드 폰으로 테스트시. 에러 발생

Error: [Temp\StagingArea\AndroidManifest-main.xml:24, C:\Users\Administrator\Documents\FacebookTest\Temp\StagingArea\android-libraries\facebook-android-sdk-4.7.0\AndroidManifest.xml:3] Main manifest has <uses-sdk android:minSdkVersion='9'> but library uses minSdkVersion='15'

minsdk 15로 맞추고 다시 빌드

안드로이드 빌드시..

페이스북 대시보드 - 설정 - 기본설정 - 플랫폼 추가 - 안드로이드 선택

빈칸 입력 (유니티의 facebook-Edit settings에 나와있는 값 그대로 입력)
sso - on
(sso on시 나타남) 딥링킹 - on 설정

변경내용 저장.

원하는 작업을 진행하면 될듯..

## 페이스북 Example 설명 ## 


## 게임에 사용되는 DB 스키마 정보 ##

사용자 정보 파악

서버를 구성해 기본적으로 작동할 수 있게 만들었지만 이 상태로는 어떤일도 제대로 처리할수 없습니다.

원하는 형태로 서버를 반응할 수 있도록 구성하려면 먼저 사용자가 게임을 통해서 어떤 일을 할 수 있는지 정의해야 합니다.

범위            | 가능한일
----------------|--------
 사용자(단독)    |  게임 플레이를 통한 코인 획득 및 점수 기록 <br>코인을 사용해 공격력, 방어력 업그레이드 <br>코인을 사용해 아이템 구매 <br>보석을 사용해 게임 플레이를 할 수 있도록 하는 하트 구매<br>결제하여 보석 구매(외부 서버에 요청하고 내부 서버에 기록을 남긴다.) 
 사용자 간(소셜) |  친구 찾기 및 친구 등록 <br>친구 간 순위 확인 <br>하트 선물(일정 시간의 주기가 존재) 


테이블 이름 | 역할                            | 필요한 정보
-----------|--------------------------------|------------
usercore   | 사용자의 기초 정보 기록          | ID, 보석, 코인, 하트, 점수 
userupgrade| 캐릭터의 업그레이드 정보 기록     | 공격력, 레벨, 방어력 레벨 
useritem   | 사용자가 보유한 아이템 정보 기록  | 아이템 종류, 아이템 개수 
usermessage| 사용자 간 주고 받은 메시지 기록   | 메시지 종류, 수신여부 
friendlist | 사용자의 친구 정보 기록          | 상태 id, 상태, 하트 전송 시간 
purchaseresult | 사용자의 결제 정보 기록      | ID, 성공 여부, 상품 정보, 완료 시간 


**usercore 테이블**

Name     | Type    | Length | Default    | Attributes | Index  | Auto Increase 
---------|---------|--------|------------|------------|--------|--------------
no       |  INT    | -      | none       | UNSIGNED   | PRIMARY| 체크
id       | VARCHAR | 14     | none       |    -       |    -   | -
gems     |MEDIUMINT| -      |As defined:0| UNSIGNED   |    -   | -
coins    |  INT    | -      |As defined:0| UNSIGNED   |    -   | -
hearts   |SMALLINT | -      |As defined:0| UNSIGNED   |    -   | -
highScore|  INT    | -      |As defined:0| UNSIGNED   |    -   | -
loginTime|  INT    | 10     |As defined:0| UNSIGNED   |    -   | -


**useritem 테이블**

Name     | Type    | Length | Default    | Attributes | Index  | Auto Increase 
---------|---------|--------|------------|------------|--------|--------------
no       |  INT    | -      | none       | UNSIGNED   | PRIMARY| 체크
user     |  INT    | -      |As defined:0| UNSIGNED   |    -   | -
ItemNo   |SMALLINT | -      |As defined:0| UNSIGNED   |    -   | -
amount   |SMALLINT | -      |As defined:0| UNSIGNED   |    -   | -
use      |BOLLEAN  | -      |As defined:0| UNSIGNED   |    -   | -



**friendlist 테이블**

Name     | Type    | Length | Default    | Attributes | Index  | Auto Increase 
---------|---------|--------|------------|------------|--------|--------------
no       |  INT    | -      | none       | UNSIGNED   | PRIMARY| 체크
user     |  INT    | -      |As defined:0| UNSIGNED   |    -   | -
friend   |  INT    | -      |As defined:0| UNSIGNED   |    -   | -
state    | TINYINT | -      |As defined:0| UNSIGNED   |    -   | -
sendTime |  INT    | 10     |As defined:0| UNSIGNED   |    -   | -


**usermessage 테이블**

Name     | Type    | Length | Default    | Attributes | Index  | Auto Increase 
---------|---------|--------|------------|------------|--------|--------------
no       |  INT    | -      | none       | UNSIGNED   | PRIMARY| 체크
user     |  INT    | -      |As defined:0| UNSIGNED   |    -   | -
sender   |  VARCHAR| 14     |     -      |     -      |    -   | -
senderNo |  INT    | -      |As defined:0| UNSIGNED   |    -   | -
msgType  | TINYINT | -      |As defined:0| UNSIGNED   |    -   | -
amount   | SMALLINT| -      |As defined:0| UNSIGNED   |    -   | -
time     |  INT    | 10     |As defined:0| UNSIGNED   |    -   | -


**price 테이블**

Name     | Type    | Length | Default    | Attributes | Index  | Auto Increase 
---------|---------|--------|------------|------------|--------|--------------
no       |  INT    | -      | none       | UNSIGNED   | PRIMARY| 체크
codeNo   |SMALLINT | -      |As defined:0| UNSIGNED   |    -   | -
price    |MEDIUMINT| -      |As defined:0| UNSIGNED   |    -   | -
bouns    |SMALLINT | -      |As defined:0| UNSIGNED   |    -   | -
amount   |MEDIUMINT| -      |As defined:0| UNSIGNED   |    -   | -


price 테이블의 codeNo는 1번부터 원하는 번호를 할당하면 되지만 확장을 고려해 넘버링하는 것이 좋습니다.

여기서는 크게 두가지 유형이 있습니다.

첫째는 제품 가격이 고정된 상품입니다. 이 경우 101~999 까지 할당해 제품을 등록합니다. 

세부 규칙으로는 백단위로 상품을 나누고 나머지 단위로 제품을 나누는 것입니다. 예를 들어 현금으로 보석을 구매하는 상품은 codeNo가 100번째 입니다.

둘째는 제품은 1개이나 구매할 때마다 가격이 변동되는 상품입니다. 

여기서는 업그레이드가 이 경우에 해당합니다. 레벨마다 가격이 존재하므로 많은 데이터를 요구합니다. 

codeNo는 1001~9999까지 할당해 제품을 등록합니다. 예를 들어 공격력 업그레이드라면 1000번대를 할당합니다. 

그리고 뒤에 자리 숫자를 이용해 레벨을 나타냅니다. 공격력 1레벨 업그레이드는 1001이 되는 겁니다.

* BIGINT - 8 bytes
* INT - 4 bytes
* MEDIUMINT - 3 bytes
* SMALLINT - 2 bytes
* TINYINT - 1 bytes

bollean -> unsinged 하면 에러남.. unsigned 해제


usrcore 에 facebook 컬럼을 하나 더 추가해서 작업.. Use BIGINT(64) to store Facebook User IDs in MySQL Database Table.(https://developers.facebook.com/blog/post/45/)

액세스 토큰은 varchar (255)로..

c# 에서는 long or int64로 받으면 됨.

## 게임에 적용될 페이스북 흐름 ##

FB.Init (페이스북 초기화)

FB.Login (페이스북 로그인) -> 페이스북에 로그인되면 페이스북 ID와 사용자 이름 요청.

친구정보 받기.

FB API 함수를 사용하여 입맛데로 컨트롤?

FB API Example

```
FB.API("me?fields=id,name", Facebook.HttpMethod.GET, GetIDComplete);

// 페이스북 아이디와 사용자 이름 결과 처리.
void GetIDComplete(FBResult result)
{
    if(FB.IsLoggedIn 
        && PlayerPrefs.GetInt("UserKeyNo") == 0)
    {
        Dictionary<string, object> userInfo = 
            Facebook.MiniJSON.Json.Deserialize(result.Text) 
                as Dictionary<string, object>;
    
        string facebookName = System.Convert.ToString( userInfo["name"]);
        string facebookID = System.Convert.ToString( userInfo["id"]);
    }
}
```

```
string requestImgQuery = string.Format("/{0}/picture?width=80&height=80", fData.facebook);
FB.API(requestImgQuery, Facebook.HttpMethod.GET, ProfileImgCallBack);
// 프로필 이미지를 로딩한 후 실행된다.
void ProfileImgCallBack(FBResult result)
{
    if (result.Error != null){
        Debug.Log(result.Error);
        // 에러가 난 경우 기본 이미지로 나타나도록 함.
        userImg.mainTexture = GameData.Instance.lobbyGM.normalUserImg;
        return;
    }
    // 받아온 프로필 이미지 적용.
    userImg.mainTexture = (Texture2D)result.Texture;
}
```

## 페이스북 액세스 토큰 ##

[페이스북 액세스 토큰](https://developers.facebook.com/docs/facebook-login/access-tokens/)

[Ouath와 춤을](http://d2.naver.com/helloworld/24942)

[How-To: Handle expired access tokens](https://developers.facebook.com/blog/post/2011/05/13/how-to--handle-expired-access-tokens/) 액세스 토큰 갱신

[nodeJS로 웹/모바일 소셜+로컬 로그인 구현하기 ](https://www.gitbook.com/book/g6ling/nodejs-login-perfect-practice/details)

토큰 값은 60일간 유효


## 페이스북 관련 질문 글 ##


### 질문 1 ###

크루쉐이더 퀘스트 facebook 친구부분입니다. 

맨왼쪽에보면 캐릭터 얼굴과 레벨 직업이 나타나는데... 이부분은 자체개발서버랑 연동해야 가능한지요?

제질문의 요점은 facebook 친구 리스트불러올때 제가원하는 값을넣어( 예를들어 게임 nick name) 그값을 불러올수 있는지 궁금한것입니다. 

고로 자체서버를 사용하지않고 facebook api로만 저러한 작업이 가능한지 궁금합니다.. 이문제로 몇일째 ㄱ끙끙 앓고 있네요..

페이스북 페이지를 만들어 두면 해당 게임을 인스톨 한 유져만 따로 불러 오는건 Facebook API만으로 가능 합니다.

(게임 인스톨 한 유져, 페북친구지만 게임은 인스톨 하지 않은 유져 다 가져올수있습니다)

다만 게임내 친구로 연동을 하려면 당연히 별도의 작업이 필요 하게 됩니다 

페북 API 만으로는 안될꺼에요. 저희도 예전 게임에서 자체 서버연동을 했어요.

친구할때 페북 친구 목록을 읽어서 통으로 Add요청을 보내면,, 

받아온 페북 친구 키중에서, 계정 테이블에도 있는 해당 키가 있다면 친구 테이블에 넣고, 넣을 때 페북 여부를 on 시키는 방식으로 구현했습니다.

계정 테이블
- 키
- 페북키
- 닉네임

친구테이블
- 내키
- 친구키
- 페북여부

저희도 혹시나 페북 프로필쪽에 extraData를 넣을 수 있지 않을까하고 찾아봤는데,, 없더라구요 
ㄴ 네 저도 페북 프로필쪽에 다른 데이타를 넣을수있나 보는데 도저히 안나오네요.... 
ㄴ kstyner  네 말씀대로 인스톨한 유저와 하지않은 유저는 따로 분리되도록 짜놨어요 ㅜㅜ 문제는 저 그림처럼 매칭을 시켜줄려고하는데,,, 결국 자체 서버로 db에 넣어서 매칭을 해주는법밖에없느 ㄴ지 ...

### 질문 2 ###

안녕하세요.

안드로이드/아이폰 게임을 개발하고 있는데요.

페이스북 계정으로 로그인이 가능하도록 하려고 합니다.

그냥 API만 연동해서 친구정보 가져오는것만 하는게 아니라

로그인 인증 자체를 페이스북을 통해서 하려고 합니다.

제가 생각한 방식이 이런건데 이게 맞는건지 경험 있으신분들은 조언 부탁드립니다.

1. 클라이언트 -> 게임서버로 TCP 접속 시도 -> 소켓 연결 유지.
2. 클라이언트 -> 페이스북으로 ID/PW 인증 -> 인증 성공시 callback url -> 별도 구축한 웹서버로 전달.
3. 웹서버에서 -> 게임서버로 인증 데이터 전달.
4. 게임서버 -> 클라이언트의 게임 로그인 처리.



1. 클라이언트 -> 페이스북으로 ID/PW 인증
2. 클라이언트 -> 페이스북으로 인증을 통해 받은 user id, access token 등을 게임서버로 전달
3. 게임서버 -> 클라이언트의 게임 로그인 처리

정도면 될것같은데요?
	
궁금한게 있는데요.
2, 3번 과정에서
해당 클라이언트가 보낸 access token이 정상적인지 유무를 체크해야 하는데
그렇다면 저희 게임서버에서 페이스북API등을 써서 validate한지를 알 수 있는 방법이 있는거겠지요?
(페북 API등을 써서 하는거겠죠?)^^;
  
저희는 그런식으로 했었는데 페북 문서를 보니 로그인 시점부터 게임서버로 로그인 요청을 날리는걸 게임서버에서 다시 페북으로 리다이렉트를 하는 식으로 처리하라고 올라오는군요.

http://developers.facebook.com/docs/concepts/login/login-architecture/
  

구글님에게 redirect_uri 으로 검색하시면 많은 정보들을 받으실 수 있으세요~


그런데요,

게임 초반에 로그인 인증 절차를 강제하시면,

해당 인증 절차를 거쳐야만 게임을 할 수 있다는건데요..

주요 타겟 유저층을 일부러 초반부터 좁힐 필요가 있을까요?


저희도 초반 인증절차 거치다가,

인증 없이 게임에 접속하고 기본플레이가 무리 없이 진행되게 업데이트 하고서는 (사실 내부적으로 유저들 모르게 인증을 시도합니다만,)

업데이트 이전보다 유저가 훨씬 많이 들어왔네요.

나중에 필요에 의해서 결제나, 친구연동이나

기타 등등의 이유로 인증을 요구하면 될 텐데요.. ^^;


지나가는 나그네의 주절 거림이였습니다.
			
/훗쇼님
아, 저 사이트 참고하면 도움이 많이 되겠네요. 감사합니다.^^
스택 오버플로우 뒤져보니까 관련 정보도 좀 나오네요.
삽질 해봐야 겠습니다.

/한월
네, 저도 그렇게 생각하는데 다른한편에서는 강제하자는 얘기가 강하게 나와서요.
그냥 필요에 의해서 연동하는게 좋은데.. ㅠ_ㅠ 저 자신조차도 귀찮아서 페북을 잘 안쓰니^^
계속 설득 해봐야 겠습니다.


### 질문 3 ###


제가 생각하는 방법이 맞는지 궁금해서 작업전 미리 질문드립니다.(도구 : prime31)
1. 게임을 끝내고 점수가 나옴.
2.페이스북 id와 점수를 서버(mysql)에 저장
3.prime31을 이용해 친구목록(페이스북 id + 사진)을 가져옴
4.가져온 친구목록을 이용해 서버에서 아이디를 검색하여 아이디가 있을시 서버에 저장되어있던 친구의 점수를 뿌려줌.
5.가져온 친구목록을 이용해 서버에서 아이디를 검색하여 아이디가 없을시 친구에게 게임 같이하기.담벼락에 남길수있는버튼만들어줌

위방법을 생각하고있는데 맞나요? 그리고 사진을 가져왔을때 저장을 해야되는건지 아니면 다른방법이있는건지 궁금합니다.
의견부탁드리겠습니다.


저도 지금 구현을 하고 있는데요, 일주일에 한번씩 갱신하는게 아니라면 post/get score로 해당 아이디에 점수를 저장하고 가져오는 기능이 있습니다. (facebook api 지원)
1. 로그인
2. access token 가져오기
3. 친구 목록 불러오기 (내 아이디는 별도로 graph api 이용. 예제에 있습니다.)

이미지는 친구 목록에 따로 없고요, 친구 아이디를 통해 별도로 이미지를 호출해서 뿌려주셔야 할 것 같아요.
/친구 id/picture로 호출하면 됩니다. (WWW + LoadImageIntoTexture로 texture 읽어서 입혀주시면 됩니다.)
texture는 저장하셔도 되고 계속 불러오셔도 될 것 같네요.
Texture2D로 메모리에 유지하면 되는데 친구가 많으면 혹시 모르겠네요. ㅎ
참고로 gif는 유니티에서 지원을 하지 않고요. LoadImageIntoTexture로 gif를 불러오면 default 이미지가 뜹니다. (못생긴 물음표 이미지)
이 물음표 이미지는 8x8 크기를 가지고 있으니 texture size가 8x8이면 별도 이미지로 대체하는 방법을 사용중입니다. (이 이미지는 파일이 아니라 프로그램 생성 방식이라 완벽하게 처리하기는 쉽지 않아보여 편법으로 사용중입니다.)
단, 페이스북은 default size가 50x50 그리고 가로 100, 가로 200 이렇게 세 가지 사이즈를 지원합니다. (api 참조)

관련한 링크 공유드립니다.

http://www.paladinstudios.com/2011/11/15/facebook-and-unity-tutorial-part-one/

http://www.paladinstudios.com/2011/11/24/facebook-and-unity-tutorial-part-two/

http://www.paladinstudios.com/2011/12/06/facebook-and-unity-tutorial-part-three/

도움이 되시길!!

### 질문 4 ###

트위터나 페이스북 처럼 sns계정을 통한 제3의 웹 사이트 로그인을 구현 하려고 합니다. 트위터나 페이스북은 한번 access_token을 받으면 DB에 저장해서 API사용시 계속 재사용 할수 있는것으로 알고 있는데요..

카카오처럼 계속 갱신을 해줘야 한다면 자체 DB에 저장 하는게 의미 없을것 같다는 생각이 드는데요.. 한번 방문후 몇달있다 다시 방문할수도 있으니까요..

그러면 로그인 할때마다 DB의 access_token을 갱신해 주면 되는건가요?



그리고 DB에 저장은 1개월간 유효한 refresh_token 을 저장해두셨다가 access_token 을 갱신하는 방법을 취하면 될 듯 합니다. 

access_token은 DB보다는 보다는 좀 더 임시적인 저장소(클라이언트단에 cookie 또는 서버의 캐시 등)에 저장해두고 사용하는게 어떤가 싶네요.

방문이 없다가 몇달 있다 다시 방문하는 경우는 사용자에게 다시 동의를 받는게 보안적으로 적절하지 않나 싶네요.

### 질문 5 ###

안녕하세요.

안드로이드 앱에서 페이스북에 퍼미션(email)을 요청하여 이메일을 받아오는것까지 성공하였는데요,,

안드로이드 앱에서 웹서버랑 연동하여 로그인 기능을 완성하려고 하거든요.

첨엔 이메일을 받아와서 그걸로 로그인(가입되어있지 않으면 가입부터)하는거라고 단순하게 생각했는데,,

페이스북에서 받어온 어떠한 코드를 웹서버로 보내면 웹서버에서 그 코드를 읽어서 앱키?(시크릿키?)랑 같이

페이스북 서버에 보내어 리턴을 받는다고 하는군요..

그런데 어떠한 코드를 웹서버에 보내야 하는지 모르겠습니다.

참고로 AccessToken은 보안상 해커가 중간에 가로채기를 할수있기 때문에 안좋다고 하는데요.

AccessToken이 완제품이라면 그 AccessToken을 받아내기 이전의 반제품 상태인 code를 달라고 하시네요.

그런데 그 코드 이름이 뭔지도 모르겠고... 검색능력이 부족한 탓인지 찾아봐도 안나옵니다...

어떤 코드를 보내야하나요?

감사합니다.


페이스북 sdk 를 사용하고 계신다면 AccessToken 관리는 잊어버리셔도 됩니다.

보통 기본적인 flow 는 다음과 같습니다.

user가 facebook login 버튼 누름 -> facebook login window 뜸 (웹뷰, 네이티브)

-> user가 facebook login 완료 -> 앱에 access token 과 facebook user member key 반환

-> 자체 서버에 member key 와 access token 정보 담아서 호출

-> 자체 서버에서 facebook.com/me?access_token={access_token}  이었던가

암튼 access token 으로 user 반환받는 api 가 있습니다. 그걸로 return 된 user 와

앱에서 받은 member key 일치하는지 확인

-> 회원가입 안되있으면 가입시키고 DB에 facebook_id 칼럼에 member key 저장

-> 회원가입 되어있으면 로그인 flow 진행

멤버키랑 액세스토큰을 서버로 보낸다는 뜻인가요? 보안상 안좋다며 code를 달라고 하셔서요...ㅠㅠ 그리고 , 멤버키는 어떤걸 말씀하시는지...userId말씀 하시는 건가요? 정확한 명칭을 알려주심 감사하겠습니당.

제가 알기로는 OAuth 에서 code 는 Facebook api 를 사용하는 client 앱 권한을 인증하는 용도로 사용하는 것으로 알고 있는데요 code 로 facebook 사용자를 확인할 수 있다구요? member key 는 userId 가 맞습니다.

보안상 안좋으면 https 를 사용하면 될것이며 facebook api 를 통해서도 access token을 항상 주고받고 하는데 어떤 보안을 말하는지 모르겠네요 ㅋ

문서 찾아보니 AccessToken으로 주고받고 하는게 맞다고 하시네요... 감사합니닷ㅎㅎ
