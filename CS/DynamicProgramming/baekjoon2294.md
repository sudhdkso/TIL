## **동전 2**


### ***problem***
n가지 종류의 동전이 있다. 이 동전들을 적당히 사용해서, 그 가치의 합이 k원이 되도록 하고 싶다. 그러면서 동전의 개수가 최소가 되도록 하려고 한다. 각각의 동전은 몇 개라도 사용할 수 있다.

사용한 동전의 구성이 같은데, 순서만 다른 것은 같은 경우이다.

#### **Input**
첫째 줄에 n, k가 주어진다. (1 ≤ n ≤ 100, 1 ≤ k ≤ 10,000) 다음 n개의 줄에는 각각의 동전의 가치가 주어진다. 동전의 가치는 100,000보다 작거나 같은 자연수이다. 가치가 같은 동전이 여러 번 주어질 수도 있다.

#### **Output**
첫째 줄에 사용한 동전의 최소 개수를 출력한다. 불가능한 경우에는 -1을 출력한다.

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

        int[] dp = new int[10001];
        int[] coins = new int[n+1];
        for(int i=1;i<=n;i++){
            coins[i] = Integer.parseInt(br.readLine());
        }
        Arrays.fill(dp,Integer.MAX_VALUE-1);
        dp[0] = 0;
        for(int i=1;i<=n;i++){
           for(int j=coins[i];j<=k;j++){
                dp[j] = Math.min(dp[j-coins[i]] + 1,dp[j]);
           }
        }
        if(dp[k] == Integer.MAX_VALUE-1) bw.write(-1+"\n");
        else bw.write(dp[k]+"\n");
        bw.flush();
    }
}
```
- dp를 활용한 문제이다.

### 출처
https://www.acmicpc.net/problem/2294