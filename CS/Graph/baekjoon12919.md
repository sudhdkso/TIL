## **A와 B 2**


### ***problem***
수빈이는 A와 B로만 이루어진 영어 단어 존재한다는 사실에 놀랐다. 대표적인 예로 AB (Abdominal의 약자), BAA (양의 울음 소리), AA (용암의 종류), ABBA (스웨덴 팝 그룹)이 있다.

이런 사실에 놀란 수빈이는 간단한 게임을 만들기로 했다. 두 문자열 S와 T가 주어졌을 때, S를 T로 바꾸는 게임이다. 문자열을 바꿀 때는 다음과 같은 두 가지 연산만 가능하다.

- 문자열의 뒤에 A를 추가한다.
- 문자열의 뒤에 B를 추가하고 문자열을 뒤집는다.

주어진 조건을 이용해서 S를 T로 만들 수 있는지 없는지 알아내는 프로그램을 작성하시오. 


#### Input
첫째 줄에 S가 둘째 줄에 T가 주어진다. (1 ≤ S의 길이 ≤ 49, 2 ≤ T의 길이 ≤ 50, S의 길이 < T의 길이)

#### Output
S를 T로 바꿀 수 있으면 1을 없으면 0을 출력한다.

### ***Solution***
``` java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;

public class Main {
    public static int answer = 0;

    public static void dfs(String s, StringBuilder t){
        int length = t.length();
        if(s.length() >= t.length()){
            if(s.equals(t.toString())){
                answer = 1;
            }
            return;
        }
        if(t.charAt(length-1) == 'A'){
            t= t.deleteCharAt(length-1);
            dfs(s,t);
            t.append("A");
        }
        if(t.charAt(0) == 'B'){
            t = reverse(t);
            t = t.deleteCharAt(length-1);
            dfs(s,t);
            t.append("B");
            t = reverse(t);
        }
    }

    public static StringBuilder reverse(StringBuilder s){
        StringBuilder sb = new StringBuilder();
        int length = s.length();
        for(int i = length-1;i>=0;i--){
            sb.append(s.charAt(i));
        }
        return sb;
    }

    public static void main( String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String S = br.readLine();
        String T = br.readLine();

        dfs(S,new StringBuilder(T));

        bw.write(String.valueOf(answer));
        bw.flush();
    }

}
```
- String을 이용해서 S가 T가 되도록 문자열을 더해나가면 시간초과가 일어난다.
- t를 String대신 StringBuilder를 사용하는 이유는 String이 불변객체이기 때문에 문자열을 더하거나 빼는 연산이 문자열의 길이만큼의 시간이 걸리기 때문이다.
- t에서 A를 빼거나 뒤집은 후 B를 뺴주는 두개의 경우로 나누어서 재귀함수를 돌린다.


### 출처
https://www.acmicpc.net/problem/12919
