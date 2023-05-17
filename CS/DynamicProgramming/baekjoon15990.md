## **1, 2, 3 더하기 5**


### ***problem***
정수 4를 1, 2, 3의 합으로 나타내는 방법은 총 3가지가 있다. 합을 나타낼 때는 수를 1개 이상 사용해야 한다. 단, 같은 수를 두 번 이상 연속해서 사용하면 안 된다.

- 1+2+1
- 1+3
- 3+1

정수 n이 주어졌을 때, n을 1, 2, 3의 합으로 나타내는 방법의 수를 구하는 프로그램을 작성하시오.
#### **Input**
첫째 줄에 테스트 케이스의 개수 T가 주어진다. 각 테스트 케이스는 한 줄로 이루어져 있고, 정수 n이 주어진다. n은 양수이며 100,000보다 작거나 같다.

#### **Output**
각 테스트 케이스마다, n을 1, 2, 3의 합으로 나타내는 방법의 수를 1,000,000,009로 나눈 나머지를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int t = Integer.parseInt(st.nextToken());

        long[][] dp = new long[100001][4];

        dp[1][1] = dp[2][2] = 1;
        dp[3][1] = dp[3][2] = dp[3][3] = 1;
        for(int i=4;i<=100000;i++){
            dp[i][1] = (dp[i-1][2] + dp[i-1][3])%1000000009;
            dp[i][2] = (dp[i-2][1] + dp[i-2][3])%1000000009;
            dp[i][3] = (dp[i-3][1] + dp[i-3][2])%1000000009;

        }

        while(t-- > 0){
            int num = Integer.parseInt(br.readLine());
            bw.write((dp[num][1] + dp[num][2] + dp[num][3])%1000000009+"\n");
        }
        bw.flush();
    }

}
```
- dp[i][1]은 숫자 i를 만들 때 가장 마지막에 오는 숫자가 1인 경우의 수이다.
- dp[i][2]은 숫자 i를 만들 때 가장 마지막에 오는 숫자가 2인 경우의 수이다.
- dp[i][3]은 숫자 i를 만들 때 가장 마지막에 오는 숫자가 3인 경우의 수이다.
- dp[i][1]이 될 수 있는 건 dp[i-1][2]와 dp[i-1][3]에 1을 더하는 경우이다.
    - 이외에도 dp[i][2]는 dp[i-2][1]과 dp[i-2][3]에 2를 더하는 경우이다.
    - 이외에도 dp[i][3]는 dp[i-3][1]과 dp[i-3][2]에 3을 더하는 경우이다.
- 이런식으로 n의 최댓값인 100,000까지 경우의 수를 모두 계산한다.
- 그 후 입력받은 숫자 num에 대해서 dp[num][1] + dp[num][2] + dp[num][3]을 더한 값이 num을 숫자 1,2,3을 이용해서, 똑같은 숫자를 연속해서 사용하지 않고 만들 수 있는 경우의 수이다.
    - 1,000,000,009로 나누어줘야한다.
### 출처
https://www.acmicpc.net/problem/15990