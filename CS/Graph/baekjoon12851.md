## **숨바꼭질 2**


### ***problem***
수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 1초 후에 2*X의 위치로 이동하게 된다.

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 그리고, 가장 빠른 시간으로 찾는 방법이 몇 가지 인지 구하는 프로그램을 작성하시오.


#### **Input**
첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.

#### **Output**
첫째 줄에 수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.

둘째 줄에는 가장 빠른 시간으로 수빈이가 동생을 찾는 방법의 수를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static class Coordi{
        int x, time;

        public Coordi(int x, int time) {
            this.x = x;
            this.time = time;
        }
    }
    static boolean[] visited;
    static int min = Integer.MAX_VALUE;
    static int count = 0;
    private static void bfs(int N, int K){
        PriorityQueue<Coordi> pq = new PriorityQueue<>((o1,o2) -> o1.time-o2.time);
        pq.offer(new Coordi(N,0));
        visited[N] = true;
        while(!pq.isEmpty()){
            Coordi cd = pq.poll();
            visited[cd.x] = true;
            if(cd.x == K){
                if(min == cd.time){
                    count++;
                }
                if(min > cd.time){
                    min = cd.time;
                    count = 1;
                }
            }
            if(cd.x*2 <=100_001 && !visited[cd.x*2]){
                pq.offer(new Coordi(cd.x*2,cd.time+1));
            }
            if(cd.x+1 <= 100_001 && !visited[cd.x+1]){
                pq.offer(new Coordi(cd.x+1,cd.time+1));
            }
            if(cd.x-1 >= 0 && !visited[cd.x-1]){
                pq.offer(new Coordi(cd.x-1,cd.time+1));
            }
        }

    }
    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());
        visited = new boolean[100_002];
        bfs(N,K);
        bw.write(min+"\n"+count);
        bw.flush();
    }

}
```
- [숨바꼭질3](CS\Graph\baekjoon13549.md)번 문제와 거의 똑같은 문제이다.
- 다만 bfs를 통해 K에 도착했을 때 min값이 같으면 count를 추가하고, 최솟값이 변경될 때는 count를 초기화해줘야한다.

### 출처
https://www.acmicpc.net/problem/12851