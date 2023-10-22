## **두 수의 합**


### ***problem***
n개의 서로 다른 양의 정수 a1, a2, ..., an으로 이루어진 수열이 있다. ai의 값은 1보다 크거나 같고, 1000000보다 작거나 같은 자연수이다. 자연수 x가 주어졌을 때, ai + aj = x (1 ≤ i < j ≤ n)을 만족하는 (ai, aj)쌍의 수를 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 수열의 크기 n이 주어진다. 다음 줄에는 수열에 포함되는 수가 주어진다. 셋째 줄에는 x가 주어진다. (1 ≤ n ≤ 100000, 1 ≤ x ≤ 2000000)

#### **Output**
문제의 조건을 만족하는 쌍의 개수를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());

        int[] arr = new int[N];

        StringTokenizer st = new StringTokenizer(br.readLine()," ");

        for(int i=0;i<N;i++){
            arr[i] = Integer.parseInt(st.nextToken());
        }

        Arrays.sort(arr);

        int x = Integer.parseInt(br.readLine());

        int left = 0, right = N-1;
        int answer = 0;
        while(left < right && left >= 0 && right >= 0 && left < N && right < N){
            int num = arr[left] + arr[right];
            if(num == x){
                answer++;
            }
            if(num >= x){
                right-=1;
            }
            else if (num < x){
                left += 1;
            }
        }
        bw.write(answer+"\n");
        bw.flush();
    }

}
```
### **문제 풀이**

 
### **[출처]**
https://www.acmicpc.net/problem/3273