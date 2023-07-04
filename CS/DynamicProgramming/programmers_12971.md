## **스티커 모으기(2)**


### ***problem***
N개의 스티커가 원형으로 연결되어 있습니다. 다음 그림은 N = 8인 경우의 예시입니다.
<p>

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d8d3a8b3-606c-4fb6-baf2-3a96cb53d70c/%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%84%8F%E1%85%A5_hb1jty.jpg" width ="300px">
</p>
스티커_hb1jty.jpg

원형으로 연결된 스티커에서 몇 장의 스티커를 뜯어내어 뜯어낸 스티커에 적힌 숫자의 합이 최대가 되도록 하고 싶습니다. 단 스티커 한 장을 뜯어내면 양쪽으로 인접해있는 스티커는 찢어져서 사용할 수 없게 됩니다.

예를 들어 위 그림에서 14가 적힌 스티커를 뜯으면 인접해있는 10, 6이 적힌 스티커는 사용할 수 없습니다. 스티커에 적힌 숫자가 배열 형태로 주어질 때, 스티커를 뜯어내어 얻을 수 있는 숫자의 합의 최댓값을 return 하는 solution 함수를 완성해 주세요. 원형의 스티커 모양을 위해 배열의 첫 번째 원소와 마지막 원소가 서로 연결되어 있다고 간주합니다.

#### **제한 사항**
- sticker는 원형으로 연결된 스티커의 각 칸에 적힌 숫자가 순서대로 들어있는 배열로, 길이(N)는 1 이상 100,000 이하입니다.
- sticker의 각 원소는 스티커의 각 칸에 적힌 숫자이며, 각 칸에 적힌 숫자는 1 이상 100 이하의 자연수입니다.
- 원형의 스티커 모양을 위해 sticker 배열의 첫 번째 원소와 마지막 원소가 서로 연결되어있다고 간주합니다.

### ***Solution***
``` java
import java.util.Arrays;
class Solution {
    public int solution(int sticker[]) {
        int answer = 0;
        int n = sticker.length;
        if(n < 3){
            return sticker[n-1];
        }
        int[][] dp = new int[2][n];
        dp[0][0] = dp[0][1] = sticker[0];
        int max = -1;
        for(int i=2;i<n-1;i++){
            dp[0][i] = Math.max(dp[0][i-1],dp[0][i-2]+sticker[i]);
            max = Math.max(dp[0][i],max);
        }

        dp[1][1] = sticker[1];
        for(int i=2;i<n;i++){
            dp[1][i] = Math.max(dp[1][i-1],dp[1][i-2]+sticker[i]);
            max = Math.max(dp[1][i],max);
        }

        return Math.max(dp[0][n-2],dp[1][n-1]);
    }
}
```

- dp로 [0,len-1] , [1,len] 두 구간으로 나눌 수 있다.
- 0번부터 스티커를 뜯으면 맨 마지막 스티커는 뜯을 수 없고, 1번부터 뜯으면 맨 마지막 스티커는 뜯을 수 있으나 첫번째 스티커인 0번 스티커는 뜯을 수 없다.
- 그래서 dp[0][i]는 0번부터 스티커를 뜯을때 값을 저장하고, dp[1][i]는 1번부터 스티커를 뜯을 때의 값을 저장한다.

- 스티커를 뜯는 경우도 2가지로 나눌 수 있다.
    1. 이전전 스티커와 현재 스티커를 뜯는다.
        - 이경우는 dp[i-2]+sticker[i]로 바꿀 수 있다.
    2. 이전스티커를 뜯었기 때문에 현재 스티커를 뜯지 않는다.
        - 이 경우는 dp[i-1]로 바꿀 수 있다.
- 2가지 경우 중 더 큰 값을 dp[i]에 넣는다.
- 그렇게 구한 각각 dp[0][n-2]과 dp[1][n-1] 중 더 큰 값을 return한다.
    - 0번째 뜯는 경우 가장 마지막 스티커는 못 뜯기 때문에 n-2를 확인한다.


### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/12971#