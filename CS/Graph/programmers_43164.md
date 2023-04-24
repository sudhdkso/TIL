## **여행 경로**


### ***problem***
주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 "ICN" 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

#### **제한사항**
- 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
- 주어진 공항 수는 3개 이상 10,000개 이하입니다.
- tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
- 주어진 항공권은 모두 사용해야 합니다.
- 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
- 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public static boolean[] visited;
    public static HashMap<String, ArrayList<String>> airlines = new HashMap<String, ArrayList<String>>();
    public static StringBuilder sb = new StringBuilder();
    public static String[] str;
    public static String[] solution(String[][] tickets) {
        int length = tickets.length;
        String[] answer;
        visited = new boolean[length];
        str = new String[length];

        Arrays.fill(visited, false);
        for(int i=0;i<length;i++){
            if(!airlines.containsKey(tickets[i][0])){
                airlines.put(tickets[i][0], new ArrayList<>());
            }
            airlines.get(tickets[i][0]).add(tickets[i][1]);
            Collections.sort(airlines.get(tickets[i][0]));
        }

        dfs(0, "ICN", tickets);
        answer = sb.toString().split(" ");
        return answer;
    }

    public static int check (String s1, String s2, String[][] tickets){
        int length = tickets.length;
        for(int i=0;i<length;i++){
            if((tickets[i][0].equals(s1) && tickets[i][1].equals(s2) )&& !visited[i]){
                return i;
            }
        }
        return -1;
    }

    public static void dfs(int index, String start, String[][] tickets){
        if(index > tickets.length-1){
            StringBuilder ss = new StringBuilder();
            ss.append("ICN").append(" ");
            for(int i=0;i<index;i++){
                ss.append(str[i]).append(" ");
            }
            if(sb.toString().isEmpty()){
                sb = ss;
            }
            else {
                sb = sb.compareTo(ss) > 0 ? ss: sb;
            }
            return;
        }
        if(!airlines.containsKey(start)){
            return;
        }

        for(String t: airlines.get(start)){
            int num = check(start, t, tickets);
            if(num < 0){
                continue;
            }

            if(!visited[num]){
                visited[num] = true;
                str[index] = t;
                dfs(++index, t, tickets);
                str[--index] = "";
                visited[num] = false;
            }
        }
        return;
    }
}
```
- visited는 티켓의 사용여부를 boolean타입으로 저장하는 일차원 배열이다.
- str은 dfs를 돌면서 방문한 공항의 이름을 저장하는 String타입의 일차원 배열이다.
- check()는 출발 공항과 도착 공항이름을 매개변수로 받아 해당 티켓의 사용 여부를 확인한 후 티켓의 인덱스를 반환하는 함수이다.
- dfs를 활용하여 공항에 방문가능한 모든 경우의 수를 확인한다.
    - 모든 공항을 방문한 경우
        1.  StringBuilder 타입의 ss에 str를 차례대로 append한다.
        2. 그리고 전역적으로 선언되어있는 StringBuilder타입의 sb가 비어있다면 ss를 sb에 바로 append한다.
        3. sb가 비어있지 않다면 sb와 ss를 비교하여 알파벳 순으로 먼저 위치되어있는 것을 확인하고 ss가 앞서면 sb를 ss로 초기화 하고 그게 아니면 그냥 sb를 넣어준다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/43164