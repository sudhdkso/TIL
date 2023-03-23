## 나는야 포켓몬 마스터 이다솜


### ***problem***
첫째 줄부터 차례대로 M개의 줄에 각각의 문제에 대한 답을 말해줬으면 좋겠어!!!. 입력으로 숫자가 들어왔다면 그 숫자에 해당하는 포켓몬의 이름을, 문자가 들어왔으면 그 포켓몬의 이름에 해당하는 번호를 출력하면 돼. 그럼 땡큐~


이게 오박사님이 나에게 새로 주시려고 하는 도감이야. 너무 가지고 싶다ㅠㅜ. 꼭 만점을 받아줬으면 좋겠어!! 파이팅!!!

### ***Solution***

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;


public class Main {

    public static boolean isInteger(String s) {
        try {
            Integer.parseInt(s);
            return true;
        } catch (NumberFormatException e) {
            return false;
        }

    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String[] NM = br.readLine().split(" ");

        ArrayList<String> encyclopedia = new ArrayList<>();
        HashMap<String, Integer> numberEncyclopedia = new HashMap<>();
        StringBuilder sb = new StringBuilder();

        int N = Integer.parseInt(NM[0]);
        int M = Integer.parseInt(NM[1]);

        for(int i=0; i<N;i++){
            String s = br.readLine();
            encyclopedia.add(s);
            numberEncyclopedia.put(s, i+1);
        }

        for(int i=0;i<M;i++) {
            String pokemon = br.readLine();

            //숫자 들어오는 경우
            if(isInteger(pokemon)) {
                sb.append(encyclopedia.get(Integer.parseInt(pokemon)-1))
                        .append("\n");
            }
            //문자 들어오는 경우우
           else {
                int index = numberEncyclopedia.get(pokemon);
                sb.append(index)
                        .append("\n");
            }
        }
        bw.write(String.valueOf(sb));
        bw.flush();

    }
}
```
1. 포켓몬 이름을 ArrayList에 저장한다.
2. 포켓몬 번호와 이름을 각각 key와 value로 정하여 HashMap에 저장한다.
3. 들어온 String이 정수로 변환가능한지 isInteger함수로 확인한다.
    -  들어온 String이 정수면 ArrayList의 get함수를 사용하여 index에 해당하는 포켓몬의 이름을 StringBuilder에 append해준다.
    - 들어온 String이 문자면 HashMap에서 get함수를 사용하여 key값을 찾아 해당하는 key값을 StringBuilder에 append해준다.
        


### 출처
https://www.acmicpc.net/problem/1620