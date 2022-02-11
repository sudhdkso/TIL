# ***2022.02.11 TIL***

## **도커 용어 정의**
### ***■ docker hub*** 
- 필요한 소프트웨어를 다운로드 받는 곳 (app store)

### ***■ image*** 
- 도커허브에서 다운받은 것 (program)
- 도커허브에서 이미지를 다운받는 행동 : pull

### ***■ container*** 
- 이미지를 실행하는 것 (processor)
- 이미지를 실행시켜서 컨테이너를 만드는 행동 : run
- 이미지는 여러개의 컨테이너를 가질 수 있음


### image를 pull받는 명령어

```
docker pull <image 이름> 
```

### 보유하고 있는 이미지 목록을 볼 수 있는 명령어

``` 
docker images
```

### images 실행시키는 명령어(컨테이너를 만드는 명령어)
```
 docker run [option] <image 이름> [command]
 ```
- 대괄호 생략 가능
- option: name ...

### 실행중인 컨테이너를 보고싶을때 실행시키는 명령어
```
docker ps
```

### 모든 컨테이너를 보고싶을때 실행시키는 명령어
```
docker ps -a
```

### 실행중인 컨테이너를 끄고싶을때 실행시키는 명령어
```
docker stop [option] <컨테이너 이름>
```
- 대괄호 생략 가능

### 중지시켰던 컨테이너를 켜고싶을때 실행시키는 명령어
```
docker start <컨테이너 이름>
```