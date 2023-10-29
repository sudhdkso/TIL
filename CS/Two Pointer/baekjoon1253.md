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
- 배열의 두개의 합이 배열에 있는 숫자인지 구하는 투포인터 문제이다.
- 문제는 전반적으로 기본 투포인터와 비슷하다. 하지만 배열의 전체 숫자를 각각 반복문으로 돌리면서 해당 숫자를 만들 수 있는지 투포인터방식을 사용해야한다.
- 그리고 주의해야할 점은 0이 포함될 수 있기 때문에 index처리를 잘해줘야한다. 만약 주어진 배열의 값이 [0,0,1]은 해당 결과값은 0이 나와야한다. 자기 자신을 제외한 값을 이용해서 자기 자신을 만들 수 있는지 확인해야하는데, 단순 투포인터만 적용하면 [0,0]을 가지고 0을 만들 수 있고, [0,1]을 가지고 1을 만들 수 있기 때문에 투포인터가 가리키는 값이 지금 찾는 값의 인덱스와 비교해줘야한다.
 
### **[출처]**
https://www.acmicpc.net/problem/1253