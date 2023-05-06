## **정수 삼각형**


### ***problem***
위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.

#### **제한사항**
- 삼각형의 높이는 1 이상 500 이하입니다.
- 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

### ***Solution***
``` java
import java.util.Arrays;

class Solution {
    public int solution(int[][] triangle) {
        int answer = 0;
        int n = triangle.length;
        int[][] dp = new int[n+1][n+1];
        dp[0][0] = triangle[0][0];
        for(int i=1;i<n;i++){
            for(int j=0;j<=i;j++){
                if(j == 0){ //가장 왼쪽일 때
                    dp[i][j] = dp[i-1][j] + triangle[i][j];
                    continue;
                }
                else if( j == i){ 
                    dp[i][j] = dp[i-1][j-1] + triangle[i][j];
                    continue;
                }
                dp[i][j] = Math.max(dp[i-1][j-1] , dp[i-1][j]) + triangle[i][j];
            }
        }

        for(int i=0;i<n;i++){
            answer = Math.max(answer,dp[n-1][i]);
        }
        return answer;
    }
}
```
- 위에서 부터 아래로 진행하는 방식의 dp문제이다.
- j == 0일때는 가장 왼쪽에 있는 값일 때 이므로, 바로 위의 dp[i-1][j] + triangle[i][j]이다.
- j==i일 때는 가장 오른쪽에 있는 값일 때 이므로, 바로 위의 dp[i-1][j-1] + triangle[i][j]이다.
- 각 층의 왼쪽과 오른쪽을 제외하고는 모두 두개의 경우의 수가 존재한다.
    - dp[i-1][j-1]과 dp[i-1][j] 두 값중 더 큰 값을 가진 dp를 triangle[i][j]에 더해서 dp[i][j]에 넣는다.

- 가장 마지막 dp[n-1][0] ~ dp[n-1][n-1] 까지 반복하면서 가장 큰 값을 찾아서 answer에 저장한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/43105