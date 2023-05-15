## **점프 점프**


### ***problem***
재환이가 1×N 크기의 미로에 갇혀있다. 미로는 1×1 크기의 칸으로 이루어져 있고, 각 칸에는 정수가 하나 쓰여 있다. i번째 칸에 쓰여 있는 수를 Ai라고 했을 때, 재환이는 Ai이하만큼 오른쪽으로 떨어진 칸으로 한 번에 점프할 수 있다. 예를 들어, 3번째 칸에 쓰여 있는 수가 3이면, 재환이는 4, 5, 6번 칸 중 하나로 점프할 수 있다.

재환이는 지금 미로의 가장 왼쪽 끝에 있고, 가장 오른쪽 끝으로 가려고 한다. 이때, 최소 몇 번 점프를 해야 갈 수 있는지 구하는 프로그램을 작성하시오. 만약, 가장 오른쪽 끝으로 갈 수 없는 경우에는 -1을 출력한다.
#### **Input**
첫째 줄에 N(1 ≤ N ≤ 1,000)이 주어진다. 둘째 줄에는 Ai (0 ≤ Ai ≤ 100)가 주어진다.

#### **Output**
재환이가 최소 몇 번 점프를 해야 가장 오른쪽 끝 칸으로 갈 수 있는지 출력한다. 만약, 가장 오른쪽 끝으로 갈 수 없는 경우에는 -1을 출력한다.


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

        int[] arr = new int[n+1];
        int[] dp = new int[10001];

        st = new StringTokenizer(br.readLine(), " ");
        for(int i=1;i<=n;i++){
            arr[i] = Integer.parseInt(st.nextToken());
        }
        Arrays.fill(dp,Integer.MAX_VALUE-1);
        dp[0] = dp[1] = 0;
        for(int i=1;i<=n;i++){
            for(int j=0;j<=arr[i];j++){
                dp[i+j] = Math.min(dp[i]+1,dp[i+j]);
            }
        }
        int answer = dp[n] == Integer.MAX_VALUE-1 ? -1 : dp[n];
        bw.write(answer+"\n");
        bw.flush();
    }

}
```
- dp[i]는 i번째까지 올 때의 최소 횟수이다.
- j를 arr[i]까지 반복하며 dp[i+j]에 올 수 있는 최소 횟수를 계산한다.
- dp[n]이 Integer.MAX_VALUE-1이면 가장 오른쪽에 도달할 수 없기 때문에 -1을 answer에 넣는다.
- 그게 아니면 그냥 dp[n]을 answer에 넣는다.

### 출처
https://www.acmicpc.net/problem/11060