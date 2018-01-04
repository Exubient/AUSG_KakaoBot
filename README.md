## AWS Free Tier 가입하기
* https://aws.amazon.com/free/
* 무료 계정 생성

## C9
* https://aws.amazon.com/ko/cloud9/
* 싱가폴 리전 
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
* 내계정 -> AWS Management Console
* NETWORK & SECURITY탭 -> Elastic Ips -> Allocate new address -> Allocates -> 이미 생성한 인스턴스에 붙임

## AWS Inbound 열기
* [Dashboard](https://aws.amazon.com/ko/)
* 콘솔에 접근
* Security Groups
* Inbound -> Edit  -> Add Rules Button -> custom -> 8000 포트만 열고 -> save

## 기본 Bash명령어
* pwd 현재 디렉토리
* cd 파일/.. 디렉토리 이동
* vim 파일 편집
* i를 누르면 편집기능
* :w 저장 :q종료 :wq 저장+종료

## Django
* requirement 설정
```bash
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
@csrf_exempt
def answer(request):
    json_str = ((request.body).decode('utf-8'))
    received_json_data = json.loads(json_str)
    returnButton = received_json_data['content']
    with open(r'coin.csv', 'r') as f:
        reader = csv.reader(f)
        for row in reader:
            info = row[0].split("-")
            ret = info[0]
            if info[0] == returnButton:
                return JsonResponse({
                    'message': {
                        'text': "you have selected " + returnButton
                     },
                    'keyboard': {
                        'type': 'buttons',
                        'buttons': ['BTC', 'ETH', 'XRP']
                    }

                })
            if info[1] == returnButton and ret == info[0]: 
                ret = ""
                return JsonResponse({
                    'message': {
                        'text': row[1]
                     },
                    'keyboard':{
                        'type': 'buttons',
                        'buttons' : ['Coinone', 'Bithumb', 'Bitfinex']

                        }
                })
    
```

* kakao/coin.py
```
import requests
import json
import csv
import os
import datetime
from apscheduler.schedulers.blocking import BlockingScheduler

def fetch_cryptocompare():
	coins = {'BTC','ETH', 'XRP'}
	exchanges = {'Coinone':'KRW', 'Bithumb':'KRW', 'Bitfinex':'USD'}
	_dict = {}
	cur = datetime.datetime.now()
	timestamp = cur.strftime('%Y-%m-%d-%H:%M')
	_dict["Time"] = timestamp

	for market,currency in exchanges.items():
		url = 'https://min-api.cryptocompare.com/data/pricemulti?fsyms=%s&tsyms=%s&e=%s' % (','.join(coins), currency, market)
		response = requests.get(url)
		data = response.json()

		for coin in coins:
			if response.status_code == requests.codes.ok:
				_dict[market+'-'+coin+'-'+currency] = data[coin][currency] #data['ETH']['KRW']
			else:
				_dict[market+'-'+coin+'-'+currency] = -1


	with open(r'coin.csv', 'w') as f:
		writer = csv.writer(f)
		for key, value in _dict.items():
			writer.writerow([key, value])
	print("Success")

def scheduler():
    sched = BlockingScheduler()
    sched.configure(timezone='Asia/Seoul')
    sched.add_job(fetch_cryptocompare, 'interval', minutes=1)
    sched.start()

scheduler()
```
* runserver kakao/manage.py
```bash
$ python manage.py migrate
$ python manage.py runserver 0:8000
```
## KaKao
* [플러스친구 관리자 센터](https://center-pf.kakao.com/signup)
* 가입 (핸드폰 인증 필요)
* 새 플러스 친구
* 관리 -> 공개설정
* 스마트채팅 -> API형 설정하기 -> http://엘라스틱ip주소:8000/ -> Api Test
* 알림받을 전화번호 -> 자기 전화번호 입력 -> 인증 -> 시작!
* 휴대폰으로 플러스친구 검색 -> 테스트 ㅎㅎ 

## 파괴하기
* EC2 파괴
* 카톡 플러스친구 파괴

