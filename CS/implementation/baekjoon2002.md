## **추월**


### ***problem***
대한민국을 비롯한 대부분의 나라에서는 터널 내에서의 차선 변경을 법률로 금하고 있다. 조금만 관찰력이 있는 학생이라면 터널 내부에서는 차선이 파선이 아닌 실선으로 되어 있다는 것을 알고 있을 것이다. 이는 차선을 변경할 수 없음을 말하는 것이고, 따라서 터널 내부에서의 추월은 불가능하다.

소문난 명콤비 경찰 대근이와 영식이가 추월하는 차량을 잡기 위해 한 터널에 투입되었다. 대근이는 터널의 입구에, 영식이는 터널의 출구에 각각 잠복하고, 대근이는 차가 터널에 들어가는 순서대로, 영식이는 차가 터널에서 나오는 순서대로 각각 차량 번호를 적어 두었다.

N개의 차량이 지나간 후, 대근이와 영식이는 자신들이 적어 둔 차량 번호의 목록을 보고, 터널 내부에서 반드시 추월을 했을 것으로 여겨지는 차들이 몇 대 있다는 것을 알게 되었다. 대근이와 영식이를 도와 이를 구하는 프로그램을 작성해 보자.

#### **Input**
입력은 총 2N+1개의 줄로 이루어져 있다. 첫 줄에는 차의 대수 N(1 ≤ N ≤ 1,000)이 주어진다. 둘째 줄부터 N개의 줄에는 대근이가 적은 차량 번호 목록이 주어지고, N+2째 줄부터 N개의 줄에는 영식이가 적은 차량 번호 목록이 주어진다. 각 차량 번호는 6글자 이상 8글자 이하의 문자열로, 영어 대문자('A'-'Z')와 숫자('0'-'9')로만 이루어져 있다.

같은 차량 번호가 두 번 이상 주어지는 경우는 없다.

#### **Output**
첫째 줄에 터널 내부에서 반드시 추월을 했을 것으로 여겨지는 차가 몇 대인지 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());
        List<String> before = new ArrayList<>() ;
        for(int i=0;i<N;i++){
            before.add(br.readLine());
        }
        List<String> after = new ArrayList<>();
        for(int i=0;i<N;i++){
            after.add(br.readLine());
        }
        int count = 0;
        while(!after.isEmpty()){
            String s = after.get(0);
            int index = 0;
            if(!s.equals(before.get(0))){
                index = before.indexOf(s);
                count++;
            }
            before.remove(index);
            after.remove(0);

        }
        bw.write(count+"\n");
        bw.flush();
    }

}
```
- before는 터널에 들어가기 전 차량의 순서를 저장하는 List이다.
- after는 터널에 빠져나오는 차량의 순서를 저장하는 List이다.

- after의 맨앞의 값과 before의 맨앞의 값이 같은지 비교한다.
    - after의 맨앞과 before의 맨앞의 값이 다르면, after의 맨앞 차량이 추월한 차량이다.
        - 따라서 count를 1증가 시키고, before에서도 해당 차량을 삭제하기 위해서 index를 찾는다.
    - 그리고 after의 맨앞값을 삭제하고, before의 index번째 값을 삭제한다.
        - index는 after의 맨앞과 before의 맨앞이 값으면 0이고, 그게 아니면 before에 after값이 같은 index를 가진다.
- after가 비어있을때가지 위의 행동을 반복하며 추월한 차량의 숫자를 체크한다.
- count를 출력한다.
### **[출처]**
https://www.acmicpc.net/problem/2002