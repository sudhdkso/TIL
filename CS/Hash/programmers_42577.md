## 전화번호 목록


### ***problem***
전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

- 구조대 : 119
- 박준영 : 97 674 223
- 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.


### ***Solution***

```c++
#include <string>
#include <vector>
#include<unordered_map>
#include<iostream>
using namespace std;

bool solution(vector<string> phone_book) {
    bool answer = true;
    unordered_map<string, int> hash;
    for (int i = 0; i < phone_book.size(); i++) {
        hash.insert({ phone_book[i],1 });
    }

    for (int i = 0; i < phone_book.size(); i++) {
        for (int j = 0; j < phone_book[i].length(); j++) {
            if (hash.find(phone_book[i].substr(0, j)) != hash.end()) {
                answer = false;
                return answer;
            }
        }
    }
    return answer;
}

```
1. 전화번호를 hash에 저장
    - hash의 함수를 사용하기 위함으로 value는 그냥 1로 저장

2. 전화번호가 다른 전화번호의 접두어인지 체크
    -  if (hash.find(phone_book[i].substr(0, j)) != hash.end())
        - 전화번호는string이기 때문에 substr함수를 이용하여 0~j까지 돌며 전화번호를 자름

        - 잘라진 전화번호와 다른 키 값들을 비교하기 위해서 unordered_map의 find함수를 이용

        - 이때 값이 존재하면 hash.end()를 가리키지 않음
        


### 출처
https://programmers.co.kr/learn/courses/30/lessons/42577