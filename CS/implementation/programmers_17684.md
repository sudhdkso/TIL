## **[3차] 압축**


#### ***problem***
신입사원 어피치는 카카오톡으로 전송되는 메시지를 압축하여 전송 효율을 높이는 업무를 맡게 되었다. 메시지를 압축하더라도 전달되는 정보가 바뀌어서는 안 되므로, 압축 전의 정보를 완벽하게 복원 가능한 무손실 압축 알고리즘을 구현하기로 했다.

어피치는 여러 압축 알고리즘 중에서 성능이 좋고 구현이 간단한 **LZW(Lempel–Ziv–Welch)** 압축을 구현하기로 했다. LZW 압축은 1983년 발표된 알고리즘으로, 이미지 파일 포맷인 GIF 등 다양한 응용에서 사용되었다.

LZW 압축은 다음 과정을 거친다.

1. 길이가 1인 모든 단어를 포함하도록 사전을 초기화한다.
2. 사전에서 현재 입력과 일치하는 가장 긴 문자열 `w`를 찾는다.
3. `w`에 해당하는 사전의 색인 번호를 출력하고, 입력에서 `w`를 제거한다.
4. 입력에서 처리되지 않은 다음 글자가 남아있다면(`c`), `w+c`에 해당하는 단어를 사전에 등록한다.
5. 단계 2로 돌아간다.

압축 알고리즘이 영문 대문자만 처리한다고 할 때, 사전은 다음과 같이 초기화된다. 사전의 색인 번호는 정수값으로 주어지며, 1부터 시작한다고 하자.

|색인 번호|	1	|2	|3	|...	|24	|25	|26|
| --- | --- |---|---|---|---|---|---|
|단어	|A	|B	|C	|...|	X|	Y|	Z|

예를 들어 입력으로 `KAKAO`가 들어온다고 하자.

1. 현재 사전에는 `KAKAO`의 첫 글자 `K`는 등록되어 있으나, 두 번째 글자까지인 `KA`는 없으므로, 첫 글자 `K`에 해당하는 색인 번호 11을 출력하고, 다음 글자인 `A`를 포함한 `KA`를 사전에 27 번째로 등록한다.
2. 두 번째 글자 `A`는 사전에 있으나, 세 번째 글자까지인 `AK`는 사전에 없으므로, `A`의 색인 번호 1을 출력하고, `AK`를 사전에 28 번째로 등록한다.
3. 세 번째 글자에서 시작하는 `KA`가 사전에 있으므로, `KA`에 해당하는 색인 번호 27을 출력하고, 다음 글자 `O`를 포함한 `KAO`를 29 번째로 등록한다.
4. 마지막으로 처리되지 않은 글자 `O`에 해당하는 색인 번호 15를 출력한다.

|현재 입력(w)|	다음 글자(c)|	출력|	사전 추가(w+c)|
|---|---|---|---|
|K	|A	|11|27: KA|
|A	|K	|1|28: AK|
|KA	|O	|27|29: KAO|
|O|		|15|	|

이 과정을 거쳐 다섯 글자의 문장 `KAKAO`가 4개의 색인 번호 [11, 1, 27, 15]로 압축된다.

입력으로 `TOBEORNOTTOBEORTOBEORNOT`가 들어오면 다음과 같이 압축이 진행된다.

|현재 입력(w)|	다음 글자(c)|	출력|	사전 추가(w+c)|
|---|---|---|---|
|T	|O	|20	|27: TO|
|O	|B	|15	|28: OB|
|B	|E	|2	|29: BE|
|E	|O	|5	|30: EO|
|O	|R	|15	|31: OR|
|R	|N	|18	|32: RN|
|N	|O	|14	|33: NO|
|O	|T	|15	|34: OT|
|T	|T	|20	|35: TT|
|TO	|B	|27	|36: TOB|
|BE	|O	|29	|37: BEO|
|OR	|T	|31	|38: ORT|
|TOB|E	|36	|39: TOBE|
|EO	|R	|30	|40: EOR|
|RN|O	|32	|41: RNO|
|OT|		|34	| |

#### **입력형식**
입력으로 영문 대문자로만 이뤄진 문자열 `msg`가 주어진다. `msg`의 길이는 1 글자 이상, 1000 글자 이하이다.

#### **출력형식**
주어진 문자열을 압축한 후의 사전 색인 번호를 배열로 출력하라.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public static ArrayList<String> setting(){
        ArrayList<String> list = new ArrayList<>();
        for(int i=0;i<26;i++){
            list.add(Character.toString('A'+i));
        }
        return list;
    }
    
    public int[] solution(String msg) {
        ArrayList<String> dict = setting();
        ArrayList<Integer> answer = new ArrayList<>();
        int length = msg.length();
        
        for(int i=0;i<length;i++){
            StringBuilder w = new StringBuilder(String.valueOf(msg.charAt(i)));
            if(i >= length-1){
                answer.add(dict.indexOf(w.toString())+1);
                break;
            }
            
            String c = String.valueOf(msg.charAt(i+1));
            
            while(dict.contains(w+c)){
                w.append(c);
                i++;
                if(i>= length-1){
                    break;
                }
                c = String.valueOf(msg.charAt(i+1));
            }
            if(!dict.contains(w+c)){
                dict.add(w+c);
            }
            int index = dict.indexOf(w.toString());
            
            if(index >= 0){
                answer.add(index+1);
            }
        }
        
        return  answer.stream().mapToInt(Integer::intValue).toArray();
    }
}
```
- dict은 A-Z까지 알파벳을 0부터 차례대로 가지고 있는 List이다.
- 매개변수로 받은 msg를 msg의 길이만큼 반복문을 돌린다.
    - 이때 msg의 i번째 알파벳을 w라고 한다.
    - 그리고 i+1번째를 c라고 한다.
    1. 그리고 msg의 i번째 알파벳과 i+1번째 알파벳을 서로 더한 `w+c`가 dict에 존재하는지 확인한다.
    2. dict에 `w+c`가 존재하면 w에 c를 append하고 이전 c의 다음 알파벳을 새로운 c로 둔다.
    3. 그리고 `w+c`가 dict에 없을 때 까지 1번,2번을 차례대로 반복한다.
        - `w+c`가 dict에 없거나 마지막 알파벳까지 모두 살펴본 경우 위의 과정을 멈춘다.

    - `w+c`가 dict에 없는지 확인한다.   
        - 문자열 끝까지 다돌아서 위의 과정을 탈출한 경우 일 수 있기 때문에 확인을 한번 더 한다.
    - `w+c`가 dict에 없으면 dict에 `w+c`를 추가한다.
    -  그리고 dict에사 w의 index를 찾아 answer에 추가한다.
    

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/17684