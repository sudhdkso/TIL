## **좋다**


### ***problem***
N개의 수 중에서 어떤 수가 다른 수 두 개의 합으로 나타낼 수 있다면 그 수를 “좋다(GOOD)”고 한다.

N개의 수가 주어지면 그 중에서 좋은 수의 개수는 몇 개인지 출력하라.

수의 위치가 다르면 값이 같아도 다른 수이다.

#### **Input**
첫째 줄에는 수의 개수 N(1 ≤ N ≤ 2,000), 두 번째 줄에는 i번째 수를 나타내는 Ai가 N개 주어진다. (|Ai| ≤ 1,000,000,000, Ai는 정수)

#### **Output**
좋은 수의 개수를 첫 번째 줄에 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());

        long[] arr = new long[N];

        StringTokenizer st = new StringTokenizer(br.readLine()," ");

        for(int i=0;i<N;i++){
            arr[i] = Long.parseLong(st.nextToken());
        }

        Arrays.sort(arr);

        int answer = 0;
        for(int i=0;i<N;i++){
            long x = arr[i];
            int left = 0, right = N-1;
            while(left < right && right >=0 && left < N){
                long num = arr[left] + arr[right];
                if(num == x){
                    if(left != i && right != i){
                        answer++;
                        break;
                    }
                    else if(left == i){
                        left++;
                    }
                    else if(right == i){
                        right--;
                    }
                }
                else if(num >= x){
                    right--;
                }
                else if (num < x){
                    left ++;
                }
            }
        }
        bw.write(answer+"\n");
        bw.flush();
    }

}
```
### **문제 풀이**

 
### **[출처]**
https://www.acmicpc.net/problem/1253