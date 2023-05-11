## **동물원**


### ***problem***
어떤 동물원에 가로로 두칸 세로로 N칸인 아래와 같은 우리가 있다.

이 동물원에는 사자들이 살고 있는데 사자들을 우리에 가둘 때, 가로로도 세로로도 붙어 있게 배치할 수는 없다. 이 동물원 조련사는 사자들의 배치 문제 때문에 골머리를 앓고 있다.

동물원 조련사의 머리가 아프지 않도록 우리가 2*N 배열에 사자를 배치하는 경우의 수가 몇 가지인지를 알아내는 프로그램을 작성해 주도록 하자. 사자를 한 마리도 배치하지 않는 경우도 하나의 경우의 수로 친다고 가정한다.

#### **Input**
첫째 줄에 우리의 크기 N(1≤N≤100,000)이 주어진다.

#### **Output**
첫째 줄에 사자를 배치하는 경우의 수를 9901로 나눈 나머지를 출력하여라.

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

        long[][] dp = new long[100001][3];

        Arrays.fill(dp[1],1);

        for(int i=2;i<=n;i++){
            dp[i][0] = (dp[i-1][0] + dp[i-1][1] + dp[i-1][2])%9901;
            dp[i][1] = (dp[i-1][0] + dp[i-1][2])%9901;
            dp[i][2] = (dp[i-1][0] + dp[i-1][1])%9901;
        }

        long answer = (dp[n][0] + dp[n][1] + dp[n][2])% 9901;
        bw.write(answer+"\n");
        bw.flush();
    }

}
```
- dp[n][0]은 n번째 줄에 사자를 배치하지 않는 경우이다.
- dp[n][1]은 n번째 줄에 사자를 왼쪽에 배치하는 경우이다.
- dp[n][2]는 n번째 줄에 사자를 오른쪽에 배치하는 경우이다.

- 따라서 dp[i][0]인 경우는 i-1번째 줄에 사자를 아예 배치하지 않거나,왼쪽에 배치하거나, 오른쪽에 배치한 경우의 수를 모두 합한 값이다.

- dp[i][1]은 i-1번째 줄에 사자를 아예 배치하지 않거나 사자를 오른쪽에 배치한 경우의 수를 모두 더한 값이다.

- dp[i][2]는 i-1번째 줄에 사자를 아예 배치하지 않거나 사자를 왼쪽에 배치한 경우의 수를 모두 더한 값이다.

- 따라서 답은 dp[n][0] + dp[n][1] + dp[n][2]한 값이다.

### 출처
https://www.acmicpc.net/problem/1309