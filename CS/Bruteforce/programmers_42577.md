## **전화번호 목록**


### ***problem***
전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

- 구조대 : 119
- 박준영 : 97 674 223
- 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

### **제한사항**
- phone_book의 길이는 1 이상 1,000,000 이하입니다.
    - 각 전화번호의 길이는 1 이상 20 이하입니다.
    - 같은 전화번호가 중복해서 들어있지 않습니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    public boolean solution(String[] phone_book) {
        boolean answer = true;
        Arrays.sort(phone_book);
        int len = phone_book.length;
        
        for(int i=0;i<phone_book.length-1;i++){
            String s = phone_book[i];
            if(phone_book[i+1].startsWith(s)){
                return false;
            }
        }
        
        return answer;
    }
}
```
### **문제 풀이** 
-   
    String배열을 정렬하면 숫자는 오름차순이 아닌 사전 순으로 정렬이 된다.

    예를 들어 `["119", "97674223", "1195524421"]`라면 1로 시작하면서 길이가 제일 짧은 "119"가 오고 그다음 "97674223"이 아닌 "1195524421"이 온다. 따라서 `["119", "1195524421", "97674223"]`

- 이렇게 문자열을 먼저 사전순으로 정렬하고 i번째 문자열이 i+1번째 문자열의 접두사인지 확인하기 위해 String의 startsWith함수를 사용하였다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42577?language=java