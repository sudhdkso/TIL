## **빗물**


### ***problem***
2차원 세계에 블록이 쌓여있다. 비가 오면 블록 사이에 빗물이 고인다.

<img src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/14719/1.png"/>

비는 충분히 많이 온다. 고이는 빗물의 총량은 얼마일까?

#### **Input**
첫 번째 줄에는 2차원 세계의 세로 길이 H과 2차원 세계의 가로 길이 W가 주어진다. (1 ≤ H, W ≤ 500)

두 번째 줄에는 블록이 쌓인 높이를 의미하는 0이상 H이하의 정수가 2차원 세계의 맨 왼쪽 위치부터 차례대로 W개 주어진다.

따라서 블록 내부의 빈 공간이 생길 수 없다. 또 2차원 세계의 바닥은 항상 막혀있다고 가정하여도 좋다.

#### **Output**
2차원 세계에서는 한 칸의 용량은 1이다. 고이는 빗물의 총량을 출력하여라.

빗물이 전혀 고이지 않을 경우 0을 출력하여라.

### ***Solution***
``` java
import java.io.*;
import java.util.*;
import java.util.stream.Collectors;

public class Main {

    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int H = Integer.parseInt(st.nextToken());
        int W = Integer.parseInt(st.nextToken());
        int[] arr = new int[W];
        st = new StringTokenizer(br.readLine()," ");
        for(int i=0;i<W;i++){
            arr[i] = Integer.parseInt(st.nextToken());
        }
        int left = arr[0];
        int sum = 0;
        for(int i=1;i<W;i++){
            if(left < arr[i]){
                left = arr[i];
            }
            else if(left > arr[i]){
                int right = arr[i];
                for(int j=i+1;j<W;j++){
                    if(right < arr[j]){
                        right = arr[j];
                    }
                }
                sum += Math.min(right-arr[i],left-arr[i]);
            }
        }
        bw.write(sum+"\n");
        bw.flush();
    }
}
```


### 출처
https://www.acmicpc.net/problem/14719