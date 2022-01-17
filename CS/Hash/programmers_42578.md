## 전화번호 목록


### ***problem***
스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.함수를 작성해주세요.


### ***Solution***

```c++
#include<iostream>
#include<vector>
#include<unordered_map>
using namespace std;

int solution(vector<vector<string>> clothes) {
    int answer = 1;
    unordered_map<string, int> hash;
    vector<string> clo;

    for (int i = 0; i < clothes.size(); i++) {
        if (hash.find(clothes[i][1]) == hash.end()) {
            hash.insert({ clothes[i][1],1});
            clo.push_back(clothes[i][1]);
        }

        else
            hash[clothes[i][1]]++;
    }

            
    for (int i = 0; i < hash.size(); i++) {
        answer *= hash[clo[i]] + 1;
    }

    return answer-1;
}
```
1. 옷 부위 종류를 키 값으로 해시에 저장, 똑같은 옷 종류가 들어오면 value를 +1
2. 벡터 clo에 옷의 부위 종류를 저장
3. 옷을 입을 경우의 수 계산
    - answer *= hash[clo[i]] + 1;
    - 옷의 부위 종류+1(그 부위를 안쓰는 경우) 한 값을 다 곱한다.
4. 옷을 무조건 하나는 착용해야 하므로 모두 안쓰는 경우를 뺀  answer-1을 return    


### 출처
https://programmers.co.kr/learn/courses/30/lessons/42578