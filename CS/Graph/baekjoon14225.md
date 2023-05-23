## **부분수열의 합**


### ***problem***
수열 S가 주어졌을 때, 수열 S의 부분 수열의 합으로 나올 수 없는 가장 작은 자연수를 구하는 프로그램을 작성하시오.

예를 들어, S = [5, 1, 2]인 경우에 1, 2, 3(=1+2), 5, 6(=1+5), 7(=2+5), 8(=1+2+5)을 만들 수 있다. 하지만, 4는 만들 수 없기 때문에 정답은 4이다.


#### **Input**
첫째 줄에 수열 S의 크기 N이 주어진다. (1 ≤ N ≤ 20)

둘째 줄에는 수열 S가 주어진다. S를 이루고있는 수는 100,000보다 작거나 같은 자연수이다.

#### **Output**
첫째 줄에 수열 S의 부분 수열의 합으로 나올 수 없는 가장 작은 자연수를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    public static boolean[] check = new boolean[(20 *100000)+1];
    public static void go(int index,int sum, int[] arr){
        if(index >= arr.length){
            check[sum] = true;
            return;
        }
        go(index+1, sum, arr);
        go(index+1,sum + arr[index], arr);
    }

    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int[] arr = new int[n];
        st = new StringTokenizer(br.readLine(), " ");
        for(int i=0;i<n;i++){
            arr[i] = Integer.parseInt(st.nextToken());
        }
        int index = 0;
        go(0,0,arr);
        while(true){
            if(check[index]){
                index++;
            }
            else{
                break;
            }
        }
        bw.write(index+"\n");
        bw.flush();
    }

}
```
- 재귀를 통해서 수열들의 숫자들의 합을 확인한다.
- check는 덧셈한 값이 부분 수열의 합인지 아닌지 체크하는 boolean타입의 일차원 배열이다.
- index를 통해서 수열의 값을 더하거나 더하지 않거나 하면서 재귀 함수를 호출을 한다.
- index가 수열의 끝까지 같다면 매개변수로 받은 sum을 check에 index로 받아 true해준다.

- 모든 재귀 함수를 호출하였을 때, check배열의 값을 처음부터 확인하며 check가 false일 때 반복문을 break해주고 index를 출력한다.



### 출처
https://www.acmicpc.net/problem/14225