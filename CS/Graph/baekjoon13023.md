## **ABCDE**


### ***problem***
BOJ 알고리즘 캠프에는 총 N명이 참가하고 있다. 사람들은 0번부터 N-1번으로 번호가 매겨져 있고, 일부 사람들은 친구이다.

오늘은 다음과 같은 친구 관계를 가진 사람 A, B, C, D, E가 존재하는지 구해보려고 한다.

- A는 B와 친구다.
- B는 C와 친구다.
- C는 D와 친구다.
- D는 E와 친구다.
위와 같은 친구 관계가 존재하는지 안하는지 구하는 프로그램을 작성하시오.


#### **Input**
첫째 줄에 사람의 수 N (5 ≤ N ≤ 2000)과 친구 관계의 수 M (1 ≤ M ≤ 2000)이 주어진다.

둘째 줄부터 M개의 줄에는 정수 a와 b가 주어지며, a와 b가 친구라는 뜻이다. (0 ≤ a, b ≤ N-1, a ≠ b) 같은 친구 관계가 두 번 이상 주어지는 경우는 없다.

#### **Output**
문제의 조건에 맞는 A, B, C, D, E가 존재하면 1을 없으면 0을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    public static boolean[] visited;
    public static ArrayList<Integer>[] arrayList;
    public static boolean check = false;
    public static void dfs(int node,int count){
        visited[node] = true;
        if(count >= 4){
            check = true;
            return;
        }

        for(int n: arrayList[node]){
            if(check) return;
            if(!visited[n]){
                visited[n] = true;
                dfs(n,++count);
                count--;
                visited[n] = false;
            }
        }
    }
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String[] NM = br.readLine().split(" ");

        int n = Integer.parseInt(NM[0]);
        int m = Integer.parseInt(NM[1]);
        arrayList = new ArrayList[n];
        visited = new boolean[n];

        for(int i=0;i<n;i++){
            arrayList[i] = new ArrayList<>();
        }

        for(int i=0;i<m;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            arrayList[a].add(b);
            arrayList[b].add(a);
        }

        for(int i=0;i<n;i++){
            Collections.sort(arrayList[i]);
        }
        for(int i=0;i<n;i++){
            if(check) break;
            visited = new boolean[n];
            dfs(i,0);
        }
        if(check){
            bw.write(1+"\n");
        }
        else{
            bw.write(0+"\n");
        }
        bw.flush();
    }

}
```
- 단순 dfs문제였지만 문제의 `A, B, C, D, E`가 존재한다는 의미가 어려워 많이 헷갈렸던 문제이다.

- `A, B, C, D, E`는 노드가 연속적으로 5개가 연결되어 있는 경우가 존재하냐는 의미였다. 노드가 5개이면 depth 4이기 때문에 dfs의 depth를 count로 정의하고 count가 4이상인 경우 return해주도록 하였다.
- 그리고 생각보다 시간이 걸리는 문제였기에,`A, B, C, D, E`가 존재하면 바로 return해줄 수 있도록 check를 전역적으로 선언하고 dfs의 그다음 노드를 탐색하는 과정에서 check가 true이며 바로 return해주었다.


### 출처
https://www.acmicpc.net/problem/13023