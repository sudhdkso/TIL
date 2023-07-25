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
- 맨 밑바닥은 막혀있기 때문에 왼쪽과 오른쪽이 블록으로 쌓여있어야 빗물이 흐르지 않는다.
- 첫번째로 가장 먼저 빗물이 고일 수 있는 왼쪽의 높이와 index를 구한다.
    - 왼쪽 부터 계속 블럭이 증가하는 형태이기 때문에, 가장 먼저 왼쪽 벽이 되어줄 수 있는 index를 찾는 것
- 그리고 그 인덱스 다음부터 오른쪽 벽이 되어줄 수 있는 index와 높이를 찾는다.
- 이렇게 오른쪽과 왼쪽을 구했을 때, 그 사이에 고일 수 있는 빗물의 양을 구하여 sum에 더한다.
- 그렇게 왼쪽 index가 오른쪽 끝까지 모두 진행되면 sum을 출력한다.

### **[출처]**
https://www.acmicpc.net/problem/14719