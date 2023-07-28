## **베스트앨범**


### ***problem***
스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

#### **제한사항**
- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    private static class Music implements Comparable<Music>{
        int index, play;
        public Music(int index, int play){
            this.index = index;
            this.play = play;
        }
        @Override
        public int compareTo(Music music){
            if(this.play == music.play){
                return this.index-music.index;
            }
            return music.play-this.play;
        }

    }
    
    public int[] solution(String[] genres, int[] plays) {
        int[] answer = {};
        Map<String, Integer> total = new HashMap<>();
        Map<String, PriorityQueue<Music>> playMap = new HashMap<>();
        int len = genres.length;
        
        for(int i=0;i<len;i++){
            String genre = genres[i];
            int play = plays[i];
            total.put(genre, total.getOrDefault(genre, 0) + play);
            if(!playMap.containsKey(genre)){
                PriorityQueue<Music> pq = new PriorityQueue<>();
                pq.offer(new Music(i, play));
                playMap.put(genre, pq);
            }
            else{
                playMap.get(genre).offer(new Music(i,play));
            }
        }
        
        List<String> keySet = new ArrayList<>(total.keySet());
        keySet.sort((o1,o2) -> total.get(o2)-total.get(o1));
        
        List<Integer> musicList = new ArrayList<>();
        for(String key : keySet){
            PriorityQueue<Music> pq = playMap.get(key);
            musicList.add(pq.poll().index);
            if(pq.isEmpty()){
                continue;
            }
            musicList.add(pq.poll().index);
        }
        
        return musicList.stream().mapToInt(Integer::intValue).toArray();
    }
}
```
### **문제 풀이** 
- 장르별 전체 플레이 음악 횟수와 각 음악의 장르별 음악의 번호, 각 음악의 플레이 횟수를 따로 관리해야하므로 map을 두개 활용하였다.

- 장르별 전체 플레이 음악 횟수에 대한 map의 변수명은 total이고, 각 장르별 음악에 대한 정보를 담은 map의 변수명은 playMap이다.
- Music은 음악의 index와 플레이 횟수를 가지는 객체이고, Comparable을 상속하여 compareTo()함수를 문제의 정렬 순서에 맞게 재정의하고 있다.
    - 문제의 정렬 순서는 플레이 횟수가 많은 것 먼저, 플레이 횟수가 같을 때는 index가 작은 것 우선이다.

- total의 경우 일반 map처럼 장르를 Key로 두고, key에 대해서 value에 play를 더하는 방식으로 관리한다.
- playMap의 경우 total과 동일하게 key는 음악 장르이지만 value의 경우 Music객체를 저장하는 PriorityQueue이다.
    - 이 PriorityQueue는 playMap에 장르가 존재하면 이미 있는 priorityQueue에 해당 음악의 index와 play횟수를 Music클래스의 생성자로 생성하여 offer함수를 통해 Queue에 삽입한다.
    - playMap에 해당 장르가 존재하지 않으면 PriorityQueue를 새로 만들어서 해당 PriorityQueue에 해당 음악의 index와 play횟수를 Music클래스의 생성자로 생성하여 playMap에 장르와 함께 넣어준다.

- 그리고 total의 keySet을 List로 선언하고, 그 List를 total의 value에 내림차순으로 정렬한다.
    - 총 플레이 횟수가 많은 장르 부터 정렬
- 그리고 keySet의 값을 for문을 통해 차례로 돌며 해당 key값의 playMap에서 value를 따로 PriorityQueue로 만든다.
- 그렇게 만들어진 PriorityQueue의 가장 앞에 있는 값의 index를 index들을 저장하기 위해 미리 선언된 musicList에 추가한다.
- 그리고 PriorityQueue에 아직 객체가 남아있는지 확인한다.
    - 만약 한곡밖에 없어서, PriorityQueue가 비어있는 상태라면 다른 장르로 넘어가야하기 때문이다.
- 객체가 남아있다면 다음 곡의 index도 musicList에 추가한다.
- musicList는 List이기 때문에 Array로 변환한 후 return한다.


playMap에서 value를 PriorityQueue로 선택한 이유는 새로운 음악이 추가될 때마다 play순으로 정렬되어 후에 간편하게 앨범에 추가할 음악들을 찾기 위해서였다.

하지만 PriorityQueue를 사용하기 위해서는 이미 존재하는 값이 아니면 PriorityQueue를 생성해주고 그 pq에 값을 삽입하고, playMap에 해당 pq를 넣어줘야하는 번거로움이 있었다.

priorityQueue대신 value에 Map을 사용하는 방법도 사용할 수 있다. map의 key는 index이고 value는 play로하여 map을 활용할 수 있을 것이다. 다만 이 방법도 나중에 앨범에 추가할 음악을 찾기 위해서 value별로 따로 정렬하는 keySet을 매번 만들어 줘야 할 것 같다.



### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42579