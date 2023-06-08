## **후보키**


#### ***problem***
프렌즈대학교 컴퓨터공학과 조교인 제이지는 네오 학과장님의 지시로, 학생들의 인적사항을 정리하는 업무를 담당하게 되었다.

그의 학부 시절 프로그래밍 경험을 되살려, 모든 인적사항을 데이터베이스에 넣기로 하였고, 이를 위해 정리를 하던 중에 후보키(Candidate Key)에 대한 고민이 필요하게 되었다.

후보키에 대한 내용이 잘 기억나지 않던 제이지는, 정확한 내용을 파악하기 위해 데이터베이스 관련 서적을 확인하여 아래와 같은 내용을 확인하였다.

- 관계 데이터베이스에서 릴레이션(Relation)의 튜플(Tuple)을 유일하게 식별할 수 있는 속성(Attribute) 또는 속성의 집합 중, 다음 두 성질을 만족하는 것을 후보 키(Candidate Key)라고 한다.
    - 유일성(uniqueness) : 릴레이션에 있는 모든 튜플에 대해 유일하게 식별되어야 한다.
    - 최소성(minimality) : 유일성을 가진 키를 구성하는 속성(Attribute) 중 하나라도 제외하는 경우 유일성이 깨지는 것을 의미한다. 즉, 릴레이션의 모든 튜플을 유일하게 식별하는 데 꼭 필요한 속성들로만 구성되어야 한다.
제이지를 위해, 아래와 같은 학생들의 인적사항이 주어졌을 때, 후보 키의 최대 개수를 구하라.

<img src = "https://grepp-programmers.s3.amazonaws.com/files/production/f1a3a40ede/005eb91e-58e5-4109-9567-deb5e94462e3.jpg" align="center"/>

위의 예를 설명하면, 학생의 인적사항 릴레이션에서 모든 학생은 각자 유일한 "학번"을 가지고 있다. 따라서 "학번"은 릴레이션의 후보 키가 될 수 있다.

그다음 "이름"에 대해서는 같은 이름("apeach")을 사용하는 학생이 있기 때문에, "이름"은 후보 키가 될 수 없다. 그러나, 만약 ["이름", "전공"]을 함께 사용한다면 릴레이션의 모든 튜플을 유일하게 식별 가능하므로 후보 키가 될 수 있게 된다.
물론 ["이름", "전공", "학년"]을 함께 사용해도 릴레이션의 모든 튜플을 유일하게 식별할 수 있지만, 최소성을 만족하지 못하기 때문에 후보 키가 될 수 없다.
따라서, 위의 학생 인적사항의 후보키는 "학번", ["이름", "전공"] 두 개가 된다.

릴레이션을 나타내는 문자열 배열 relation이 매개변수로 주어질 때, 이 릴레이션에서 후보 키의 개수를 return 하도록 solution 함수를 완성하라.

#### **제한사항**
- relation은 2차원 문자열 배열이다.
- relation의 컬럼(column)의 길이는 `1` 이상 `8` 이하이며, 각각의 컬럼은 릴레이션의 속성을 나타낸다.
- relation의 로우(row)의 길이는 `1` 이상 `20` 이하이며, 각각의 로우는 릴레이션의 튜플을 나타낸다.
- relation의 모든 문자열의 길이는 `1` 이상 `8` 이하이며, 알파벳 소문자와 숫자로만 이루어져 있다.
- relation의 모든 튜플은 유일하게 식별 가능하다.(즉, 중복되는 튜플은 없다.)

### ***Solution***
``` java
import java.util.*;

class Solution {
    static List<String> candidateKeyList = new ArrayList<>();
    static Set<String> set = new HashSet<>();
    public int solution(String[][] relation) {
        int answer = 0;
        int size = relation[0].length;

        getAllKeyCases(size,1,"");
        List<String> keyList = new ArrayList<>(set);
        Collections.sort(keyList, (o1,o2) -> Integer.parseInt(o1)-Integer.parseInt(o2));
        for(String key : keyList){
            if(!isMinimality(key)){
                continue;
            }
            if(isCandidateKey(relation, key)){
                answer++;
            }
        }
        return answer;
    }
    private static void getAllKeyCases(int num, int index, String key){

        if(index >= num+1){
            if(key == ""){
                return;
            }
            set.add(key);
            return;
        }
        getAllKeyCases(num,index+1,key+String.valueOf(index));
        getAllKeyCases(num,index+1,key);
    }
    
    private static boolean isCandidateKey(String[][] relation, String key){
        int[] keyList = Arrays.stream(key.split(""))
            .mapToInt(Integer::parseInt)
            .toArray();
        
        List<String> info = new ArrayList<>();
        
        for(int i=0;i<relation.length;i++){
            StringBuilder sb = new StringBuilder();
            for(int j=0;j<keyList.length;j++){
                sb.append(relation[i][keyList[j]-1]);
            }
            if(info.contains(sb.toString())){
                return false;
            }
            info.add(sb.toString());
        }
        candidateKeyList.add(key);
        return true;
        
    }
    
    private static boolean isMinimality(String key){
        String[] keys = key.split("");

        for(String candidateKey : candidateKeyList){
            String s = candidateKey;
            for(int i=0;i<keys.length;i++){                
                s = s.replace(keys[i],"");
            }
            
            if(s.length() == 0){
                return false;
            }
        }
        return true;
    }
}
```
- getAllKeyCases()함수는 만들 수 있는 key조합을 모두 만들어주는 재귀함수이다.
- isCandidateKey()함수는 후보키가 될 수 있는지 판별하는 함수이다.
    1. key에 해당하는 정보를 realtion에서 뽑아서 하나의 문자열로 만든다.
    2. 만든 문자열이 list에 포함되어있는지 확인한다.
    3. 포함되어있으면 false를 return한다.
    4. 포함되어있지 않으면 list에 추가한다.
    5. 모든 realtion에 대해서 확인하면, 해당 키는 후보키가 될 수 있으므려 후보키리스트인 candidateKeyList에 추가하고 true를 return한다.

- isMinimality()함수는 해당키가 최소성을 만족하는지 확인한다.
    1. 확인할 key를 split함수를 통해 배열로 만든다.
    2. 이미 후보키로 인증된 candidateKeyList의 candidateKey를 확인한다.
        - candidateKey는 s에 저장한다.
    3. 이때 key split해서 만든 배열의 값을 s에 replace를 통해서 s에서 제거한다.
    4. s.length()이면 해당 키는 최소성을 만족하지 않으므로 return false해준다.
        - candidateKey는 길이가 작은 것 부터 확인하여 candidateKeyList에 넣었기 때문에 
### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/42890