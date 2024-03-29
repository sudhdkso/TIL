## **효율적인 해킹**


### **problem**
케빈 베이컨의 6단계 법칙에 의하면 지구에 있는 모든 사람들은 최대 6단계 이내에서 서로 아는 사람으로 연결될 수 있다. 케빈 베이컨 게임은 임의의 두 사람이 최소 몇 단계 만에 이어질 수 있는지 계산하는 게임이다.

예를 들면, 전혀 상관없을 것 같은 인하대학교의 이강호와 서강대학교의 민세희는 몇 단계만에 이어질 수 있을까?

천민호는 이강호와 같은 학교에 다니는 사이이다. 천민호와 최백준은 Baekjoon Online Judge를 통해 알게 되었다. 최백준과 김선영은 같이 Startlink를 창업했다. 김선영과 김도현은 같은 학교 동아리 소속이다. 김도현과 민세희는 같은 학교에 다니는 사이로 서로 알고 있다. 즉, 이강호-천민호-최백준-김선영-김도현-민세희 와 같이 5단계만 거치면 된다.

케빈 베이컨은 미국 헐리우드 영화배우들 끼리 케빈 베이컨 게임을 했을때 나오는 단계의 총 합이 가장 적은 사람이라고 한다.

오늘은 Baekjoon Online Judge의 유저 중에서 케빈 베이컨의 수가 가장 작은 사람을 찾으려고 한다. 케빈 베이컨 수는 모든 사람과 케빈 베이컨 게임을 했을 때, 나오는 단계의 합이다.

예를 들어, BOJ의 유저가 5명이고, 1과 3, 1과 4, 2와 3, 3과 4, 4와 5가 친구인 경우를 생각해보자.

1은 2까지 3을 통해 2단계 만에, 3까지 1단계, 4까지 1단계, 5까지 4를 통해서 2단계 만에 알 수 있다. 따라서, 케빈 베이컨의 수는 2+1+1+2 = 6이다.

2는 1까지 3을 통해서 2단계 만에, 3까지 1단계 만에, 4까지 3을 통해서 2단계 만에, 5까지 3과 4를 통해서 3단계 만에 알 수 있다. 따라서, 케빈 베이컨의 수는 2+1+2+3 = 8이다.

3은 1까지 1단계, 2까지 1단계, 4까지 1단계, 5까지 4를 통해 2단계 만에 알 수 있다. 따라서, 케빈 베이컨의 수는 1+1+1+2 = 5이다.

4는 1까지 1단계, 2까지 3을 통해 2단계, 3까지 1단계, 5까지 1단계 만에 알 수 있다. 4의 케빈 베이컨의 수는 1+2+1+1 = 5가 된다.

마지막으로 5는 1까지 4를 통해 2단계, 2까지 4와 3을 통해 3단계, 3까지 4를 통해 2단계, 4까지 1단계 만에 알 수 있다. 5의 케빈 베이컨의 수는 2+3+2+1 = 8이다.

5명의 유저 중에서 케빈 베이컨의 수가 가장 작은 사람은 3과 4이다.

BOJ 유저의 수와 친구 관계가 입력으로 주어졌을 때, 케빈 베이컨의 수가 가장 작은 사람을 구하는 프로그램을 작성하시오.


#### **Input**
첫째 줄에 유저의 수 N (2 ≤ N ≤ 100)과 친구 관계의 수 M (1 ≤ M ≤ 5,000)이 주어진다. 둘째 줄부터 M개의 줄에는 친구 관계가 주어진다. 친구 관계는 A와 B로 이루어져 있으며, A와 B가 친구라는 뜻이다. A와 B가 친구이면, B와 A도 친구이며, A와 B가 같은 경우는 없다. 친구 관계는 중복되어 들어올 수도 있으며, 친구가 한 명도 없는 사람은 없다. 또, 모든 사람은 친구 관계로 연결되어져 있다. 사람의 번호는 1부터 N까지이며, 두 사람이 같은 번호를 갖는 경우는 없다.

#### **Output**
첫째 줄에 BOJ의 유저 중에서 케빈 베이컨의 수가 가장 작은 사람을 출력한다. 그런 사람이 여러 명일 경우에는 번호가 가장 작은 사람을 출력한다.

### ***Solution***
#### **\< BFS \>**
``` java
import java.io.*;
import java.util.*;

public class Main {
    private static class Bacon{
        int name,count;
        public Bacon(int name, int count){
            this.name = name;
            this.count = count;
        }
    }
    public static int[][] arr;
    public static boolean[] visited;
    public static int min = Integer.MAX_VALUE;

    public static int bfs(int node, int n) {
        Queue<Bacon> q = new LinkedList<>();
        visited[node] = true;
        q.offer(new Bacon(node, 0));

        int count  = 0;
        while(!q.isEmpty()){
            Bacon b = q.poll();
            count += b.count;
            for(int i = 1; i <n+1;i++){
                if(arr[b.name][i] == 1 && !visited[i]){
                    visited[i] = true;
                    q.offer(new Bacon(i,b.count+1));
                }
            }
        }

        return count;
    }



    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String[] s = br.readLine().split(" ");
        int n = Integer.parseInt(s[0]);
        int m = Integer.parseInt(s[1]);

        arr = new int[n+1][n+1];
        visited = new boolean[n+1];

        for(int i=0;i<m;i++){
            String[] AB = br.readLine().split(" ");
            int a = Integer.parseInt(AB[0]);
            int b = Integer.parseInt(AB[1]);
            arr[a][b] = 1;
            arr[b][a] = 1;
        }

        int index = 0;
        for(int i = 1 ;i<n+1; i++){
            visited = new boolean[n+1];
            int result = bfs(i,n);
            if(result < min){
                min = result;
                index = i;
            }

        }
        bw.write(index+"\n");
        bw.flush();
    }

}
```

#### **\< dfs \>**
``` java
import java.io.*;
import java.util.*;

public class Main {

    public static ArrayList<Integer>[] arrayList;
    public static boolean[] visited;
    public static int[] depth;

    public static void dfs(int node,int count) {
        visited[node] = true;
        depth[node] = Math.min(depth[node],count);

        for(int n: arrayList[node]){
            if(!visited[n]){
                visited[n] = true;
                dfs(n,count+1);
                visited[n] = false;
            }
        }
    }



    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String[] s = br.readLine().split(" ");
        int n = Integer.parseInt(s[0]);
        int m = Integer.parseInt(s[1]);

        arrayList = new ArrayList[n+1];
        visited = new boolean[n+1];
        depth = new int[n+1];

        for(int i = 1 ;i<n+1;i++){
            arrayList[i] = new ArrayList<>();
        }

        for(int i=0;i<m;i++){
            String[] AB = br.readLine().split(" ");
            int a = Integer.parseInt(AB[0]);
            int b = Integer.parseInt(AB[1]);
            arrayList[a].add(b);
            arrayList[b].add(a);
        }

        for(int i = 1 ;i<n+1; i++){
            Collections.sort(arrayList[i]);
        }
        int index = 0;
        int min = Integer.MAX_VALUE;
        for(int i = 1 ;i<n+1; i++){
            visited = new boolean[n+1];
            Arrays.fill(depth,Integer.MAX_VALUE);
            dfs(i,0);
            int count = 0;

            for(int j=1;j<n+1;j++){
                if(depth[i] == Integer.MAX_VALUE) continue;
                count+=depth[j];
            }
            if(min > count){
                min = count;
                index = i;
            }
        }
        bw.write(index+"\n");
        bw.flush();
    }

}
```
- bfs와 dfs를 둘 다 사용하여 구현하였다.
- bfs에서는 친구 연관관계를 이차원 배열 arr에 넣어서 친구관계를 표현하였다.
- 시작노드를 기준으로 각각의 친구까지 몇단계인지 확인해야하므로 visited를 반복문이 돌며 bfs로 탐색하기 전에 계속 초기화해주었다.
- bacon클래스는 친구의 이름과 그 친구와 몇단계까지 걸쳐졌는지 count를 가지고 있는 클래스이다.
- 시작 친구를 큐에 넣어주고 큐를 돌며 시작 친구와 연관된 친구들을 큐에 모두 넣어준다.
- 그리고 그 친구까지의 count들을 다 지역변수 count에 더하여 마지막에 return해준다.
- 이렇게 return한 결과가 min보다 작으면 min에 결과를 저장하고 index에는 시작 친구를 저장한다.
- 이때 결과가 같으면 index가 더 작은아이를 출력해야하기 때문에, 반복문은 순서대로 돌아서 min과 같을때는 판단하지 않는다.


- dfs의 경우는 각각의 친구관계들을 모두 arrayList배열에 저장한다.
- 그리고 시작 친구를 기준으로 각각의 친구들에게 몇depth만에 도착할 수 있는지 depth배열에 저장한다.
- 모두 탐색을 하고 depth배열의 모든 값을 더하고 그 값이 최소값이면 min에 저장하고, 그 시작 친구도 index에 저장한다.ㄴ


### 출처
https://www.acmicpc.net/problem/1389