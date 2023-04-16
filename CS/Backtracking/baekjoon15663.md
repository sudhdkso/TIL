### **N과 M (9)**


#### ***problem***
N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

- N개의 자연수 중에서 M개를 고른 수열


#### **Input**
첫째 줄에 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)

둘째 줄에 N개의 수가 주어진다. 입력으로 주어지는 수는 10,000보다 작거나 같은 자연수이다.

#### **Output**
한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.
### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    public static boolean[] visited;
    public static int[] arr,temp;
    public static StringBuilder sb = new StringBuilder();

    public static void go(int index, int n, int m){
        if(index >= m){
            for(int i=0;i<m;i++){
                sb.append(temp[i]).append(" ");
            }
            sb.append("\n");
            return;
        }

        for(int i=0;i<n;i++){
            if(!visited[i]){
                if(i > 0 && arr[i-1] == arr[i] && !visited[i-1]){
                    continue;
                }
                visited[i] = true;
                temp[index] = arr[i];
                go(index+1,n,m);
                visited[i] = false;
            }
        }
    }

    public static void main(String[] args)throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String[] NM = br.readLine().split(" ");
        int n = Integer.parseInt(NM[0]);
        int m = Integer.parseInt(NM[1]);

        arr = new int[n];
        visited = new boolean[n];
        temp = new int[m];

        String[] s = br.readLine().split(" ");

        for(int i=0;i<n;i++){
            arr[i] = Integer.parseInt(s[i]);
        }

        Arrays.sort(arr);

        go(0,n,m);

        bw.write(String.valueOf(sb));
        bw.flush();
    }


}
```
- n개의 숫자들로 길이가 m인 수열을 구하는 문제이다.
- 수열을 이루는 숫자 n개 중에서는 같은 값이 포함되어 있을 수 있는데 예를 들어 주어진 숫자 n개가 `[1,2,9,9]`라고 가정했을 때 `[2 9]`와 같이 9가 포함된 수열의 경우 3번째 9인지 4번째 9인지 상관없이 한번만 나와야한다.
- n개의 숫자들을 arr에 차례대로 넣어준다.
- 수열이 오름차순으로 출력되어야하기 때문에 arr을 오름차순으로 정렬하였다.
- go는 index와 n,m을 매개변수로 받는 함수이다.
    - go에서 index가 m보다 크거나 같아지면 return해준다.
        - 이때 temp에 가능한 수열을 저장하고 있어 temp의 값들을 차례대로 StringBuilder에 append해준다.
        - 0부터 n까지 반복하며 해당 값의 visited가 false이면 이번 방문에 사용되지 않은 값이므로 visited가 false일때만 확인한다.
            - arr[i-1]과 arr[i]가 같은 값일 때 visited[i-1]이 false이면 이미 같은 값으로 방문을 한 후라는 의미이므로 continue를 해준다.
            - 그게아니면 temp[index]에 arr[i]를 넣고 visited를 방문 처리하고 인덱스를 증가시킨 재귀함수를 호출한다.
            - 재귀함수 호출 후에는 visited를 false로 해준다.




### 출처
https://www.acmicpc.net/problem/15663