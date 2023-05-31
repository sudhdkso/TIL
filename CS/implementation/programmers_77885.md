## **2개 이하로 다른 비트**


#### ***problem***
양의 정수 `x`에 대한 함수 `f(x)`를 다음과 같이 정의합니다.

- `x`보다 크고 `x`와 비트가 1~2개 다른 수들 중에서 제일 작은 수

예를 들어, 
- `f(2) = 3` 입니다. 다음 표와 같이 2보다 큰 수들 중에서 비트가 다른 지점이 2개 이하이면서 제일 작은 수가 3이기 때문입니다.

|수	|비트	|다른 비트의 개수|
|---|---|---|
|2	|000...0010| |	
|3	|000...0011|	1|

- `f(7) = 11` 입니다. 다음 표와 같이 7보다 큰 수들 중에서 비트가 다른 지점이 2개 이하이면서 제일 작은 수가 11이기 때문입니다.

|수	|비트	|다른 비트의 개수|
|---|---|---|
|7	|000...0111| |	
|8	|000...1000|	4|
|9	|000...1001|	3|
|10 |000...1010|	3|
|11	|000...1011|	2|

정수들이 담긴 배열 `numbers`가 매개변수로 주어집니다. `numbers`의 모든 수들에 대하여 각 수의 `f` 값을 배열에 차례대로 담아 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- 1 ≤ `numbers`의 길이 ≤ 100,000
- 0 ≤ `numbers`의 모든 수 ≤ $10 ^{15} $


### ***Solution***
``` java
import java.util.*;
class Solution {
    public static long converToAnotherBit(String s){
        StringBuilder sb = new StringBuilder(s);
        int index = s.lastIndexOf("01");
        if(index < 0){
            sb.delete(0,1);
            sb.insert(0,"10");
        }
        else{
            sb.delete(index,index+2);
            sb.insert(index,"10");
        }
        long result = 0;
        int len = sb.length();
        sb.reverse();
        for(int i=0;i<len;i++){
            result += Integer.parseInt(sb.substring(i,i+1)) * Math.pow(2,i);
        }
        return result;
    }
    public long[] solution(long[] numbers) {
        ArrayList<Long> list = new ArrayList<>();
        int len = numbers.length;
        for(int i=0;i<len;i++){
            if(numbers[i]%2 == 0){
                list.add(numbers[i]+1);
            }
            else{
                String s = Long.toBinaryString(numbers[i]);  
                long result = converToAnotherBit(s);
                list.add(result);
            }
            
        }
        long[] answer = list.stream().mapToLong(Long::longValue)
    	.toArray();
        return answer;
    }
}
```
- numbers[i]가 짝수인 경우 numbers[i]+1한 값이 비트가 1~2개 다르고 가장 작은 값이다.
- numbers[i]가 홀수인 경우 numbers[i]를 이진법 문자열 `s`로 변경한다.
    - 그리고 가장 오른쪽에 있는 `01`을 `10`으로 변경한 값이 비트가 1~2개 다르고 가장 작은 값이다.
    - 가장 오른쪽에서 `01`을 찾아야하기 때문에 lastIndexOf()함수를 사용하였다.
    - `s`에 `01`을 찾을 수 없다면 가장 맨앞에 `100...0000`이거나 `111...111`모두 1이거나한 값이기 때문에 문자열 s의 맨 앞에 0을 추가하고 계산한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/77885?language=java#