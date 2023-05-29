## **2018 KAKAO BLIND RECRUITMENT - 캐시**


### ***problem***
지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다.
이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.
어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.


#### **입력 형식**
- 캐시 크기(`cacheSize`)와 도시이름 배열(`cities`)을 입력받는다.
- `cacheSize`는 정수이며, 범위는 0 ≦ `cacheSize` ≦ 30 이다.
- `cities`는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
- 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다. 도시 이름은 최대 20자로 이루어져 있다.
#### **출력 형식**
- 입력된 도시이름 배열을 순서대로 처리할 때, "총 실행시간"을 출력한다.

#### **조건**
- 캐시 교체 알고리즘은 `LRU`(Least Recently Used)를 사용한다.
- `cache hit`일 경우 실행시간은 1이다.
- `cache miss`일 경우 실행시간은 5이다.
### ***Solution***
``` java
import java.util.*;
class Solution {
    public int solution(int cacheSize, String[] cities) {
        int answer = 0;
        Queue<String> cache = new LinkedList<>();
        int len = cities.length;
        for(int i=0;i<len;i++){
            String city = cities[i].toUpperCase();
            if(cache.contains(city)){
                cache.remove(city);
                cache.offer(city);
                answer+=1;
            }
            else{
                if(cacheSize > 0){
                    if(cache.size() >= cacheSize){
                        cache.poll();
                    }
                    cache.offer(city);
                }
                answer+=5;

            }
        }
        return answer;
    }
}
```
- Least Recently Used 캐시 교체 알고리즘은 가장 오랫동안 사용되지 않은 캐시를 교체하는 알고리즘이다.

- Queue를 통해 LRU를 구현하였다.
- 반복문을 통해 cities의 값을 하나씩 가져온다
- 이때 가져온 값을 city에 대문자로 변경해서 저장한다.(대소문자 상관없기 때문에)
- 그리고 city의 값이 Queue에 존재하는지 확인한다.
    - Queue에 존재하면 CacheHit이기 때문에 answer에 1을 더한다.
    - 그리고 최근에 사용했기 때문에 Queue에 원래있던 city를 삭제한다.
    - 그리고 city값을 Queue의 마지막에 넣는다.
- city가 Queue에 존재하지 않으면 먼저 cacheSize를 확인한다.
    - cacheSize가 0보다 클때만 Queue를 활용할 수 있기 때문에 cacheSize가 0보다 큰지 확인한다.
    - 0보다 크면 cacheSize와 Queue의 size를 비교한다.
        - cacheSize와 Queue의 size가 같으면 가장 오랫동안 참조가 되지 않은 Queue의 가장 맨 앞을 삭제한다.
            - poll()해준다.
    - 그리고 Queue에 city값을 가장 마지막에 추가한다.
    - 마지막으로 CacheMiss이기 때문에 answer에 5를 더한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/17680#