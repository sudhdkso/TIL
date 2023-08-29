## **나만 안되는 연애**


### ***problem***
깽미는 24살 모태솔로이다. 깽미는 대마법사가 될 순 없다며 자신의 프로그래밍 능력을 이용하여 미팅 어플리케이션을 만들기로 결심했다. 미팅 앱은 대학생을 타겟으로 만들어졌으며 대학교간의 도로 데이터를 수집하여 만들었다.

이 앱은 사용자들을 위해 사심 경로를 제공한다. 이 경로는 3가지 특징을 가지고 있다.

1. 사심 경로는 사용자들의 사심을 만족시키기 위해 남초 대학교와 여초 대학교들을 연결하는 도로로만 이루어져 있다.
2. 사용자들이 다양한 사람과 미팅할 수 있도록 어떤 대학교에서든 모든 대학교로 이동이 가능한 경로이다.
3. 시간을 낭비하지 않고 미팅할 수 있도록 이 경로의 길이는 최단 거리가 되어야 한다.

만약 도로 데이터가 만약 왼쪽의 그림과 같다면, 오른쪽 그림의 보라색 선과 같이 경로를 구성하면 위의 3가지 조건을 만족하는 경로를 만들 수 있다.

<p>
    <img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/14621/1.png">
</p>

이때, 주어지는 거리 데이터를 이용하여 사심 경로의 길이를 구해보자.

#### **Input**
입력의 첫째 줄에 학교의 수 N와 학교를 연결하는 도로의 개수 M이 주어진다. (2 ≤ N ≤ 1,000) (1 ≤ M ≤ 10,000)

둘째 줄에 각 학교가 남초 대학교라면 M, 여초 대학교라면 W이 주어진다.

다음 M개의 줄에 u v d가 주어지며 u학교와 v학교가 연결되어 있으며 이 거리는 d임을 나타낸다. (1 ≤ u, v ≤ N) , (1 ≤ d ≤ 1,000)

#### **Output**
깽미가 만든 앱의 경로 길이를 출력한다. (모든 학교를 연결하는 경로가 없을 경우 -1을 출력한다.)

### ***Solution***
``` java
import java.io.*;
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {
    private static class Node implements Comparable<Node>{
        int index, cost;

        public Node(int index, int cost){
            this.index = index;
            this.cost = cost;
        }

        @Override
        public int compareTo(Node e){
            return Integer.compare(this.cost, e.cost);
        }
    }
    private static int[][] arr;
    private static int INF = Integer.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        arr = new int[N][N];

        String[] school = new String[N];
        st = new StringTokenizer(br.readLine()," ");

        for(int i=0;i<N;i++){
            school[i] = st.nextToken();
        }

        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");
            int[] link = new int[3];

            int a = Integer.parseInt(st.nextToken())-1;
            int b = Integer.parseInt(st.nextToken())-1;
            int c = Integer.parseInt(st.nextToken());

            if(arr[a][b] > 0 && arr[a][b] < c) continue;
            arr[a][b] = arr[b][a] = c;

        }

        int answer = prim(N, school);
        bw.write(String.valueOf(answer));
        bw.flush();

    }

    private static int prim(int N, String[] school){
        PriorityQueue<Node> pq = new PriorityQueue<Node>();
        int[] dist = new int[N];
        boolean[] visited = new boolean[N];

        Arrays.fill(dist, INF);
        dist[0] = 0;
        pq.offer(new Node(0,0));

        int count = 0;
        int sum = 0;
        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;
            sum += now.cost;
            count++;
            if(count == N){
                return sum;
            }

            for(int i=0;i<N;i++){
                if(arr[now.index][i] == 0 || school[now.index].equals(school[i])) continue;
                if(!visited[i] && dist[i] > arr[now.index][i]){
                    dist[i] = arr[now.index][i];
                    pq.offer(new Node(i, dist[i]));
                }
            }


        }
        return -1;
    }
}
```
### **문제 풀이**
- 모든 대학들 방문하는 최소 비용을 구하는 MST문제이다.

- 문제에서 주의해야할 부분은 현재 방문중인 대학의 문자열이 M인 경우 그 노드에서 방문하는 다음 노드는 W이여야하고, W인 경우 역으로 M이어야한다.
- 그래서 같은 알파벳인 경우 해당 노드에 대한 정보를 확인하지 않고 바로 넘긴다.

- 모든 대학을 방문하는 경우 방문한 최소 비용을 저장하는 sum을 return하지만, 모든 대학을 방문하지 않고 while문을 빠져나온 경우라면 -1을 return해준다.

### **[출처]**
https://www.acmicpc.net/problem/14621