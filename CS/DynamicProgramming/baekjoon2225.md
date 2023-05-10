## **합분해**


### ***problem***
0부터 N까지의 정수 K개를 더해서 그 합이 N이 되는 경우의 수를 구하는 프로그램을 작성하시오.

덧셈의 순서가 바뀐 경우는 다른 경우로 센다(1+2와 2+1은 서로 다른 경우). 또한 한 개의 수를 여러 번 쓸 수도 있다.

#### **Input**
첫째 줄에 두 정수 N(1 ≤ N ≤ 200), K(1 ≤ K ≤ 200)가 주어진다.

#### **Output**
첫째 줄에 답을 1,000,000,000으로 나눈 나머지를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int n = Integer.parseInt(st.nextToken());
        int k = Integer.parseInt(st.nextToken());

        long[][] dp = new long[201][201];

        Arrays.fill(dp[1],1);

        for(int i=2;i<=k;i++){
            for(int j=0;j<=n;j++){
                for(int a=0;a<=j;a++){
                    dp[i][j]= (dp[i][j]+dp[i-1][j-a])%1000000000;
                }
            }
        }


        bw.write((dp[k][n]% 1000000000)+"\n");
        bw.flush();
    }

}
```
- dp[k][n]은 숫자 k개로 n개를 만들 수 있는 경우의 수이다.
- dp[k][n]은 dp[k-1][0] ~ dp[k-1][n]까지를 다 더한 값이다.


### 출처
https://www.acmicpc.net/problem/2225