## **DB Connection Pool 개념**
- DB에 요청이 들어올 때 마다 연결하고 해제하는 것은 굉장히 비효율적입니다.
- 이를 해결하기 위해서 미리 여러개의 DB Connection을 생성하고, 필요할 때마다 꺼내서 사용하는 방법이 DB Connection Pool입니다.
- DB Connection Pool에는 미리 데이터베이스와 연결이 수립된 여러개의 Connection이 존재합니다.
- 이 Connection Pool안의 Connection들은 데이터베이스에 요청이 들어올 때마다 새롭게 연결을 수립하고, 연결을 해제하지 않고 항상 연결을 열린 상태로 유지합니다.
- WAS는 데이터베이스 커넥션이 필요할 때 직접 생성하지 않고, DB Connection Pool에게 Connection을 하나 빌려와서 사용한 후 반납합니다.
- 이런 방법을 통해 데이터베이스 **연결을 열고, 닫는 비용을 절약**할 수 있습니다.

### **Hikari CP**

- HikariCP는 **스프링부트에 기본으로 내장되어 있는 JDBC 데이터베이스 커넥션 풀링 프레임워크**입니다.
 - 스프링부트는 HikariCP를 사용할 수 있는 상황에서는 항상 HikariCP를 선택한다고 한다. HikariCP를 사용할 수 없는 경우 Tomcat DBCP → Apachde Commons DBCP → Oracle UCP 순으로 선택한다고 합니다.

## **동시 접속자가 많으면?**

- 동시에 접속 할 경우 pool에서 미리 생성 된 Connection을 빌려줍니다.
- 빌려줄 Connection이 없는 경우는  Connection이 반환될 때 까지 번호순서대로 기다립니다.

### **Connection Pool을 늘리면?**

- 여기서 WAS에서 커넥션 풀을 크게 설정하면 메모리 소모가 큰 대신 많은 사용자가 대기시간이 줄어들고, 반대로 커넥션 풀을 적게 설정하면 그 만큼 대기시간이 길어집니다.

## **정리**
- DB Connection을 요청마다 매번 수립하고 해제하는 데는 비용이 소모가 되며 비효율적입니다.
- 그래서 DB Connection Pool이라는 공간에 여러개의 이미 수립된 DB Connection을 생성해놓고 필요한 상황에 빌려주고 항상 연결을 하고 있습니다.

### **[참고]**
https://d2.naver.com/helloworld/5102792
https://hudi.blog/dbcp-and-hikaricp/