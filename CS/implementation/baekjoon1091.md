## **카드 섞기**


### ***problem***
지민이는 카지노의 딜러이고, 지금 3명의 플레이어(0, 1, 2)가 있다. 이 게임은 N개의 카드를 이용한다. (0 ~ N-1번)

일단 지민이는 카드를 몇 번 섞은 다음에, 그것을 플레이어들에게 나누어 준다. 0번째 위치에 있던 카드가 플레이어 0에게 가고, 1번째 위치에 있던 카드는 플레이어 1에게 가고, 2번째 위치에 있던 카드는 플레이어 2에게 가고, 3번째 위치에 있던 카드는 플레이어 0에게 가고, 이런식으로 카드를 나누어 준다. 하지만, 지민이는 약간 사기를 치려고 한다.

지민이는 처음에 카드를 섞기 전에 카드의 순서를 알고 있고, 이 정보를 이용해 각 카드가 특정한 플레이어에게 보내지게 할 것이다.

카드를 한 번 섞을 때는 주어진 방법을 이용해서만 섞을 수 있고, 이 방법은 길이가 N인 수열 S로 주어진다. 카드를 한 번 섞고 나면 i번째 위치에 있던 카드는 S[i]번째 위치로 이동하게 된다.

각 카드가 어떤 플레이어에게 가야 하는지에 대한 정보는 길이가 N인 수열 P로 주어진다. 맨 처음 i번째 위치에 있던 카드를 최종적으로 플레이어 P[i] 에게 보내야한다.

지민이가 목적을 달성하기 위해 필요한 카드 섞는 횟수의 최솟값을 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 N이 주어진다. N은 3보다 크거나 같고, 48보다 작거나 같은 3의 배수이다.

둘째 줄에 길이가 N인 수열 P가 주어진다. 수열 P에 있는 수는 0, 1, 2 중 하나이다.

셋째 줄에 길이가 N인 수열 S가 주어진다. 수열 S에 있는 수는 모두 N-1보다 작거나 같은 자연수 또는 0이고 중복되지 않는다.

#### **Output**
첫째 줄에 몇 번 섞어야 하는지 출력한다. 만약, 섞어도 섞어도 카드를 해당하는 플레이어에게 줄 수 없다면, -1을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static int N;
    static int[] S,P, cards;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        S = new int[N]; //카드 섞는 방법 i번째 카드가 s[i]에 위치
        P = new int[N]; //
        cards = new int[N];
        Arrays.setAll(cards,i -> i);
        st = new StringTokenizer(br.readLine(), " ");
        for(int i=0;i<N;i++){
            P[i] = Integer.parseInt(st.nextToken());
        }

        st = new StringTokenizer(br.readLine(), " ");
        for(int i=0;i<N;i++){
            S[i] = Integer.parseInt(st.nextToken());
        }
        int index = -1;
        for(int i=0;i<200_000;i++){
            if(checkOrder()){
                index = i;
                break;
            }
            ShuffleCards();
        }
        bw.write(index+"\n");
        bw.flush();
    }
    private static boolean checkOrder(){
        for(int i=0;i<N;i++){
            if(P[cards[i]] != i%3){
                return false;
            }
        }
        return true;
    }
    private static void ShuffleCards(){
        int[] temp = Arrays.copyOf(cards,N);
        for(int i=0;i<N;i++){
            cards[S[i]] = temp[i];
        }
    }
}
```



### **[출처]**
https://www.acmicpc.net/problem/1091