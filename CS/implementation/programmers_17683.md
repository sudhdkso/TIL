## **방금그곡**


#### ***problem***
라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

- 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.
- 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.
- 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.
- 음악이 00:00를 넘겨서까지 재생되는 일은 없다.
- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.
- 조건이 일치하는 음악이 없을 때에는 “`(None)`”을 반환한다.

#### **입력형식**
입력으로 네오가 기억한 멜로디를 담은 문자열 `m`과 방송된 곡의 정보를 담고 있는 배열 `musicinfos`가 주어진다.

- `m`은 음 1개 이상 `1439`개 이하로 구성되어 있다.
- `musicinfos`는 `100`개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 '`,`'로 구분된 문자열이다.
    - 음악의 시작 시각과 끝난 시각은 24시간 `HH:MM` 형식이다.
    - 음악 제목은 '`,`' 이외의 출력 가능한 문자로 표현된 길이 `1` 이상 `64` 이하의 문자열이다.
    - 악보 정보는 음 `1`개 이상 `1439`개 이하로 구성되어 있다.

#### **출력형식**
조건과 일치하는 음악 제목을 출력한다.

### ***Solution***
``` java
import java.util.Arrays;
class Solution {
    public static int calcLength(String s1, String s2){
        int start =Integer.parseInt(s1.split(":")[0])*60 + Integer.parseInt(s1.split(":")[1]);
        int end =Integer.parseInt(s2.split(":")[0])*60 + Integer.parseInt(s2.split(":")[1]);
        return end - start;
    }
    
    public static int checkMusicInfo(String m, String musicinfo){
        String[] info = musicinfo.split(",");
        int musicLen = calcLength(info[0],info[1]);
        String musicSlice = convertMusicSheet(info[3]);
        StringBuilder sb = new StringBuilder(musicSlice);
        while(musicLen > sb.length()){
            sb.append(musicSlice);
        }
        String music = sb.toString().substring(0,musicLen);

        if(music.contains(m)){
            return musicLen;
        }
        else return -1;
    }
    
    public static String convertMusicSheet(String m){
        StringBuilder sb = new StringBuilder();
        int length = m.length();
        
        for(int i=0;i<length;i++){
            String c = m.substring(i,i+1);
            if( i+2 <=length && m.substring(i+1,i+2).equals("#")){
                sb.append(c.toLowerCase());
                i++;
            }
            else{
                sb.append(c);
            }
            
        }
        return sb.toString();
    }
    
    public String solution(String m, String[] musicinfos) {
        String answer = "";
        int length = musicinfos.length;
        int max = 0;
        m = convertMusicSheet(m);
        for(int i=0;i<length;i++){
            int result = findMusicInfo(m,musicinfos[i]);

            if(result > max){
                max = result;
                answer = musicinfos[i].split(",")[2];
            }
        }
        return answer == "" ? "(None)" : answer;
    }
}
```
- calcLength()함수는 입력받은 음악의 시작시간과 종료시간을 통해서 재생 시간을 계산하여 반환하는 함수이다.
- checkMusicInfo()함수는 라디오의 음악과 네오가 기억하는 음악을 비교하여, 네오가 기억하는 음악이 맞으면 재생시간을 return하는 함수이다.
    1. 음악의 길이를 구한다.
    2. 라디오의 음악시트를 convertMusicSheet()함수를 통해서 변경한다.
        - C#,D#,E#...을 한번에 체크하기 위해서
    3. 라디오의 음악시트를 재생시간과 길이를 동일하게 만든다.
    4. 재생시간을 동일하게 만든 음악에 네오가 들은 음악이 있으면 재생 시간을 return한다.
    5. 없으면 -1을 return한다.
- convertMusicSheet()함수는 C#,E#..들이 들어오면 음악이 맞는지 확인하기 힘들기 때문에, C# → c, E# → e ..이런식으로 장조음을 소문자로 변경해주는 함수이다.

- 라디오의 음악을 순서대로 네오가 들은 음악과 비교한다.
    - 네오가 들은 음악이 맞으면 max값과 비교하여 answer와 max를 해당 값으로 갱신한다.
- 최종적으로 answer가 ""이면 일치하는 음악이 없는 것이므로 "`(None)`"을 return하고 아니면 answer를 return한다.
### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/17683#