## **불우이웃돕기**


### ***problem***
다솜이는 불우이웃 돕기 활동을 하기 위해 무엇을 할지 생각했다. 마침 집에 엄청나게 많은 랜선이 있다는 것을 깨달았다. 마침 랜선이 이렇게 많이 필요 없다고 느낀 다솜이는 랜선을 지역사회에 봉사하기로 했다.

다솜이의 집에는 N개의 방이 있다. 각각의 방에는 모두 한 개의 컴퓨터가 있다. 각각의 컴퓨터는 랜선으로 연결되어 있다. 어떤 컴퓨터 A와 컴퓨터 B가 있을 때, A와 B가 서로 랜선으로 연결되어 있거나, 또 다른 컴퓨터를 통해서 연결이 되어있으면 서로 통신을 할 수 있다.

다솜이는 집 안에 있는 N개의 컴퓨터를 모두 서로 연결되게 하고 싶다.

N개의 컴퓨터가 서로 연결되어 있는 랜선의 길이가 주어질 때, 다솜이가 기부할 수 있는 랜선의 길이의 최댓값을 출력하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 컴퓨터의 개수 N이 주어진다. 둘째 줄부터 랜선의 길이가 주어진다. i번째 줄의 j번째 문자가 0인 경우는 컴퓨터 i와 컴퓨터 j를 연결하는 랜선이 없음을 의미한다. 그 외의 경우는 랜선의 길이를 의미한다. 랜선의 길이는 a부터 z는 1부터 26을 나타내고, A부터 Z는 27부터 52를 나타낸다. N은 50보다 작거나 같은 자연수이다.

#### **Output**
첫째 줄에 다솜이가 기부할 수 있는 랜선의 길이의 최댓값을 출력한다. 만약, 모든 컴퓨터가 연결되어 있지 않으면 -1을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    private static class Edge implements Comparable<Edge>{
        int nodeA,nodeB, cost;
        public Edge(int nodeA,int nodeB ,int cost){
            this.nodeA = nodeA;
            this.nodeB = nodeB;
            this.cost = cost;
        }

        @Override
        public int compareTo(Edge e){
            return Integer.compare(this.cost, e.cost);
        }
    }

    static int[] parent = new int[50];
    static List<Edge> edges = new ArrayList<>();

    static int INF = Integer.MAX_VALUE;
    static List<Character> list = new ArrayList<>();
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());

        init();

        int length = 0;

        for(int i=0;i<N;i++){
            char[] ch = br.readLine().toCharArray();
            for(int j=0;j<N;j++){
                if(ch[j] != '0'){
                    int lan = list.indexOf(ch[j])+1;
                    edges.add(new Edge(i,j, lan));
                    length += lan;
                }
            }
        }

        Collections.sort(edges);

        int sum = 0, count = 0;
        for(Edge e : edges){
            int a = e.nodeA;
            int b = e.nodeB;
            int c = e.cost;

            if(findParent(a) != findParent(b)){
                union(a,b);
                sum += c;
                count++;
            }
        }
        int answer = -1;
        if(count == N-1){
            answer = length - sum;
        }
        bw.write(String.valueOf(answer));
        bw.flush();
    }

    private static void init(){

        for(int i=0;i<50;i++){
            parent[i] = i;
        }

        for(int i=0;i<26;i++){
            list.add((char)('a'+i));
        }
        for(int i=0;i<26;i++){
            list.add((char)('A'+i));
        }
    }

    private static int findParent(int x){
        if(x == parent[x]){
            return x;
        }
        return parent[x] = findParent(parent[x]);
    }

    private static void union(int a, int b){
        a = findParent(a);
        b = findParent(b);
        if(a < b){
            parent[b] = a;
        }
        else{
            parent[a] = b;
        }
    }

}
```
### **문제 풀이**

### **[출처]**
https://www.acmicpc.net/problem/1414