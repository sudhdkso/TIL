## **가장 큰 수**


### ***problem***
0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

### **제한사항**
- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    static boolean[] visited;
    public String solution(int[] numbers) {
        String answer = "";
        int n = numbers.length;
        String[] arr = new String[n];
        for(int i=0;i<n;i++){
            arr[i] = String.valueOf(numbers[i]);
        }
        
        Arrays.sort(arr, (o1,o2) -> {
            int n1 = Integer.parseInt(o1+o2);
            int n2 = Integer.parseInt(o2+o1);
            return Integer.compare(n2,n1);
        });
        
        answer = String.join("",arr);
        
        if(answer.charAt(0) == '0'){
            return "0";
        }
        return answer;
    }
    
}
```
### **문제 풀이** 
- int배열인 numbers를 String배열로 변환한 후, String배열을 정렬해준다.
    - 이때 정렬은 o1과 o2중 더해서 더 큰값이 나오는 것을 기준으로 내림차순으로 정렬한다.
- 그리고 String배열인 arr에 있는 값을 모두 join()을 통해 String타입으로 합친 후 answer에 넣는다.
- answer의 첫번째 값이 0이면 가장 큰값이 0이라는 의미로 0이 몇개든 답은 `"0"`이어야 한다.


### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42746?language=java