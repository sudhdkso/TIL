## **동전 1**


### ***problem***
n가지 종류의 동전이 있다. 각각의 동전이 나타내는 가치는 다르다. 이 동전을 적당히 사용해서, 그 가치의 합이 k원이 되도록 하고 싶다. 그 경우의 수를 구하시오. 각각의 동전은 몇 개라도 사용할 수 있다.

사용한 동전의 구성이 같은데, 순서만 다른 것은 같은 경우이다.

#### **Input**
첫째 줄에 n, k가 주어진다. (1 ≤ n ≤ 100, 1 ≤ k ≤ 10,000) 다음 n개의 줄에는 각각의 동전의 가치가 주어진다. 동전의 가치는 100,000보다 작거나 같은 자연수이다.

#### **Output**
첫째 줄에 경우의 수를 출력한다. 경우의 수는 231보다 작다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int n = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());

        long[] dp = new long[10001];
        int[] coins = new int[n+1];
        for(int i=1;i<=n;i++){
            coins[i] = Integer.parseInt(br.readLine());
        }

        Arrays.sort(coins);
        dp[0] = 1;
        for(int i=1;i<=n;i++){
           for(int j=coins[i];j<=k;j++){
                dp[j] += dp[j-coins[i]];
           }
        }

        bw.write(dp[k]+"\n");
        bw.flush();
    }

}
```
- dp[i]는 주어진 동전들로 i원을 만들 수 있는 경우의 수이다.
- 주어진 동전들이 [1,2,5]라면 
  - dp[j]는 각 j-1원에 1원짜리 동전이 하나 추가되는 경우의 수, j-2원에 2원 짜리 동전이 하나 추가되는 경우의 수, j-5원에 5원 짜리 동전이 하나 추가되는 경우의 수 각각을 더한 값이 주어진 동전들로 j원을 만들 수 있는 경우의 수이다.
  - dp[j] = dp[j-1] + dp[j-2] + dp[j-5]   

### 출처
https://www.acmicpc.net/problem/2294
