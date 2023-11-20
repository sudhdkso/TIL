## **구간 합 구하기 5**


### ***problem***
N×N개의 수가 N×N 크기의 표에 채워져&nbsp;있다. (x1, y1)부터 (x2, y2)까지 합을 구하는 프로그램을 작성하시오. (x, y)는 x행 y열을 의미한다.

예를 들어, N = 4이고, 표가&nbsp;아래와 같이 채워져 있는 경우를 살펴보자.

<table style="line-height:20.8px; width:158px">
	<tbody>
		<tr>
			<td style="text-align:center">1</td>
			<td style="text-align:center">2</td>
			<td style="text-align:center">3</td>
			<td style="text-align:center">4</td>
		</tr>
		<tr>
			<td style="text-align:center">2</td>
			<td style="text-align:center">3</td>
			<td style="text-align:center">4</td>
			<td style="text-align:center">5</td>
		</tr>
		<tr>
			<td style="text-align:center">3</td>
			<td style="text-align:center">4</td>
			<td style="text-align:center">5</td>
			<td style="text-align:center">6</td>
		</tr>
		<tr>
			<td style="text-align:center">4</td>
			<td style="text-align:center">5</td>
			<td style="text-align:center">6</td>
			<td style="text-align:center">7</td>
		</tr>
	</tbody>
</table>

여기서 (2, 2)부터 (3, 4)까지 합을 구하면 3+4+5+4+5+6 = 27이고, (4, 4)부터 (4, 4)까지 합을 구하면 7이다.

표에 채워져 있는 수와 합을 구하는 연산이 주어졌을 때, 이를 처리하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 표의 크기 N과 합을 구해야 하는 횟수 M이 주어진다. (1 ≤ N ≤ 1024, 1 ≤ M ≤ 100,000) 둘째 줄부터 N개의 줄에는 표에 채워져 있는 수가 1행부터 차례대로 주어진다. 다음 M개의 줄에는 네 개의 정수 x1, y1, x2, y2 가 주어지며, (x1, y1)부터 (x2, y2)의 합을 구해 출력해야 한다. 표에 채워져 있는 수는 1,000보다 작거나 같은 자연수이다. (x1 ≤ x2, y1 ≤ y2)

#### **Output**
총 M줄에 걸쳐 (x1, y1)부터 (x2, y2)까지 합을 구해 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st= new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        int[][] arr = new int[N+1][N+1];
        for(int i=1;i<=N;i++){
            st = new StringTokenizer(br.readLine()," ");
            for(int j=1;j<=N;j++){
                arr[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        for(int i=1;i<=N;i++){
            for(int j=1;j<=N;j++){
                arr[i][j] += arr[i][j-1];
            }
        }

        for(int i=1;i<=N;i++){
            for(int j=1;j<=N;j++){
                arr[j][i] += arr[j-1][i];
            }
        }

        while(M-- > 0){
            st = new StringTokenizer(br.readLine()," ");
            int x1 = Integer.parseInt(st.nextToken());
            int y1 = Integer.parseInt(st.nextToken());

            int x2 = Integer.parseInt(st.nextToken());
            int y2 = Integer.parseInt(st.nextToken());

            int answer = arr[x2][y2] + arr[x1-1][y1-1] - arr[x1-1][y2] - arr[x2][y1-1];

            bw.write(answer+"\n");
        }
        bw.flush();
    }

}
```
### **문제 풀이**

 
### **[출처]**
https://www.acmicpc.net/problem/11660