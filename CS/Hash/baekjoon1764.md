## 나는야 포켓몬 마스터 이다솜


### ***problem***
김진영이 듣도 못한 사람의 명단과, 보도 못한 사람의 명단이 주어질 때, 듣도 보도 못한 사람의 명단을 구하는 프로그램을 작성하시오.

### ***Solution***

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;


public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String[] NM = br.readLine().split(" ");
        int N = Integer.parseInt(NM[0]);
        int M = Integer.parseInt(NM[1]);
        HashSet<String> notListen = new HashSet<>(); 

        ArrayList<String> notListenAndLook = new ArrayList<>(); 

        for(int i=0; i<N; i++) {
            notListen.add(br.readLine());
        }

        for(int i=0; i<M; i++) {
            String notLook = br.readLine();

            if(!notListen.contains(notLook)) {
                continue;
            }
            notListenAndLook.add(notLook);
        }

        Collections.sort(notListenAndLook);

        bw.write(String.valueOf(notListenAndLook.size())+"\n");

        for(int i=0; i<notListenAndLook.size(); i++) {
            bw.write(String.valueOf(notListenAndLook.get(i))+"\n");
        }


        bw.flush();

    }
}
```
1. notListen은 듣도 못한 사람을 저장하는 HashSet이다.
2. notListenAndLook은 듣도 보도 못한 사람을 저장하는 ArrayList이다.
3. N은 듣도 보도 못한 사람의 수이다.
4. M은 보도 못한 사람의 수이다.
5. 듣도 못한 사람을 notListen이라는 HashSet에 모두 저장한다.
5. 보도 못한 사람을 입력받으면서 notListen에 포함되어 있는지 확인한다.
    - 포함되어있으면 notListenAndLook이라는 ArrayList에 추가한다.
        


### 출처
https://www.acmicpc.net/problem/1764