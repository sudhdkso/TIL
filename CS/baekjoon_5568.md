## 카드 놓기

### ***problem***
상근이는 카드 n(4 ≤ n ≤ 10)장을 바닥에 나란히 놓고 놀고있다. 각 카드에는 1이상 99이하의 정수가 적혀져 있다. 상근이는 이 카드 중에서 k(2 ≤ k ≤ 4)장을 선택하고, 가로로 나란히 정수를 만들기로 했다. 상근이가 만들 수 있는 정수는 모두 몇 가지일까?

예를 들어, 카드가 5장 있고, 카드에 쓰여 있는 수가 1, 2, 3, 13, 21라고 하자. 여기서 3장을 선택해서 정수를 만들려고 한다. 2, 1, 13을 순서대로 나열하면 정수 2113을 만들 수 있다. 또, 21, 1, 3을 순서대로 나열하면 2113을 만들 수 있다. 이렇게 한 정수를 만드는 조합이 여러 가지 일 수 있다.

n장의 카드에 적힌 숫자가 주어졌을 때, 그 중에서 k개를 선택해서 만들 수 있는 정수의 개수를 구하는 프로그램을 작성하시오.



### ***Solution***

```java
import java.io.*;
import java.util.HashSet;

public class Main {
    public static int k;
    public static HashSet<String> hs = new HashSet<>();
    public static String[] str,selected;
    public static boolean[] check;

    public static void go(int n, int count){
        if(count >= k){
            String s = "";
            for(int i=0;i<k;i++){
                s+=selected[i];
            }
            if(!hs.contains(s)) hs.add(s);
            return;
        }

        for(int i=0;i<n; i++){
            if(check[i]) continue;
            check[i] = true;
            selected[count++] = str[i];
            go(n,count);
            count--;
            check[i] = false;
        }
    }
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int n = Integer.parseInt(br.readLine());
        k = Integer.parseInt(br.readLine());
        check = new boolean[n];
        str = new String[n];
        selected = new String[k];

        for(int i=0;i<n;i++) {
            str[i] = br.readLine();
        }
        go(n,0);
        bw.write(String.valueOf(hs.size()));
        bw.flush();
    }
}
```
- 재귀함수로 n개중 k개를 고를 수 있는 경우를 모두 탐색합니다.
- HashSet을 이용하여 지금까지 만든 숫자 조합에 없으면 HashSet에 넣어 줍니다.
- HashSet의 size가 n개중 k개를 골라 중복 없이 만들 수 있는 정수의 개수입니다. 
    
### 출처
https://www.acmicpc.net/problem/5568