## **소수찾기**


#### ***problem***
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
        int length = numbers.length();
        String[] slist = new String[length];
        for(int i=0;i<length;i++){
            slist[i] = numbers.substring(i,i+1);
        }
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
-  

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/42839?language=java