## **네트워크**


### ***problem***
도현이는 컴퓨터와 컴퓨터를 모두 연결하는 네트워크를 구축하려 한다. 하지만 아쉽게도 허브가 있지 않아 컴퓨터와 컴퓨터를 직접 연결하여야 한다. 그런데 모두가 자료를 공유하기 위해서는 모든 컴퓨터가 연결이 되어 있어야 한다. (a와 b가 연결이 되어 있다는 말은 a에서 b로의 경로가 존재한다는 것을 의미한다. a에서 b를 연결하는 선이 있고, b와 c를 연결하는 선이 있으면 a와 c는 연결이 되어 있다.)

그런데 이왕이면 컴퓨터를 연결하는 비용을 최소로 하여야 컴퓨터를 연결하는 비용 외에 다른 곳에 돈을 더 쓸 수 있을 것이다. 이제 각 컴퓨터를 연결하는데 필요한 비용이 주어졌을 때 모든 컴퓨터를 연결하는데 필요한 최소비용을 출력하라. 모든 컴퓨터를 연결할 수 없는 경우는 없다.

#### **Input**
첫째 줄에 컴퓨터의 수 N (1 ≤ N ≤ 1000)가 주어진다.

둘째 줄에는 연결할 수 있는 선의 수 M (1 ≤ M ≤ 100,000)가 주어진다.

셋째 줄부터 M+2번째 줄까지 총 M개의 줄에 각 컴퓨터를 연결하는데 드는 비용이 주어진다. 이 비용의 정보는 세 개의 정수로 주어지는데, 만약에 a b c 가 주어져 있다고 하면 a컴퓨터와 b컴퓨터를 연결하는데 비용이 c (1 ≤ c ≤ 10,000) 만큼 든다는 것을 의미한다. a와 b는 같을 수도 있다.

#### **Output**
모든 컴퓨터를 연결하는데 필요한 최소비용을 첫째 줄에 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());
        int[][] list = new int[N+1][N+1];

        for(int i=0;i<M;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            list[a][b] = list[b][a] = c;
        }

        int[] dist = new int[N+1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[1] = 0;
        int cost = 0, count = 0;
        boolean[] visited = new boolean[N+1];
        while(true){
            int min = Integer.MAX_VALUE;
            int index = 0;
            for(int  i=1;i<=N;i++){
                if(!visited[i] && dist[i] < min){
                    min = dist[i];
                    index = i;
                }
            }

            visited[index] = true;
            cost+= min;
            count++;
            if(count == N){
                break;
            }

            for(int i=1;i<=N;i++){
                if(!visited[i] && list[index][i] > 0 && dist[i] > list[index][i]){
                    dist[i] = list[index][i];
                }
            }
        }
        bw.write(String.valueOf(cost)+"\n");
        bw.flush();
    }



}


```
### **문제 풀이**



### **[출처]**
https://www.acmicpc.net/problem/1922