## **불량 사용자**


### ***problem***
개발팀 내에서 이벤트 개발을 담당하고 있는 "무지"는 최근 진행된 카카오이모티콘 이벤트에 비정상적인 방법으로 당첨을 시도한 응모자들을 발견하였습니다. 이런 응모자들을 따로 모아 불량 사용자라는 이름으로 목록을 만들어서 당첨 처리 시 제외하도록 이벤트 당첨자 담당자인 "프로도" 에게 전달하려고 합니다. 이 때 개인정보 보호을 위해 사용자 아이디 중 일부 문자를 '*' 문자로 가려서 전달했습니다. 가리고자 하는 문자 하나에 '*' 문자 하나를 사용하였고 아이디 당 최소 하나 이상의 '*' 문자를 사용하였습니다.
"무지"와 "프로도"는 불량 사용자 목록에 매핑된 응모자 아이디를 제재 아이디 라고 부르기로 하였습니다.

예를 들어, 이벤트에 응모한 전체 사용자 아이디 목록이 다음과 같다면
<table class="table">
        <thead><tr>
<th>응모자 아이디</th>
</tr>
</thead>
        <tbody><tr>
<td>frodo</td>
</tr>
<tr>
<td>fradi</td>
</tr>
<tr>
<td>crodo</td>
</tr>
<tr>
<td>abc123</td>
</tr>
<tr>
<td>frodoc</td>
</tr>
</tbody>
      </table>

다음과 같이 불량 사용자 아이디 목록이 전달된 경우,
<table class="table">
        <thead><tr>
<th>불량 사용자</th>
</tr>
</thead>
        <tbody><tr>
<td>fr*d*</td>
</tr>
<tr>
<td>abc1**</td>
</tr>
</tbody>
      </table>
불량 사용자에 매핑되어 당첨에서 제외되어야 야 할 제재 아이디 목록은 다음과 같이 두 가지 경우가 있을 수 있습니다.
<table class="table">
        <thead><tr>
<th>제재 아이디</th>
</tr>
</thead>
        <tbody><tr>
<td>frodo</td>
</tr>
<tr>
<td>abc123</td>
</tr>
</tbody>
      </table>
<table class="table">
        <thead><tr>
<th>제재 아이디</th>
</tr>
</thead>
        <tbody><tr>
<td>fradi</td>
</tr>
<tr>
<td>abc123</td>
</tr>
</tbody>
      </table>
이벤트 응모자 아이디 목록이 담긴 배열 user_id와 불량 사용자 아이디 목록이 담긴 배열 banned_id가 매개변수로 주어질 때, 당첨에서 제외되어야 할 제재 아이디 목록은 몇가지 경우의 수가 가능한 지 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- user_id 배열의 크기는 1 이상 8 이하입니다.
- user_id 배열 각 원소들의 값은 길이가 1 이상 8 이하인 문자열입니다.
    - 응모한 사용자 아이디들은 서로 중복되지 않습니다.
    - 응모한 사용자 아이디는 알파벳 소문자와 숫자로만으로 구성되어 있습니다.
- banned_id 배열의 크기는 1 이상 user_id 배열의 크기 이하입니다.
- banned_id 배열 각 원소들의 값은 길이가 1 이상 8 이하인 문자열입니다.
    - 불량 사용자 아이디는 알파벳 소문자와 숫자, 가리기 위한 문자 '*' 로만 이루어져 있습니다.
    - 불량 사용자 아이디는 '*' 문자를 하나 이상 포함하고 있습니다.
    - 불량 사용자 아이디 하나는 응모자 아이디 중 하나에 해당하고 같은 응모자 아이디가 중복해서 제재 아이디 목록에 들어가는 경우는 없습니다.
- 제재 아이디 목록들을 구했을 때 아이디들이 나열된 순서와 관계없이 아이디 목록의 내용이 동일하다면 같은 것으로 처리하여 하나로 세면 됩니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    static List<List<String>> list = new ArrayList<>();
    static List<String> arr = new ArrayList<>();
    static boolean[] visited, bVisited;
    static int[] temp;
    public int solution(String[] user_id, String[] banned_id) {
        int answer = 0;
        int length = banned_id.length;
        for(int i=0;i<length;i++){
            int count = 0;
            for(int j=0;j<user_id.length;j++){
                //불량 사용자의 id와 응모자의 id의 길이가 같지 않으면 pass
                if(user_id[j].length() != banned_id[i].length() || arr.contains(user_id[j])){
                    continue;
                }
                if(check(user_id[j].split(""),banned_id[i].split(""))){
                    arr.add(user_id[j]);                      
                }
            }
        }
        Collections.sort(arr);
        Arrays.sort(banned_id);
        visited = new boolean[arr.size()];
        bVisited = new boolean[banned_id.length];
        temp = new int[banned_id.length];
        dfs(0,0,banned_id);
        return list.size();
    }
    // s1 : userId s2 : bannedId
    private static boolean check(String[] s1, String[] s2){
        int length = s1.length;
        StringBuilder sb1 = new StringBuilder();
        StringBuilder sb2 = new StringBuilder();
        if(s1.length != s2.length) return false;
        for(int i=0;i<length;i++){
            if(!s2[i].equals("*")){
                sb1.append(s1[i]);
                sb2.append(s2[i]);
            }
        }
        return sb1.toString().equals(sb2.toString());
    }
   
    private static void dfs(int index,int count, String[] banned_id){
        if(count >= banned_id.length){
            List<String> temp = new ArrayList<>();
            for(int i=0;i<visited.length;i++){
                if(temp.contains(arr.get(i))) continue;
                if(visited[i]){
                    temp.add(arr.get(i));
                }
            }
            Collections.sort(temp);
            if(!list.contains(temp)){
                list.add(temp);
                
            }
            return;
        }
        
        for(int i=index;i<banned_id.length;i++){
            if(bVisited[i]) continue;
            for(int j=0;j<arr.size();j++){
                if(visited[j] || arr.get(j).length() !=banned_id[i].length()){
                    continue;
                }
                if(check(arr.get(j).split(""),banned_id[i].split(""))){
                    bVisited[i] = true;
                    visited[j] = true;
                    dfs(index+1,count+1,banned_id);
                    visited[j] = false;
                    bVisited[i] = false;
                }
            }
            
        }
    }
}
```
### **문제 풀이**
- 5번 테스트케이스에서 시간초과가 발생한다..

### [출처]
https://school.programmers.co.kr/learn/courses/30/lessons/64064