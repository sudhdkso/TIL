## **한 줄로 서기**


### ***problem***
N명의 사람들은 매일 아침 한 줄로 선다. 이 사람들은 자리를 마음대로 서지 못하고 오민식의 지시대로 선다.

어느 날 사람들은 오민식이 사람들이 줄 서는 위치를 기록해 놓는다는 것을 알았다. 그리고 아침에 자기가 기록해 놓은 것과 사람들이 줄을 선 위치가 맞는지 확인한다.

사람들은 자기보다 큰 사람이 왼쪽에 몇 명 있었는지만을 기억한다. N명의 사람이 있고, 사람들의 키는 1부터 N까지 모두 다르다.

각 사람들이 기억하는 정보가 주어질 때, 줄을 어떻게 서야 하는지 출력하는 프로그램을 작성하시오.
#### **Input**
첫째 줄에 사람의 수 N이 주어진다. N은 10보다 작거나 같은 자연수이다. 둘째 줄에는 키가 1인 사람부터 차례대로 자기보다 키가 큰 사람이 왼쪽에 몇 명이 있었는지 주어진다. i번째 수는 0보다 크거나 같고, N-i보다 작거나 같다. i는 0부터 시작한다.

#### **Output**
첫째 줄에 줄을 선 순서대로 키를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());

        int[] arr = new int[N];

        StringTokenizer st = new StringTokenizer(br.readLine(), " ");

        for(int i=0;i<N;i++){
            arr[i] = Integer.parseInt(st.nextToken());
        }
        int[] answer = new int[N];
        for(int i=0;i<N;i++){
            int num = arr[i];
            int count = 0;
            for(int j=0;j<N;j++){
                if(answer[j] > 0) continue;
                if(count >= num){
                    answer[j] = i+1;
                    break;
                }
                count++;
            }
        }
        String str = Arrays.toString(answer);
        bw.write(str.substring(1,str.length()-1).replace(",",""));
        bw.flush();
    }

}
```
- 키가 1인 사람부터 N인 사람까지 차례대로 자신의 왼쪽에 키가 큰 사람을 입력받는다.
- answer배열은 올바른 순서를 저장하는 배열이다.
- 그리고 키(i)가 1인 사람부터 N인 사람까지 순차적으로 자신의 순서를 찾는다.
    - 앞에서부터 answer배열을 확인하며 answer가 0이면 count를 증가시킨다.
        - answer가 0인 경우에는 아직 사람이 배치가 되지 않은 것이고, 그 빈칸에는 지금 배치할 사람보다 키가 더 큰 사람이 줄을 설 수 있는 순서이다. 
    - 빈칸의 갯수(count)가 자신의 왼쪽의 큰 사람의 갯수와 같으면 거기가 키가 arr[i]인 사람의 자리이므로 answer[j] = i+1이다.

### 출처
https://www.acmicpc.net/problem/1138