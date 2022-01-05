
## ***2022.01.05 TIL***
<hr>

### ■ 카카오 소셜로그인

1. 앱 키 발행
    - 카카오소셜로그인은 `KaKaoDeveloper`에 가입한 후 앱을 만들어 `앱 키`를 발행받아야지 사용 가능

2. 동의항목 설정
    - 로그인을 사용할 경우 카카오에서 받아올 User정보를 동의항목에서 설정 가능
    <center>
    <img  width=500px src= "images\kakaoAgreement.png">
    </center>

3. Redirect URI 등록
    - Redirect URI는 Oauth 2.0을 기반으로 동작하는 카카오 로그인의 핵심 요소
    -  카카오 서버는 Redirect URI로 서비스에서 필요한 로그인 인증 정보를 보냄
    - 서비스는 Redirect URI로 받은 로그인 인증 정보를 처리해 다음 단계 요청을 보냄
    - Redirect URI가 앱 설정에 등록되어 있지 않으면 카카오 로그인 시 에러가 발생
    <center>
    <img  width=500px src= "images\kakaoRedirectURI.png">
    </center>
    


### ■ 소셜로그인 연결과정에서 발생한 오류
1. hibernate 오류

    ```java 
    Error executing DDL via JDBC Statement
    ```
    이 오류는 build.gradle에 hibernate의 버전을 설정하는 아래 코드를 추가하여 해결

    ```java
    ext['hibernate.version'] = '5.2.10.Final'
    ```

2. application.yml에러

    application.yml에서 아래 오류가 발생
    ```java
    client id must not be empty
    ```

    ```java
    spring:     
         security:
             oauth2:
                client:
                    registration:
                         google:
                            client-id: 
                            client-secret: 
                        facebook:
                            client-id: 
                            client-secret: 
    ```

    client-id를 빈채로 두어서 발생한 오류
    아래 처럼 임의의 "test"로 client-id를 대신함

    ```java
    spring:     
         security:
             oauth2:
                client:
                    registration:
                         google:
                            client-id: "test"
                            client-secret: "test"
                        facebook:
                            client-id: "test"
                            client-secret: "test"
    ```

