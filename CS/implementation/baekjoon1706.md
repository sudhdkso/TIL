## **크로스 워드**


### ***problem***
동혁이는 크로스워드 퍼즐을 좋아한다. R×C 크기의 크로스워드 퍼즐을 생각해 보자. 이 퍼즐은 R×C 크기의 표로 이루어지는데, 퍼즐을 다 풀면 금지된 칸을 제외하고는 각 칸에 알파벳이 하나씩 적혀 있게 된다. 아래는 R = 5, C = 5 인 경우 다 푼 퍼즐의 한 예이다. 검은 칸은 금지된 칸이다.

<img align= "center" src="https://www.acmicpc.net/JudgeOnline/upload/201005/cross.PNG" />

세로 또는 가로로 연속되어 있고, 더 이상 확장될 수 없는 낱말이 퍼즐 내에 존재하는 단어가 된다. 위의 퍼즐과 같은 경우, 가로 낱말은 good, an, messy, it, late의 5개가 있고, 세로 낱말은 game, one, sit, byte의 4개가 있다. 이 중 사전식 순으로 가장 앞서 있는 낱말은 an이다.

다 푼 퍼즐이 주어졌을 때, 퍼즐 내에 존재하는 모든 낱말 중 사전식 순으로 가장 앞서 있는 낱말을 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에는 퍼즐의 R과 C가 빈 칸을 사이에 두고 주어진다. (2 ≤ R, C ≤ 20) 이어서 R개의 줄에 걸쳐 다 푼 퍼즐이 주어진다. 각 줄은 C개의 알파벳 소문자 또는 금지된 칸을 나타내는 #로 이루어진다. 낱말이 하나 이상 있는 입력만 주어진다.

#### **Output**
첫째 줄에 사전식 순으로 가장 앞서 있는 낱말을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    static List<String> dict = new ArrayList<>();
    private static void searchCrossWord(int R, int C, char[][] word){
        //가로부터 확인
        for(int i=0;i<R;i++){
            StringBuilder sb = new StringBuilder();
            for(int j=0;j<C;j++){
                if(word[i][j] == '#'){
                    if(sb.length() > 1){
                        dict.add(sb.toString());
                    }
                    sb = new StringBuilder();
                    continue;
                }
                sb.append(word[i][j]);
            }
            if(sb.length() > 1){
                dict.add(sb.toString());
            }
        }

        for(int i=0;i<C;i++){
            StringBuilder sb = new StringBuilder();
            for(int j=0;j<R;j++){
                if(word[j][i] == '#'){
                    if(sb.length() > 1){
                        dict.add(sb.toString());
                    }
                    sb = new StringBuilder();
                    continue;
                }
                sb.append(word[j][i]);
            }
            if(sb.length() > 1){
                dict.add(sb.toString());
            }
        }
    }
    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int R = Integer.parseInt(st.nextToken());
        int C = Integer.parseInt(st.nextToken());

        char[][] word = new char[R][C];

        for(int i=0;i<R;i++){
            word[i] = br.readLine().toCharArray();
        }
        searchCrossWord(R,C,word);
        Collections.sort(dict);
        bw.write(dict.get(0)+"\n");
        bw.flush();
    }
}
```
- **`dict`** 는 cross word를 찾아 저장하는 List이다.
- **`searchCrossWord`** 는 퍼즐의 crossWord를 모두 찾아 dict에 넣어주는 함수이다.
    - cross word를 찾기 위해 퍼즐을 가로부터 차례대로 찾는다.
    - 가로로 먼저 cross word를 찾기 위해 word[0~R][0~C]을 차례대로 탐색하여 알파벳들을 StringBuilder에 append한다.
    - 이때 word[i][j]가 #인 경우에는 StringBuilder에 저장되어있는 문자열이 1보다 크면 dict에 넣어준다.
    - 그리고 StringBuilder를 초기화한다.
    - 세로도 가로와 동일하게 word[j~R][i~C]로 변경하여 세로로 cross word를 탐색한다.

- 사전순으로 가장 앞에 있는 것을 출력해야하기 때문에 dict를 정렬한다.
- 그리고 dict의 0번째 값을 출력한다. 
### **[출처]**
https://www.acmicpc.net/problem/1706