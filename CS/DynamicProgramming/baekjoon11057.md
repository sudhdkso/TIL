## **오르막 수**


### ***problem***
오르막 수는 수의 자리가 오름차순을 이루는 수를 말한다. 이때, 인접한 수가 같아도 오름차순으로 친다.

예를 들어, 2234와 3678, 11119는 오르막 수이지만, 2232, 3676, 91111은 오르막 수가 아니다.

수의 길이 N이 주어졌을 때, 오르막 수의 개수를 구하는 프로그램을 작성하시오. 수는 0으로 시작할 수 있다.

#### **Input**
첫째 줄에 N (1 ≤ N ≤ 1,000)이 주어진다.

#### **Output**
첫째 줄에 길이가 N인 오르막 수의 개수를 10,007로 나눈 나머지를 출력한다.

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

        long[][] dp = new long[1001][10];

        for(int i=0;i<=9;i++){
            dp[1][i] = 1;
        }

        for(int i=2;i<=n;i++){
            for(int j=9;j>=0;j--){
                for(int k=j;k<=9;k++){
                    dp[i][j] += (dp[i-1][k]) % 10007;
                }
            }
        }
        
        long answer = 0;
        for(int i=0;i<=9;i++){
            answer+= dp[n][i];
        }

        bw.write((answer % 10007)+"\n");
        bw.flush();
    }

}
```
- dp는 이차원 배열로, dp[n][0]은 길이가 n이며 바로 n-1이 0으로 끝날 때 n자리에 올 수 있는 수의 갯수를 담고있다.

- 예를들어 dp[2][0]이면 첫번째 자리가 0이면 오름 수가 되기위해서 두번째자리는 0~9까지 모두 올 수 있습니다. 
- 따라서 dp[2][0] = dp[1][0] ~ dp[1][9]의 합이다.
- 똑같이 dp[2][1] = dp[1][1] ~ dp[1][9]의 합이다.
- 따라서 점화식을 세우면 dp[i][j] = dp[i-1][k] ~ dp[i-1][9]의 합이다. (단, k>=j)


### 출처
https://www.acmicpc.net/problem/11057