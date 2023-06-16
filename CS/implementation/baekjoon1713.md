## **후보 추천하기**


### ***problem***
월드초등학교 학생회장 후보는 일정 기간 동안 전체 학생의 추천에 의하여 정해진 수만큼 선정된다. 그래서 학교 홈페이지에 추천받은 학생의 사진을 게시할 수 있는 사진틀을 후보의 수만큼 만들었다. 추천받은 학생의 사진을 사진틀에 게시하고 추천받은 횟수를 표시하는 규칙은 다음과 같다.

1. 학생들이 추천을 시작하기 전에 모든 사진틀은 비어있다.
2. 어떤 학생이 특정 학생을 추천하면, 추천받은 학생의 사진이 반드시 사진틀에 게시되어야 한다.
3. 비어있는 사진틀이 없는 경우에는 현재까지 추천 받은 횟수가 가장 적은 학생의 사진을 삭제하고, 그 자리에 새롭게 추천받은 학생의 사진을 게시한다. 이때, 현재까지 추천 받은 횟수가 가장 적은 학생이 두 명 이상일 경우에는 그러한 학생들 중 게시된 지 가장 오래된 사진을 삭제한다.
4. 현재 사진이 게시된 학생이 다른 학생의 추천을 받은 경우에는 추천받은 횟수만 증가시킨다.
5. 사진틀에 게시된 사진이 삭제되는 경우에는 해당 학생이 추천받은 횟수는 0으로 바뀐다.

후보의 수 즉, 사진틀의 개수와 전체 학생의 추천 결과가 추천받은 순서대로 주어졌을 때, 최종 후보가 누구인지 결정하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에는 사진틀의 개수 N이 주어진다. (1 ≤ N ≤ 20) 둘째 줄에는 전체 학생의 총 추천 횟수가 주어지고, 셋째 줄에는 추천받은 학생을 나타내는 번호가 빈 칸을 사이에 두고 추천받은 순서대로 주어진다. 총 추천 횟수는 1,000번 이하이며 학생을 나타내는 번호는 1부터 100까지의 자연수이다.

#### **Output**
사진틀에 사진이 게재된 최종 후보의 학생 번호를 증가하는 순서대로 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main (String[]args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine()); //사진 틀의 갯수
        int M = Integer.parseInt(br.readLine()); //추천 횟수
        Map<Integer,Integer> pic = new LinkedHashMap<>();
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        for(int i=0;i<M;i++){
            int num = Integer.parseInt(st.nextToken());
            //이미 사진틀에 있으면
            if(pic.containsKey(num)){
                pic.put(num,pic.getOrDefault(num,0)+1);
                continue;
            }
            else if(pic.size() >= N){ //사진틀에 없는데 후보 등록을 해야하는 경우
                //가장 오래된 것을 삭제!
                int[] keyList = pic.keySet().stream().mapToInt(Integer::intValue).toArray();
                int min = keyList[0];
                for(Integer key : keyList){
                    if(pic.get(min) > pic.get(key)){
                        min = key;
                    }
                }
                pic.remove(min);
            }
            pic.put(num,pic.getOrDefault(num,0));
        }
        List<Integer> keyList = pic.keySet().stream()
                .collect(Collectors.toList());

        Collections.sort(keyList);
        StringBuilder sb = new StringBuilder();
        for(Integer key : keyList){
            sb.append(key+" ");
        }

        bw.write(sb.toString()+"\n");
        bw.flush();
    }
}
```
- Map을 통해 사진틀에 개시된 후보자의 번호와 추천 횟수를 관리한다.
    - 이때 삽입순서를 보장하기 위해서 HashMao이 아닌 LinkedHashMap을 사용한다.
- 입력받은 후보의 숫자가 map에 있는지 먼저 확인한다.
    - 사진틀에 모두 후보가 개시되어있어도, 이미 추천받은 후보이면 추천숫자를 먼저 올리기 위해서
- 후보가 사진틀에 개시되어있으면 추천숫자를 올린다.
- 후보가 사진틀에 개시되어있지 않으면 사진틀이 비어있는지 확인한다.
    - 비어있지 않으면 먼저 keySet을 통해서 사진틀에 개시되어있는 후보자들을 모두 번호를 가져온다.
    - 그리고 keySet의 key들을 통해 추천횟수를 확인하여 가장 적게 참조되어있는 후보자를 찾는다.
        - 이때 같은 경우는 확인하지 않는다.
        - 가장 오래 개시되어있는 후보의 추천 횟수부터 확인하며 추천 횟수 같은 경우에는 가장 오래된 후보를 삭제해야하기 때문에 같은 경우는 확인하지 않는다.
    - 그리고 새로운 후보를 추가한다.
- 모든 추천을 다 확인한 후에 map에 남아있는 후보들을 keySet을 통해서 set으로 가져온 후 list로 변경한다.
- 그리고 list로 변경한 key들을 오름차순으로 정렬한다. 
 
### **[출처]**
https://www.acmicpc.net/problem/1713