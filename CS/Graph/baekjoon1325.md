## **효율적인 해킹**


### **problem**
해커 김지민은 잘 알려진 어느 회사를 해킹하려고 한다. 이 회사는 N개의 컴퓨터로 이루어져 있다. 김지민은 귀찮기 때문에, 한 번의 해킹으로 여러 개의 컴퓨터를 해킹 할 수 있는 컴퓨터를 해킹하려고 한다.

이 회사의 컴퓨터는 신뢰하는 관계와, 신뢰하지 않는 관계로 이루어져 있는데, A가 B를 신뢰하는 경우에는 B를 해킹하면, A도 해킹할 수 있다는 소리다.

이 회사의 컴퓨터의 신뢰하는 관계가 주어졌을 때, 한 번에 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 출력하는 프로그램을 작성하시오.


#### **Input**
첫째 줄에, N과 M이 들어온다. N은 10,000보다 작거나 같은 자연수, M은 100,000보다 작거나 같은 자연수이다. 둘째 줄부터 M개의 줄에 신뢰하는 관계가 A B와 같은 형식으로 들어오며, "A가 B를 신뢰한다"를 의미한다. 컴퓨터는 1번부터 N번까지 번호가 하나씩 매겨져 있다.

#### **Output**
첫째 줄에, 김지민이 한 번에 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 오름차순으로 출력한다.

### ***Solution***
``` java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;

public class Main {

    public static boolean[] visited;
    public static ArrayList<Integer>[] arr;
    public static int[] depth;
	
    
    public static void dfs(int node){
        visited[node] = true;
        for(int n: arr[node]){
            if(!visited[n]){
                depth[n]++;
                visited[n] = true;
                dfs(n);
            }
        }
    }
	

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringBuilder sb = new StringBuilder();

        String[] NM = br.readLine().split(" ");
        int N = Integer.parseInt(NM[0]);
        int M = Integer.parseInt(NM[1]);

        arr = new ArrayList[N];

        for(int i=0;i<N;i++){
            arr[i] = new ArrayList<Integer>();
        }

        for(int i=0;i<M;i++){
            String[] AB = br.readLine().split(" ");
            int A = Integer.parseInt(AB[0]);
            int B = Integer.parseInt(AB[1]);
            arr[A-1].add(B-1);
        }

        depth = new int[N];
        for(int i=0;i<N;i++){
            visited = new boolean[N];
            dfs(i);
        }

        int max = 0;

        for(int i=0;i<N;i++){
            max = Math.max(max, depth[i]);
        }

        for(int i=0;i<N;i++){
            if(depth[i] == max){
                sb.append(i+1).append(" ");

            }
        }

        bw.write(sb.toString());
        bw.flush();
    }
}
```
- depth는 i번째 컴퓨터를 해킹하면 몇개의 컴퓨터가 함께 해킹당하는지 저장하는 int타입 배열이다.
- visited는 i번째 컴퓨터를 방문했는지 확인하는 boolean타입 배열이다.
- dfs를 활용하여 각 노드의 컴퓨터를 해킹할 때 몇개의 컴퓨터가 함께 해킹당하는지 구한다.
- 모든 노드에 대해서 dfs를 한 뒤 depth의 max값을 구한다.
- 그 max값을 가지고 있는 index를 StringBuilder타입의 sb에 append한다.



### 출처
https://www.acmicpc.net/problem/1325