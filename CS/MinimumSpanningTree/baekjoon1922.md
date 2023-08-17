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
- 모든 네트워크를 최소한의 비용으로 연결할 수 있도록 해야하기 때문에 이 문제는 최소신장트리 문제이다.
- 최소신장 트리 문제를 해결하는 알고리즘으로는 크루스칼 알고리즘과 프림 알고리즘이 있는데 이 문제는 프림 알고리즘을 사용하여 문제를 해결하였다.

    #### 로직
    - 먼저 1번 컴퓨터부터 등록한다.
    - 그리고 1번 컴퓨터에 연결되어 있는 컴퓨터들에 대한 정보를 dist에 업데이트한다.
    - 1번 컴퓨터에서 가장 최소한에 비용으로 연결할 수 있는 컴퓨터 i를 고른다.
    - 그리고 i 컴퓨터를 등록한 후 그 컴퓨터에 대해서 위의 내용을 처음부터 반복한다.

- 프림 알고리즘은 노드에서 가장 최소한의 비용의 거리를 가지는 다른 노드를 방문 처리하며 모든 노드들에 대해서 방문하는 방식의 알고리즘이고 크루스칼 알고리즘은 모든 간선들에 대한 정보를 가지고 최소한의 비용을 가지는 간선들을 추가하며, 노드들을 방문 처리하는 알고리즘이다.
- 모두 최소신장트리를 구하는 알고리즘임은 같기 때문에 이 문제는 두 방식을 모두 사용해도 해결할 수 있다.

### **[출처]**
https://www.acmicpc.net/problem/1922