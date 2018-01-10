# Python으로 비트코인 챗봇 만들기
###### 초보자를 위한 AWS 뿌시기 세미나 1일차 강의 자료

## AWS Free Tier 가입하기
![스크린샷, 2018-01-10 20-23-00](https://i.imgur.com/MTm3XV4.png)
* https://aws.amazon.com/free/
* 무료 계정 생성

## C9
* https://aws.amazon.com/ko/cloud9/
* 싱가폴 리전 선택
* AWS Cloud9 시작하기 버튼 -> 클릭
* Create Environment 버튼 -> 클릭
* Create a new instance for environment (EC2 설정) -> Instance Type은 t2.micro설정
* Cost-saving setting은 4시간 후 설정
* Create! 하면 조금 시간이 걸립니다...
```bash
$ git clone https://github.com/Exubient/AUSG_KakaoBot
```

## AWS Elastic IP (고정아이피 할당)
* [Dashboard](https://aws.amazon.com/ko/)
* 내계정 -> AWS Management Console-> EC2
* NETWORK & SECURITY탭 -> Elastic Ips -> Allocate new address -> Allocates -> 작업-> 주소연결

## AWS Inbound 열기
* [Dashboard](https://aws.amazon.com/ko/)
* 콘솔에 접근  -> EC2 -> NETWORK & SECURITY탭
* Security Groups
* Inbound -> Edit  -> Add Rules Button -> custom -> 8000, 8080 열기 -> save

### 참고 명령어

* Bash에서 상위 디렉토리 이동
```bash
$ cd AUSG_KakaoBot
```
* Bash에서 하위 디렉토리 이동
```bash
$ cd ..
```

## Django
* requirement 설정
```bash
$ cd AUSG_KakaoBot
$ sudo pip install -r requirements
```

* kakao/kakao/settings.py
```
ALLOWED_HOSTS = ['*']
INSTALLED_APPS = ['alpaca'] #추가
```

* kakao/urls.py
```
from alpaca import views

url(r'^keyboard/', views.keyboard),
url(r'^message', views.answer),
```

* kakao/alpaca/views.py
* 카톡 플러스친구 API TEST Function
```
def keyboard(request):
    return JsonResponse({
        'type' : 'buttons',
        'buttons' : ['Coinone', 'Bithumb', 'Bitfinex']
    })

```

* 응답을 위한 Main Function
```
ret={}
@csrf_exempt #보안 Middleware
def answer(request):
    pass

    #첫번째로 보일 키보드
    #두번쨰로 보일 키보드


```

* AUSG_KakaoBot/coin.py
```
def fetch_cryptocompare():
	pass

	#가격정보를 원하는 코인 종류/ Set
	#정보를 받아올 시장 / Dictionary
	#날짜를 저장
	#_dict에 저장된 정보를 coin.csv파일에 저장.
	print("Success")

def scheduler():
	pass

	#fetch_cryptocompare() 매분돌리기

scheduler()

```
* runserver kakao/manage.py
```bash
$ python manage.py migrate
$ python coin.py // 터미널창 추가해서 돌려놓기
$ python manage.py runserver 0:8000
```

#### 만약 SyntaxError: Non-ASCII character '\xec' in file 에러가 난다면?
파이썬 코드 맨 위
```
# -*- coding: utf-8 -*-
```

## KaKao
* [플러스친구 관리자 센터](https://center-pf.kakao.com/signup)
* 가입 (핸드폰 인증 필요)
* 새 플러스 친구
* 관리 -> 공개설정
* 스마트채팅 -> API형 설정하기 -> http://엘라스틱 탄력적ip주소:8000 -> Api Test
* 알림받을 전화번호 -> 자기 전화번호 입력 -> 인증 -> 시작!
* 휴대폰으로 플러스친구 검색 -> 테스트 ㅎㅎ


## 파괴하기
* EC2 파괴
* 카톡 플러스친구 파괴
