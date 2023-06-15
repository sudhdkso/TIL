## **단축키 지정**


### ***problem***
한글 프로그램의 메뉴에는 총 N개의 옵션이 있다. 각 옵션들은 한 개 또는 여러 개의 단어로 옵션의 기능을 설명하여 놓았다. 그리고 우리는 위에서부터 차례대로 각 옵션에 단축키를 의미하는 대표 알파벳을 지정하기로 하였다. 단축키를 지정하는 법은 아래의 순서를 따른다.

1. 먼저 하나의 옵션에 대해 왼쪽에서부터 오른쪽 순서로 단어의 첫 글자가 이미 단축키로 지정되었는지 살펴본다. 만약 단축키로 아직 지정이 안 되어있다면 그 알파벳을 단축키로 지정한다.
2. 만약 모든 단어의 첫 글자가 이미 지정이 되어있다면 왼쪽에서부터 차례대로 알파벳을 보면서 단축키로 지정 안 된 것이 있다면 단축키로 지정한다.
3. 어떠한 것도 단축키로 지정할 수 없다면 그냥 놔두며 대소문자를 구분치 않는다.
4. 위의 규칙을 첫 번째 옵션부터 N번째 옵션까지 차례대로 적용한다.

#### **Input**
첫째 줄에 옵션의 개수 N(1 ≤ N ≤ 30)이 주어진다. 둘째 줄부터 N+1번째 줄까지 각 줄에 옵션을 나타내는 문자열이 입력되는데 하나의 옵션은 5개 이하의 단어로 표현되며, 각 단어 역시 10개 이하의 알파벳으로 표현된다. 단어는 공백 한 칸으로 구분되어져 있다.

#### **Output**
N개의 줄에 각 옵션을 출력하는데 단축키로 지정된 알파벳은 좌우에 [] 괄호를 씌워서 표현한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static Set<String> hotkey = new HashSet<>();
    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());

        String[] s = new String[N];
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<N;i++){
            s[i] = br.readLine();
            sb.append(String.join(" ",findHotKey(s[i]))).append("\n");

        }

        bw.write(sb.toString()+"\n");
        bw.flush();
    }

    private static String[] findHotKey(String word){
        String[] words = word.split(" ");
        if(words.length <= 1){
            int length = words[0].length();
            for(int i=0;i<length;i++){
                String s = word.substring(i,i+1);
                if(!hotkey.contains(s.toUpperCase(Locale.ROOT))){
                    hotkey.add(s.toUpperCase(Locale.ROOT));
                    words[0] = words[0].substring(0,i)+"["+s+"]"+words[0].substring(i+1,words[0].length());
                    return words;
                }
            }
            return words;
        }

        for(int i=0;i<words.length;i++){
            String s = words[i].substring(0,1);
            if(!hotkey.contains(s.toUpperCase(Locale.ROOT))){
                hotkey.add(s.toUpperCase(Locale.ROOT));
                words[i] = "["+s+"]"+words[i].substring(1,words[i].length());
                return words;
            }
        }

        for(int i=0;i<words.length;i++){
            for(int j=0;j<words[i].length();j++){
                String s = words[i].substring(j,j+1);
                if(!hotkey.contains(s.toUpperCase(Locale.ROOT))){
                    hotkey.add(s.toUpperCase(Locale.ROOT));
                    words[i] = words[i].substring(0,j)+"["+s+"]"+words[i].substring(j+1,words[i].length());
                    return words;
                }

            }
        }
        return words;
    }
}
```
- hotkey는 단축키로 지정된 알파벳들을 저장하고 있는 Set이다.
- findHotKey는 word를 입력받아 단축키를 지정하고 단축키로 지정된 알파벳은 좌우에 [] 괄호를 씌워서 return하는 함수이다.
    - word를 ` `공백 단위로 문자열 나누어 단어 단위로 나눈다.
    - 나눈 단어가 1개이면 단어하나의 알파벳 하나하나를 단축키로 지정할 수 있는지 확인한다.
    - 그게 아니면 단어 단위로 맨 앞의 알파벳만 우선적으로 단축키로 지정할 수 있는지 확인한다.
    - 맨 앞의 단어도 모두 단축키로 지정되어있다면 맨앞 단어의 알파벳부터 차례대로 단축키로 지정할 수 있는지 확인한다.
    - 모두 단축키로 지정할 수 없다면 그냥 배열을 return한다.
- findHotKey를 통해 return 받은 String배열을 join을 통해 공백단위로 하나의 문자열로 만들고 만들어진 문자열을 StringBuilder에 append한다. 
### 출처
https://www.acmicpc.net/problem/1283