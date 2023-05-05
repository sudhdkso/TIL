## **숫자 변환하기**


### ***problem***
자연수 `x`를 `y`로 변환하려고 합니다. 사용할 수 있는 연산은 다음과 같습니다.

- `x`에 `n`을 더합니다
- `x`에 2를 곱합니다.
- `x`에 3을 곱합니다.

자연수 `x`, `y`, `n`이 매개변수로 주어질 때, `x`를 `y`로 변환하기 위해 필요한 최소 연산 횟수를 return하도록 solution 함수를 완성해주세요. 이때 `x`를 `y`로 만들 수 없다면 -1을 return 해주세요.

#### **제한사항**
- 1 ≤ `x` ≤ `y` ≤ 1,000,000
- 1 ≤ `n` < `y`


### ***Solution***
``` java
import java.util.Arrays;
class Solution {
    public int solution(int x, int y, int n) {
        int answer = 0;
        int[] dp = new int[1000001];
        Arrays.fill(dp,Integer.MAX_VALUE-1);
        dp[y] = 0;
        for(int i=y;i>=0;i--){
            if(i%2 == 0){
                dp[i/2] = Math.min(dp[i]+1,dp[i/2]);     
            }
            if(i%3==0){
                dp[i/3] = Math.min(dp[i]+1,dp[i/3]); 
            }
            if(i > n){
                dp[i-n] = Math.min(dp[i]+1,dp[i-n]);
            }
           
        }
        answer = dp[x];
        if(answer == Integer.MAX_VALUE-1) return -1;
        return answer;
    }
}
```
- x를 y로 바꾸는 최소 연산을 구하는 문제이다.
- 역으로 y에서 x로 바꾸는 최소 연산 횟수를 구할 수 있도록 하였다.
- dp[i]는 y부터 i가 될 때 최소 연산 횟수를 저장한다.
- 반복문을 y부터 0까지 감소시키며 돈다.
    - 이때 dp[i/2],dp[i/3],dp[i-n]에 각각 dp[i] +1과 현재 dp값 중 더 작은 것을 저장한다.
-  반복문을 모두 수행한후 dp[x]값을 answer에 넣고 반환한다.
    - 이때 answer가 Integer.MAX_VALUE-1을 해준 값과 같으면 연산을 통해 x를 만들 수 없으므로 -1을 return 해준다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/154538