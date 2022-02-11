# ***2022.01.07 TIL,02.08 ~02.09 TIL***
## ***01. what is the Internet?***

>Internet= inter- + net(work)

- `network of network`
- 다양한 네트워들의 연결



### 인터넷의 구성요소
#### - 네트워크가 필요한 이유 → 정보를 주고받기 위해서

#### ■ 하드웨어 구성요소    
1. end hosts
- 네트워크 끝단에 달려있다.
- 정보를 요청/제공하는 주체
- ex) 스마트폰,랩탑 등

2. links
- 유선: copper, fiber
- 무선: radio, stellite

3. interconnection devices
- `end host`를 이어주는 중간 장비
- ex) router,switch,repeater
    - router & switch  →  
    end host에서 다른 호스트로 보내는 정보를 어떠한 방식으로 보내는지

    - repeater → 확성기,
    데이터가 전달되다보면 신호가 약해져서 수신을 받아도 무슨 데이터이지 모르는 상황을 막기위해서
    중간에 repeater를 설치하여 repeater로 들어온 데이터를 해석하여 재반복시켜서 결론적으로 신호가 강해짐 

    
#### ■ 소프트웨어 구성요소
1. operating sofware(운영체제)
2. application programs (응용프로그램)
3. protocols(약식)
    - ex) 둘 사이에 어떤식으로 데이터를 표현할 지

    - 프로토콜에 정의되어있는 것
        1. message format
            - 어순, 메세지 형식
        2. order of message
            - 메세지를 주고받는 순서
        3. actions
            - 메세지를 받았을 때 취해야하는 액션
            - ex) 데이터 쪼개기, 데이터 합치기, 에러확인 등


 ## ***02. Network Edge***


### ■ 네트워크의 구조
- **Network Edge**
    - 네트워크의 말단, 사용자가 직접 접하는 네트워크
- **Network Core**
    - `Network Edge`들을 서로 연결

### ■ **Access Network**
- end systems와 edge router를 연결하기 위한 라우터

- Residential Access Network
    - DSL
    - FTTH: 일반적인 한국의 인터넷 사용 방식(Fiber-To-the-home)

- Institutional Access Network
    - Ethernet
- wireless Access Network
    - Wireless LAN : wi-fi
    - Cellular network : 휴대전화

### ■ **Host : sends packets of data**
1. 어플리케이션이 보낸 메세지를 받음
2. **MTU** 보다 데이터가 크면 데이터를 쪼개서 packets단위로 만듦
    - MTU(Maximum transfer unit) : 전달할 수 있는 데이터의 최대 크기
3. Access Network로 packets을 전송
4. 네트워크를 통과해서 상대방의 End Host에 도달하면 패켓을 다시 모아서 하나의 큰 파일로 만듦
5. 만들어진 파일을 응용프로그램에 전달 

### ■ **Communication Link: Wired (유선)**
- Twisted pair (쌍꼬임선)
    - 2개의 절연 구리선으로 이루어짐
    - 100Mbps ~10Gbps
- Coaxial cable (동축)
    - 광역

- Optical fiber (광섬유)
    - 빠른 속도, 낮은 에러


<br>

 ##  ***03. Network Core***
 ### ■ **Network Core**
 - 라우터들의 연결이 촘촘한 상태 
 - **packet-switching**
    1. 이 라우터는 들어온 패켓의 단위를 보고
    2. 목적지에 따라 데이터의 방향을 결정
    3. 그 데이터가 그 방향으로 갈 수 있도록 스위칭해주는 역할

 ### ■ **Switching Mecahanisms**
 1. **Circuit Switching**
    - source 에서 destination까지 가는 경로, 즉 자원을 예약하여 사용하는 방식
    - 전체 데이터는 물처럼 길을 따라 이동
2. **Packet Switching**
    - 전체 데이터를 페켓단위로 쪼갬
    - 쪼갠 데이터 각각을 destination으로 전달
    - 각 패킷이 독립적으로 처리

 ### ■ **Circuit Switching**
 - 채널 할당 방법: FDM,TDM
    1. FDM: 주파수 쪼갬
        - 자원의 양을 쪼개서 서비스
        - 연속된 시간으로 데이터 전송
        - ex) 비디오
    2. TDM: 시간 쪼개기
        - 짧은 시간동안 많은 양의 데이터를 한번에 전송
        - ex) 웹 서비스

 ### ■ **Packet Switching**
 - Store-and-forwad
    1. 전체 데이터를 페켓단위로 쪼갬
    2. source에서 패켓들을 라우터로 전송
    3. 들어온 데이터를 라우터에 저장(store)
        - destination으로 전송하는 중간에 데이터가 손실날 수 있는데 이때 재전송 시켜주는 역할을 하기 위해서 중간 라우터에 저장
    4. 저장한 데이터를 다시 destination으로 전송(forwarding)

 ### ■ **Switching 비교**
- **Packet Switching**
    - 더 많은 사용자가 네트워크를 사용
    - 자원 공유
    - 함께

- **Circuit Switching**
    - 몇몇 호출에 대한 높은 서비스 품질을 보장합니다. 
    - 자원 예약
    - 경쟁