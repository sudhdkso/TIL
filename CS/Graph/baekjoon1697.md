## **숨바꼭질**


#### ***problem***
수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 1초 후에 2*X의 위치로 이동하게 된다.

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.


#### Input
첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.

#### Output
수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.

### ***Solution***
``` java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;

public class Main {
    public static int[] arr = new int[200001];
    public static boolean[] visited = new boolean[200001];
    public static int[] dx = {1, -1, 2};
    public static void bfs(int N, int K){
        Queue<Integer> q = new LinkedList<>();
        q.add(N);
        visited[N] = true;
        while(!q.isEmpty()){
            int x = q.remove();

            if(x == K){
                return;
            }

            for(int i=0;i<3;i++){
                int nx = 0;
                if(dx[i] == 2){
                    nx = x * dx[i];
                }
                else {
                    nx = x + dx[i];
                }
                if(nx >= 0 && nx <= 200000) {
                    if(visited[nx] == true) continue;
                    q.add(nx);
                    visited[nx] = true;
                    arr[nx] = arr[x] + 1;
                }
            }
        }
        return;
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String[] NK = br.readLine().split(" ");
        int N = Integer.parseInt(NK[0]);
        int K = Integer.parseInt(NK[1]);
        Arrays.fill(arr, 0);
        bfs(N,K);
        bw.write(String.valueOf(arr[K])+"\n");
        bw.flush();


    }
}


```

- visited[i]는 i의 방문여부를 저장하는 booelan타입의 배열이다.
- arr[i]는 i까지 가는데 걸린 초(second)를 저장하는 배열이다.


### 출처
https://www.acmicpc.net/problem/1697