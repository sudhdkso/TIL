## **괄호 회전하기**


### **problem**
다음 규칙을 지키는 문자열을 올바른 괄호 문자열이라고 정의합니다.

- `()`, `[]`, `{}` 는 모두 올바른 괄호 문자열입니다.
- 만약 `A`가 올바른 괄호 문자열이라면, `(A)`, `[A]`, `{A}` 도 올바른 괄호 문자열입니다. 예를 들어, `[]` 가 올바른 괄호 문자열이므로, `([])` 도 올바른 괄호 문자열입니다.
- 만약 `A`, `B`가 올바른 괄호 문자열이라면, `AB` 도 올바른 괄호 문자열입니다. 예를 들어, `{}` 와 `([])` 가 올바른 괄호 문자열이므로, `{}([])` 도 올바른 괄호 문자열입니다.
대괄호, 중괄호, 그리고 소괄호로 이루어진 문자열 s가 매개변수로 주어집니다. 이 `s`를 왼쪽으로 x (0 ≤ x < (`s`의 길이)) 칸만큼 회전시켰을 때 `s`가 올바른 괄호 문자열이 되게 하는 x의 개수를 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- `s`의 길이는 1 이상 1,000 이하입니다.

### ***Solution***
``` java
import java.util.Stack;

class Solution {
    public static boolean check(String s){
        Stack<String> stack = new Stack<>();
        int len = s.length();
        for(int i = 0; i < len; i++){
            String st = s.substring(i,i+1);
            if(st.equals("(") || st.equals("{")|| st.equals("[")){
                stack.push(st);
            }
            else {
                if(stack.isEmpty()){
                    return false;
                }
                if(stack.peek().equals("(") && st.equals(")")){
                    stack.pop();
                }
                else if(stack.peek().equals("{") && st.equals("}")){
                    stack.pop();
                }
                else if(stack.peek().equals("[") && st.equals("]")){
                    stack.pop();
                }
            }
        }
        if(stack.isEmpty()) return true;
        return false;
    }
    
    public static String rotateBracket(String s){
        StringBuilder sb = new StringBuilder(s);
        String s1 = s.substring(0,1);
        sb.delete(0,1);
        sb.append(s1);
        return sb.toString();
    }
    
    public int solution(String s) {
        int answer = 0;
        int len = s.length();
        StringBuilder sb = new StringBuilder(s);
        
        for(int i = 0; i < len; i++){
            s = rotateBracket(s);
            if(check(s)){
                answer++;
            }
        }
        return answer;
    }
}
```
- check함수는 문자열 s가 올바른 괄호인지 판별하는 함수이다.
- rotateBracket함수는 왼쪽으로 한칸씩 회전시키는 함수이다.

- 문자열 s를 rotateBracket함수를 통해서 왼쪽으로 한칸 회전시키고 check를 통해 올바른 괄호이면 answer를 증가시킨다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/76502#