## **소수찾기**


### ***problem***
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.
### ***Solution***
``` java
import java.util.*;

class Solution {
    public static ArrayList<Integer> primeList = new ArrayList<>();
    public static int count = 0;
    public static int solution(String numbers) {
        int answer = 0;      
        go("", numbers);
        return count;
    }

    public static void go(String s1, String s2){
        int length = s2.length();
        if(!s1.equals("")){
            int num = Integer.parseInt(s1);
            if(isPrime(num) && !primeList.contains(num)){
                primeList.add(num);
                count++;
            }
        }
        for(int i=0;i<length;i++){
            go(s1+s2.charAt(i), s2.substring(0,i)+s2.substring(i+1));
        }
    }

    public static boolean isPrime(int num){
        if(num < 2) return false;
        for(int i=2;i*i<=num;i++){
            if(num%i == 0){
                return false;
            }
        }
        return true;
    }
}
```
-  isPrime은 입력받은 숫자가 소수인지 판별하는 함수이다.
- primeList는 소수들을 저장하는 ArrayList이다.
- go는 numbers로 구성할 수 있는 모든 조합을 재귀함수로 탐색한다.
    - 이때 s1은 소수인지 판별할 string, s2는 그 s1에 추가한 숫자를 제외한 문자열이다.
    - s1이 소수이면서 primeList에 없는 숫자를 primeList에 넣어주며 count를 증가시킨다.
- 모든 탐색이 끝나면 count를 return해준다. 


### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/42839?language=java