## **오픈채팅방**


### ***problem***
카카오톡 오픈채팅방에서는 친구가 아닌 사람들과 대화를 할 수 있는데, 본래 닉네임이 아닌 가상의 닉네임을 사용하여 채팅방에 들어갈 수 있다.

신입사원인 김크루는 카카오톡 오픈 채팅방을 개설한 사람을 위해, 다양한 사람들이 들어오고, 나가는 것을 지켜볼 수 있는 관리자창을 만들기로 했다. 채팅방에 누군가 들어오면 다음 메시지가 출력된다.

"[닉네임]님이 들어왔습니다."

채팅방에서 누군가 나가면 다음 메시지가 출력된다.

"[닉네임]님이 나갔습니다."

채팅방에서 닉네임을 변경하는 방법은 다음과 같이 두 가지이다.

- 채팅방을 나간 후, 새로운 닉네임으로 다시 들어간다.
- 채팅방에서 닉네임을 변경한다.

닉네임을 변경할 때는 기존에 채팅방에 출력되어 있던 메시지의 닉네임도 전부 변경된다.

예를 들어, 채팅방에 "Muzi"와 "Prodo"라는 닉네임을 사용하는 사람이 순서대로 들어오면 채팅방에는 다음과 같이 메시지가 출력된다.

"Muzi님이 들어왔습니다."
"Prodo님이 들어왔습니다."

채팅방에 있던 사람이 나가면 채팅방에는 다음과 같이 메시지가 남는다.

"Muzi님이 들어왔습니다."
"Prodo님이 들어왔습니다."
"Muzi님이 나갔습니다."

Muzi가 나간후 다시 들어올 때, Prodo 라는 닉네임으로 들어올 경우 기존에 채팅방에 남아있던 Muzi도 Prodo로 다음과 같이 변경된다.

"Prodo님이 들어왔습니다."
"Prodo님이 들어왔습니다."
"Prodo님이 나갔습니다."
"Prodo님이 들어왔습니다."

채팅방은 중복 닉네임을 허용하기 때문에, 현재 채팅방에는 Prodo라는 닉네임을 사용하는 사람이 두 명이 있다. 이제, 채팅방에 두 번째로 들어왔던 Prodo가 Ryan으로 닉네임을 변경하면 채팅방 메시지는 다음과 같이 변경된다.

"Prodo님이 들어왔습니다."
"Ryan님이 들어왔습니다."
"Prodo님이 나갔습니다."
"Prodo님이 들어왔습니다."

채팅방에 들어오고 나가거나, 닉네임을 변경한 기록이 담긴 문자열 배열 record가 매개변수로 주어질 때, 모든 기록이 처리된 후, 최종적으로 방을 개설한 사람이 보게 되는 메시지를 문자열 배열 형태로 return 하도록 solution 함수를 완성하라.

### **제한사항**
- record는 다음과 같은 문자열이 담긴 배열이며, 길이는 `1` 이상 `100,000` 이하이다.
- 다음은 record에 담긴 문자열에 대한 설명이다.
    - 모든 유저는 [유저 아이디]로 구분한다.
    - [유저 아이디] 사용자가 [닉네임]으로 채팅방에 입장 - "Enter [유저 아이디] [닉네임]" (ex. "Enter uid1234 Muzi")
    - [유저 아이디] 사용자가 채팅방에서 퇴장 - "Leave [유저 아이디]" (ex. "Leave uid1234")
    - [유저 아이디] 사용자가 닉네임을 [닉네임]으로 변경 - "Change [유저 아이디] [닉네임]" (ex. "Change uid1234 Muzi")
    - 첫 단어는 Enter, Leave, Change 중 하나이다.
    - 각 단어는 공백으로 구분되어 있으며, 알파벳 대문자, 소문자, 숫자로만 이루어져있다.
    - 유저 아이디와 닉네임은 알파벳 대문자, 소문자를 구별한다.
    - 유저 아이디와 닉네임의 길이는 1 이상 10 이하이다.
    - 채팅방에서 나간 유저가 닉네임을 변경하는 등 잘못 된 입력은 주어지지 않는다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    static Map<String,String> map = new HashMap<>();
    public String[] solution(String[] record) {
        int n = record.length;
        int count = getChanteNameRecord(record, n);
        String[] answer = new String[n-count];
        int index = 0;
        for(int i=0;i<n;i++){
            String[] s = record[i].split(" ");
            if(s[0].equals("Change")) continue;
            if(s[0].equals("Enter")){
                answer[index] = map.get(s[1])+"님이 들어왔습니다.";
            }
            else if(s[0].equals("Leave")){
                answer[index] = map.get(s[1])+"님이 나갔습니다.";
            }
            index++;
        }
        
        return answer;
    }
    
    private static int getChanteNameRecord(String[] record, int n){
        int count = 0;
        for(int i=0;i<n;i++){
            String[] s = record[i].split(" ");
            if(s[0].equals("Enter")){
                map.put(s[1],s[2]);  
            }
            else if(s[0].equals("Change")){
                map.put(s[1],s[2]);  
                count++;
            }
        }
        return count;
    }
}
```
### **문제 풀이** 
- 채팅방에 들어온 후 닉네임이 바뀌면, 그전에 들어왔다는 안내 메세지의 닉네임도 함께 바뀌어야하기 때문에, 먼저 닉네임의 최종 변경 사항을 반영하여 저장하는 map을 사용해야겠다고 생각했다.

- map은 uid를 key값으로 사용자가 지정한 닉네임이 value가 되는 형태이다.

- getChanteNameRecord()함수는 record와 record갯수이 n을 매개변수로 받아 map에 사용자의 닉네임 최종 변경 사항을 저장해주는 함수이다.
    - 반환값은 int인데 최종적으로 닉네임 변경 이력 이외에는 안내 메세지를 출력해야하기 때문에, `Change`커맨드의 갯수를 세어 return한다.
- 그리고 solution에서는 map을 통해 최종 변경된 닉네임을 `Enter`와 `Leave`일 때 메세지를 출력할 수 있도록 하였다.
- 이때 index를 별도로 둔 이유는 n개의 record 중 `Change`에 대한 이력도 존재하기 때문에 `Enter`와 `Leave`일때만 안내 메시지를 저장하기 위해서 별도의 index를 두었다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42888?language=java