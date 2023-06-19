## **주차장**


### ***problem***
시내 주차장은 1부터 N까지 번호가 매겨진 N개의 주차 공간을 가지고 있다. 이 주차장은 매일 아침 모든 주차 공간이 비어 있는 상태에서 영업을 시작하며, 하룻동안 다음과 같은 방식으로 운영된다. 차가 주차장에 도착하면, 주차장 관리인이 비어있는 주차 공간이 있는지를 검사한다. 만일 비어있는 공간이 없으면, 차량을 빈 공간이 생길 때까지 입구에서 기다리게 한다. 만일 빈 주차 공간이 하나만 있거나 또는 빈 주차 공간이 없다가 한 대의 차량이 주차장을 떠나면 곧바로 그 장소에 주차를 하게 한다. 만일 빈 주차 공간이 여러 곳이 있으면, 그 중 번호가 가장 작은 주차 공간에 주차하도록 한다. 만일 주차장에 여러 대의 차량이 도착하면, 일단 도착한 순서대로 입구의 대기장소에서 줄을 서서 기다려야 한다. 대기장소는 큐(queue)와 같이, 먼저 대기한 차량부터 주차한다.

주차료는 주차한 시간이 아닌 차량의 무게에 비례하는 방식으로 책정된다. 주차료는 차랑의 무게에다 주차 공간마다 따로 책정된 단위 무게당 요금을 곱한 가격이다.

주차장 관리원은 오늘 M대의 차량이 주차장을 이용할 것이라는 것을 알고 있다. 또한, 차량들이 들어오고 나가는 순서도 알고 있다.

주차 공간별 요금과 차량들의 무게와 출입 순서가 주어질 때, 오늘 하룻동안 주차장이 벌어들일 총 수입을 계산하는 프로그램을 작성하라.

#### **Input**
반드시 표준 입력으로부터 다음의 데이터를 읽어야 한다.

- 첫 번째 줄에는 정수 N과 M이 빈칸을 사이에 두고 주어진다.
- 그 다음 N개의 줄에는 주차 공간들의 단위 무게당 요금을 나타내는 정수들이 주어진다. 그 중 s번째 줄에는 주차 공간 s의 단위 무게당 요금 Rs가 들어있다.
- 그 다음 M개의 줄에는 차량들의 무게를 나타내는 정수들이 주어진다. 차량들은 1 부터 M 까지 번호로 구분되고, 이 번호는 출입 순서와는 상관없다. 이 M개의 줄 중 k번째 줄에는 차량 k의 무게를 나타내는 정수 Wk가 들어있다.
- 그 다음 2*M 개의 줄에는 차량들의 주차장 출입 순서를 나타내는 정수들이 한 줄에 하나씩 주어진다. 양의 정수 i는 차량 i가 주차장에 들어오는 것을 의미하고, 음의 정수 -i는 차량 i가 주차장에서 나가는 것을 의미한다. 주차장에 들어오지 않은 차량이 주차장에서 나가는 경우는 없다. 1 번부터 M 번까지 모든 차량은 정확하게 한 번씩 주차장에 들어오고, 한 번씩 주차장에서 나간다. 또한 입구에서 대기 중인 차량이 주차를 하지 못하고 나가는 경우는 없다.
- 1 ≤ N ≤ 100 주차 공간의 수
- 1 ≤ M ≤ 2,000 차량의 수
- 1 ≤ Rs ≤ 100 주차 공간 s의 단위 무게당 요금
- 1 ≤ Wk ≤ 10,000 차량 k의 무게

#### **Output**
출력은 반드시 표준 출력으로 하여야 하며, 하나의 줄에 한 개의 정수를 출력한다. 이 정수는 오늘 하룻동안 주차장이 벌어들인 총 수입이다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static int N;
    static int[] P,C,park;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        P = new int[N];
        C = new int[M];

        park = new int[N];

        for(int i=0;i<N;i++){
            P[i] = Integer.parseInt(br.readLine());
        }

        for(int i=0;i<M;i++){
            C[i] = Integer.parseInt(br.readLine());
        }

        Queue<Integer> waiting = new LinkedList<>();
        int money = 0;
        for(int i=0;i<M*2;i++){
            int num = Integer.parseInt(br.readLine());
            if(num > 0){
                if(!check()){
                    waiting.add(num);
                    continue;
                }
                money += parking(num);
            }
            else{
                pullOut(Math.abs(num));
                if(!waiting.isEmpty()){
                    money += parking(waiting.poll());
                }
            }
        }
        bw.write(money +"\n");
        bw.flush();
    }

    private static boolean check(){
        for(int i=0;i<N;i++){
            if(park[i] <= 0){
                return true;
            }
        }
        return false;
    }

    private static int parking(int num){
        for(int i=0;i<N;i++){
            if(park[i] == 0){
                park[i] = num;
                return P[i]*C[num-1];
            }
        }
        return 0;
    }

    private static void pullOut(int num){
        for(int i=0;i<N;i++){
            if(park[i] == num){
                park[i] = 0;
                return;
            }
        }
    }
}
```
- `P`는 주차공간 각각의 비용을 저장하고 있는 int형 배열이다.
- `C`는 주차할 자동차의 무게를 저장하고 있는 int형 배열이다.
- `par`k는 주차공간을 나타내는 int형 배열이다.
    - i번쨰 주자공간에 주차하고 있는 자동차의 번호를 저장한다.
    - 아무것도 저장되어있지 않을때는 0값을 가진다.
- `check()`는 주차공간이 비어있는지 확인하고, 주차공간이 비어있으면 true, 없으면 false를 반환하는 함수이다.
- `parking()`은 주차할 자동차의 번호를 매개변수로 받아 가장 처음 비어있는 주차공간에 해당 번호를 저장하고 해당 `자동차의 무게*주차공간의 비용`, 즉 주차 비용을 return하는 함수이다.
- `pullOut()`은 출차할 자동차의 번호를 매개변수로 받아서 해당 자동차가 주차하고 있는 주차공간을 비워주는 함수이다.
- 자동차들의 상태(num)를 차례로 입력받는다.
        
    ### **주차하는 경우**
    - num이 0보다 크면 자동차를 주차한다는 의미로 
    주차 공간이 있는지 먼저 확인한다.
    - 주차공간이 있으면 `parking()`을 통해 비어있는 주차공간에 자동차의 번호를 저장한다.
    - 비어있지 않으면 `waiting`이라는 Queue에 자동차의 번호를 추가한다.
    ### **출차하는 경우**
    - num이 0보다 작으면 해당 번호의 차량을 `pullOut()`을 통해서 출차시킨다.
    - 출차한 직후에 `waiting`이라는 Queue에 주차를 대기하고있는 차량이 존재하면 해당 차를 `parking()`함수를 통해 주차한다.

    - `parking()`함수를 통해 반환된 값은 money함수에 더한다.
- 최종적으로 벌어들인 수입인 money를 출력한다.

### **[출처]**
https://www.acmicpc.net/problem/5464