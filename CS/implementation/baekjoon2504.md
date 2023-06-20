## **괄호의 값**


### ***problem***
4개의 기호 ‘(’, ‘)’, ‘[’, ‘]’를 이용해서 만들어지는 괄호열 중에서 올바른 괄호열이란 다음과 같이 정의된다.

1. 한 쌍의 괄호로만 이루어진 ‘()’와 ‘[]’는 올바른 괄호열이다.
2. 만일 X가 올바른 괄호열이면 ‘(X)’이나 ‘[X]’도 모두 올바른 괄호열이 된다.
3. X와 Y 모두 올바른 괄호열이라면 이들을 결합한 XY도 올바른 괄호열이 된다.

예를 들어 ‘(()[[]])’나 ‘(())[][]’ 는 올바른 괄호열이지만 ‘([)]’ 나 ‘(()()[]’ 은 모두 올바른 괄호열이 아니다. 우리는 어떤 올바른 괄호열 X에 대하여 그 괄호열의 값(괄호값)을 아래와 같이 정의하고 값(X)로 표시한다.

1. ‘()’ 인 괄호열의 값은 2이다.
2. ‘[]’ 인 괄호열의 값은 3이다.
3. ‘(X)’ 의 괄호값은 2×값(X) 으로 계산된다.
4. ‘[X]’ 의 괄호값은 3×값(X) 으로 계산된다.
5. 올바른 괄호열 X와 Y가 결합된 XY의 괄호값은 값(XY)= 값(X)+값(Y) 로 계산된다.

예를 들어 ‘(()[[]])([])’ 의 괄호값을 구해보자. ‘()[[]]’ 의 괄호값이 2 + 3×3=11 이므로 ‘(()[[]])’의 괄호값은 2×11=22 이다. 그리고 ‘([])’의 값은 2×3=6 이므로 전체 괄호열의 값은 22 + 6 = 28 이다.

여러분이 풀어야 할 문제는 주어진 괄호열을 읽고 그 괄호값을 앞에서 정의한대로 계산하여 출력하는 것이다.

#### **Input**
첫째 줄에 괄호열을 나타내는 문자열(스트링)이 주어진다. 단 그 길이는 1 이상, 30 이하이다.

#### **Output**
첫째 줄에 그 괄호열의 값을 나타내는 정수를 출력한다. 만일 입력이 올바르지 못한 괄호열이면 반드시 0을 출력해야 한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    private static int calcBracket(String[] bracket){
        int len = bracket.length;
        Stack<String> s = new Stack<>();
        int temp = 1;
        int result = 0;
        for(int i=0;i<len;i++){
            if(bracket[i].equals("(") || bracket[i].equals("[")){
                temp *= bracket[i].equals("(") ? 2 : 3;
                s.push(bracket[i]);
            }
            else{
                if(s.isEmpty()){
                    result = 0;
                    break;
                }
                if(s.peek().equals("(") && bracket[i].equals(")")){
                    if(bracket[i-1].equals("("))
                        result += temp;
                    temp /=2;
                    s.pop();
                }
                else if(s.peek().equals("[") && bracket[i].equals("]")){
                    if(bracket[i-1].equals("["))
                        result += temp;
                    temp /=3;
                    s.pop();
                }
            }
        }
        if(!s.isEmpty()){
            result = 0;
        }
        return result;
    }
    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String[] bracket = br.readLine().split("");
        bw.write(calcBracket(bracket)+"\n");
        bw.flush();
    }
}
```
- calcBracket은 괄호의 값을 계산하여 return해주는 함수이다.
    - bracket은 입력받은 괄호들을 배열로 가진다.
    - bracket의 값이 `(`거나 `[`일때는 stack에 넣는다.
        - 이때 `(`일때는 temp에 2를 곱하고, `[`일 때는 temp에 3을 곱한다.
    - bracket의 값이 `)`거나 `]`일때는 stack의 가장 앞의 값이 짝을 이루는 괄호인지 확인한다.
    - 이때 stack이 비어있으면 올바르지 못한 괄호임으로  result의 값은 0으로 저장하고 반복문을 즉시 종료한다.
    - 이때 bracket[i-1]을 확인한다.
        - bracket[i]가 `)`일때 bracket[i-1]이 `(`이라는 의미는 닫히는 괄호가 가장 내부의 괄호라는 것이므로 지금까지 저장했던 temp의 값을 result에 더한다.
        - 그리고 bracket[i]가 `)`이면 temp를 2로 나누고 bracket[i]가 `]`이면 temp를 3으로 나눈다.
            - 내부 괄호들을 하나씩 정리하기 위함
### **[출처]**
https://www.acmicpc.net/problem/2504