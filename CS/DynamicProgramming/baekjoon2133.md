## **타일 채우기**


### ***problem***
3×N 크기의 벽을 2×1, 1×2 크기의 타일로 채우는 경우의 수를 구해보자.

#### **Input**
첫째 줄에 N(1 ≤ N ≤ 30)이 주어진다.

#### **Output**
첫째 줄에 경우의 수를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());

        long[]dp = new long[31];

        dp[0] = 1; dp[1] = 0; dp[2] = 3;
        for(int i=4;i<=n;i++){
            if(i%2 != 0){
                dp[i] = 0;
            }
            else{
                for(int j=2;j<=i;j+=2){
                    if(j==2){
                        dp[i]+= dp[i-j] * dp[2];
                    }
                    else dp[i] += dp[i-j]*2;
                }
            }
        }

        bw.write(dp[n]+"\n");
        bw.flush();
    }

}
```
- dp[n]은 2*n을 채울 수 있는 경우의 수이다.
- n이 홀수일때 dp[n] = 0이다.
- 따라서 짝수인 경우의 수만 계산하면된다.

- n == 4라면
    - | 2 | 2 | 의 경우로 나눌 수 있고 각각의 2는 dp[2]를 의미한다. 
    - 이외에도 n=4일때만 나올 수 있는 고유의 모양이 2개 존재한다.
    - dp[4] = dp[2] * dp[2] + 2이다.
- n == 6이라면
    - | 4 | 2 | , | 2 | 4 | 두가지 경우로 나눈다.
    - | 4 | 2 |인 경우는 dp[4] * dp[2]이다.
    - | 2 | 4 |인 경우는 | 4 | 2 |와 중복되는 경우의 수가 많다. 따라서 dp[4]에서만 나타나는 2가지 모양 * dp[2]로 계산한다.
 

### 출처
https://www.acmicpc.net/problem/2133