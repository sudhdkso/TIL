## **적록색약**


#### ***problem***
n개의 음이 아닌 정수들이 있습니다. 이 정수들을 순서를 바꾸지 않고 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.
```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```
사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

#### 제한사항
- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public static int count = 0;

    public static int solution(int[] numbers, int target) {
        int answer = 0;
        dfs(0, numbers, target, 0);
        answer = count;
        return answer;
    }

    public static void dfs(int index, int[] numbers, int target, int result) {
        int length = numbers.length;
        if(index >= length){
            if(result == target){
                count++;
            }
            return;
        }

        dfs(index+1, numbers,target, result+numbers[index]);
        dfs(index+1, numbers,target, result-numbers[index]);
        
    }
}
```
- dfs를 활용하여 모든 숫자를 사용하며 숫자를 더하거나 빼는 모든 경우의 수 결과값을 계산한다.
- 그 값중 target숫자와 같은 수만 전역적으로 선언해준 count를 증가시킨다.


### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/43165?language=java