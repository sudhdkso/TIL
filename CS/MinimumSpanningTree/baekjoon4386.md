## **별자리 만들기**


### ***problem***
도현이는 우주의 신이다. 이제 도현이는 아무렇게나 널브러져 있는 n개의 별들을 이어서 별자리를 하나 만들 것이다. 별자리의 조건은 다음과 같다.

- 별자리를 이루는 선은 서로 다른 두 별을 일직선으로 이은 형태이다.
- 모든 별들은 별자리 위의 선을 통해 서로 직/간접적으로 이어져 있어야 한다.

별들이 2차원 평면 위에 놓여 있다. 선을 하나 이을 때마다 두 별 사이의 거리만큼의 비용이 든다고 할 때, 별자리를 만드는 최소 비용을 구하시오.

#### **Input**
첫째 줄에 별의 개수 n이 주어진다. (1 ≤ n ≤ 100)

둘째 줄부터 n개의 줄에 걸쳐 각 별의 x, y좌표가 실수 형태로 주어지며, 최대 소수점 둘째자리까지 주어진다. 좌표는 1000을 넘지 않는 양의 실수이다.

#### **Output**
첫째 줄에 정답을 출력한다. 절대/상대 오차는 10-2까지 허용한다.

### ***Solution***
``` java
import java.io.*;
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {
    private static class Node implements Comparable<Node>{
        int index;
        double cost;

        public Node(int index, double cost){
            this.index = index;
            this.cost = cost;
        }

        @Override
        public int compareTo(Node e){
            return Double.compare(this.cost, e.cost);
        }
    }
    private static double[][] arr;
    private static double INF = Double.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());

        arr = new double[N][N];
        double[][] star = new double[N][2];
        for(int i=0;i<N;i++){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            star[i][0] = Double.parseDouble(st.nextToken());
            star[i][1] = Double.parseDouble(st.nextToken());
        }

        for(int i=0;i<N;i++){
            for(int j=i+1;j<N;j++){
                double dx = Math.pow(star[i][0]-star[j][0],2);
                double dy = Math.pow(star[i][1]-star[j][1],2);
                arr[i][j] = arr[j][i] = Math.sqrt(dx+dy);
            }
        }

        double answer = prim(N);
        bw.write(String.format("%.2f", answer));
        bw.flush();

    }

    private static double prim(int N){
        PriorityQueue<Node> pq = new PriorityQueue<Node>();
        double[] dist = new double[N];
        boolean[] visited = new boolean[N];

        Arrays.fill(dist, INF);
        dist[0] = 0.0;
        pq.offer(new Node(0,0.0));

        int count = 0;
        double sum = 0;
        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;

            sum += now.cost;
            count++;
            if(count == N){
                break;
            }

            for(int i=0;i<N;i++){
                if(arr[now.index][i] == 0) continue;
                if(!visited[i] && dist[i] > arr[now.index][i]){
                    dist[i] = arr[now.index][i];
                    pq.offer(new Node(i, dist[i]));
                }
            }


        }
        return sum;
    }
}
```
### **문제 풀이**


### **[출처]**
https://www.acmicpc.net/problem/4386