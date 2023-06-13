## 나무꾼 이다솜

### ***problem***
이다솜은 나무꾼이다. 이다솜은 산신령이 준 금도끼와 은도끼를 이용해서 나무를 열심히 했다. 나무가 끝난 후에 나무들을 쳐다보면서 내가 왜 나무를 하면서 살까 생각하다가, 나무가 엄청나게 값어치가 있다는 것을 알고 나무를 팔러 시장에 가기로 했다.

지역 목재상에서 이다솜의 나무를 사려고 했지만, 너무 길이가 제멋대로여서 나무를 사는 것을 거절을 했다. 목재상의 조건은 일단 팔려고 하는 나무의 길이를 전부 같게 만들어 오라는 것이었다. (나무의 길이는 자연수로) 이다솜은 나무를 하나씩 여러 번 팔려고 했지만, 지역 목재상의 주인은 한 사람에게 평생 단 한번의 판매 기회를 제공하다고 했기 때문에, 이다솜은 근처 작업장으로 가서 나무를 자르기로 했다.

작업장에서 나무를 한 번 자를 때는, C원이 든다. 그리고, 지역 목재상에서 나무를 살 때는, 한 단위에 W원씩 준다. (다른 말로 하면, K개의 나무가 있고, 길이가 L이면, 이다솜은 K*L*W원을 벌 수 있다.)

이다솜이 현재 가지고 있는 나무의 길이가 주어졌을 때, 이다솜이 벌 수 있는 가장 큰 돈을 구하는 프로그램을 작성하시오.


#### **Input**
첫째 줄에 이다솜이 가지고 있는 나무의 개수 N과 나무를 자를 때 드는 비용 C와 나무 한 단위의 가격 W이 주어진다. 둘째 줄부터 총 N개의 줄에 이다솜이 가지고 잇는 나무의 길이가 한 줄에 하나씩 주어진다. N은 50보다 작거나 같은 자연수이고, C와 W는 10,000보다 작거나 같은 자연수이다. 그리고 나무의 길이는 모두 10,000보다 작거나 같은 자연수이다.

#### **Output**
첫째 줄에 이다솜이 벌 수 있는 돈의 최댓값을 출력한다.

### ***Solution***

```java
import java.io.*;
import java.util.*;

public class Main {

    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int N = Integer.parseInt(st.nextToken());
        int C = Integer.parseInt(st.nextToken());
        int W = Integer.parseInt(st.nextToken());
        int[] trees = new int[N];
        for(int i=0;i<N;i++){
            trees[i] = Integer.parseInt(br.readLine());
        }
        Arrays.sort(trees);
        long answer = 0;
        int max = trees[N-1];
        for(int i=1;i<=max;i++){
            long total = 0;
            for(int j=0;j<N;j++){
                if(trees[j] < i) continue;
                int piece = trees[j]/i;
                int cut = trees[j]%i == 0 ? piece-1 : piece;
                int money = piece*i*W - cut*C;
                if(money < 0) continue;
                total += money;

            }
            answer = Math.max(total,answer);
        }

        bw.write(String.valueOf(answer));
        bw.flush();
    }

}
```
- 입력받은 나무의 길이를 정렬한다.
- 그리고 가장 큰 나무의 길이를 max에 저장한다.
- 나무의 길이 i를 [1,max]까지 돌리면서 trees배열 안의 나무들을 i길이로 잘랐을 때의 벌 수 있는 돈을 계산한다.
    - trees[j]가 i길이보다 작으면 자를 수 없기 때문에 넘긴다.
    - trees[j]의 나무를 i길이로 잘랐을 때의 조각(picec)의 갯수는 trees[j]/i이다. 
    - trees[j]의 나무를 i길이로 자른 횟수는 trees[j]가 i로 나누어 떨어지면 조각의 갯수-1이고, 그게 아니면 조각의 갯수이다.
    - 이렇게 했을 때의 벌수 있는 금액을 계산한다.
    - 벌 수 있는 금액이 0보다 작으면 계산하지 않는다.
- 길이가 i인 조각들로 자를 때의 벌어들인 금액과 answer를 비교하여 max값을 갱신한다.
    
### 출처
https://www.acmicpc.net/problem/1421