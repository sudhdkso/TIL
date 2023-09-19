## **플로이드**


### ***problem***
n(2 ≤ n ≤ 100)개의 도시가 있다. 그리고 한 도시에서 출발하여 다른 도시에 도착하는 m(1 ≤ m ≤ 100,000)개의 버스가 있다. 각 버스는 한 번 사용할 때 필요한 비용이 있다.

모든 도시의 쌍 (A, B)에 대해서 도시 A에서 B로 가는데 필요한 비용의 최솟값을 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 도시의 개수 n이 주어지고 둘째 줄에는 버스의 개수 m이 주어진다. 그리고 셋째 줄부터 m+2줄까지 다음과 같은 버스의 정보가 주어진다. 먼저 처음에는 그 버스의 출발 도시의 번호가 주어진다. 버스의 정보는 버스의 시작 도시 a, 도착 도시 b, 한 번 타는데 필요한 비용 c로 이루어져 있다. 시작 도시와 도착 도시가 같은 경우는 없다. 비용은 100,000보다 작거나 같은 자연수이다.

시작 도시와 도착 도시를 연결하는 노선은 하나가 아닐 수 있다.

#### **Output**
n개의 줄을 출력해야 한다. i번째 줄에 출력하는 j번째 숫자는 도시 i에서 j로 가는데 필요한 최소 비용이다. 만약, i에서 j로 갈 수 없는 경우에는 그 자리에 0을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    static int INF =  100_000_001;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        //StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(br.readLine()); // 노드의 갯수
        int M = Integer.parseInt(br.readLine());

        int[][] arr = new int[N+1][N+1];
        for(int i=1;i<=N;i++){
            Arrays.fill(arr[i], INF);
            arr[i][i] = 0;
        }
        
        for(int i=0;i<M;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            if(arr[a][b] > c){
                arr[a][b] = c;
            }
        }

        for(int d = 1;d<=N;d++){
            for(int i=1;i<=N;i++){
                for(int j=1;j<=N;j++){
                    if(arr[i][j] > arr[i][d]+arr[d][j]){
                        arr[i][j] = arr[i][d] + arr[d][j];
                    }
                }
            }
        }

        StringBuilder sb = new StringBuilder();
        
        for(int i=1;i<=N;i++){
            for(int j=1;j<=N;j++){
                if(arr[i][j] >= INF){
                    sb.append("0 ");
                }
                else sb.append(arr[i][j]+" ");
            }
            sb.append("\n");
        }
        
        bw.write(sb.toString());
        bw.flush();
    }

}
```
### **문제 풀이**


### **[출처]**
https://www.acmicpc.net/problem/11404