## 전화번호 목록


### ***problem***
스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

제한사항
- genres[i]는 고유번호가 i인 노래의 장르입니다.

- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.

- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.

- 장르 종류는 100개 미만입니다.

- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.

- 모든 장르는 재생된 횟수가 다릅니다.

### ***Solution***

```c++
#include <string>
#include <vector>
#include <unordered_map>
using namespace std;

vector<int> solution(vector<string> genres, vector<int> plays) {
    vector<int> answer;
    unordered_map<string, int> musicMax;
    unordered_map<string, unordered_map<int,int>> hash;
    string str; 
    int max = 0, index = -100, maxPlay = 0;

    for (int i = 0; i < genres.size(); i++) {
        musicMax[genres[i]] += plays[i];
        hash[genres[i]].insert({ i,plays[i] });
        
    }

    while (musicMax.size()>0) {
        str = { NULL }; int max = 0; 
        for (auto it : musicMax) {
            if (max < it.second) {
                max = it.second;
                str = it.first;
            }
                
        }

        musicMax.erase(str);
        for (int i = 0; i < 2; i++) {
            index = -100; maxPlay = 0;
            for (auto it : hash[str]) {
                if (maxPlay < it.second) {
                    index = it.first;
                    maxPlay = it.second;
                }

                else if (maxPlay == it.second) {
                    if (index > it.first) {
                        index = it.first;
                    }

                }

            }
            if (index == -100) break;
            answer.push_back(index);
            hash[str].erase(index);
        }
   }

    return answer;
}
```
 
- musicMax는 각 장르별 재생횟수를 더하여 저장한 map

- hash에는 장르를 key로 하고 value에는 map을 저장하였는데 이 map은 노래의 고유번호를 key로 하고, 노래의 재생횟수를 value로 함.

1. 첫번째 반복문에서 입력받은 벡터의 값을 모두 맵에 저장

2. 두번째 반복문은 앨범에 들어갈 노래를 선택하는 반복문
    -  처음에는 정답을 저장하는 answer배열의 사이즈가 4보다 작을때까지 반복하게 하였지만 노래의 개수가 4개이상이 아닐 수 도 있기 때문에 노래의 장르의 갯수와 마찬가지인 musicMax가 0보다 클때 반복

    -  while 안의 첫번째 반복문은 가장 재생횟수가 많은 노래의 장르를 찾는 반복문
        1. str은 노래의 장르를 저장할 string타입의 변수,max는 가장 큰 노래 재생횟수를 저장하는 int타입의 변수
        2. musicMax를 돌며 가장 큰 재생횟수를 가진 노래의 장르를 str에 저장

    - str한 노래 장르는 musicMax에서 삭제, 삭제하지 않으면 처음 저장한 str이 갱신되지 않음

    - while안의 두번째 반복문은 앨범에 저장하는 노래 최대 두개를 찾는 반복문
        - index는 장르 내의 최대 재생횟수를 가진 노래의 고유 번호를 저장하는 int 타입의 변수, maxPlay는 장르 내의 최대 재생횟수를 저장하는 int타입의 변수

        -  최대 재생횟수의 장르를 저장하고 있는 str을 hash의 키 값으로 장르내에서 가장 많이 재생된 노래를 찾는 반복문
            1. if  그 장르 내에서 다시 최대 재생횟수 maxPlay와 고유 노래번호 index를 찾음

            2. else if 장르 내에서 노래 재생횟수가 같은 경우에는 고유번호가 낮은 노래가 저장되어야하기 때문에 index비교

        - if 노래의 고유번호를 반복문을 돌기 전에 -100으로 초기화 해놓았기 때문에 앞의 반복문을 돌일이 없으면 index==-100으로 반복문을 탈출
            - ex) 노래가 0~1개일 경우

        - 노래의 고유번호 index를 answer 벡터에 저장
        
        - 저장한 노래의 index를 이용하여 삭제


### 출처
https://programmers.co.kr/learn/courses/30/lessons/42579