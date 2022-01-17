## 프로그래머스 완주하지 못한 선수

### ***problem***
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.


### ***Solution***

```c++
#include <string>
#include <vector>
#include<iostream>
#include <unordered_map>
using namespace std;

string solution(vector<string> participant, vector<string> completion) {
	string answer = "";
	unordered_map<string, int> hash;
	
	for (int i = 0; i < completion.size(); i++) {
		if (hash.find(completion[i]) == hash.end()) { 
			hash.insert({ completion[i], 1 });
		}
		else 
			hash[completion[i]]++;
	}
	
	for (int i = 0; i < participant.size(); i++) {
		hash[participant[i]]--;
		if (hash[participant[i]] < 0) {
			answer = participant[i];
			break;
		}
	}

	return answer;
}


```
1. 완주자를 hash에 저장
    - if (hash.find(completion[i]) == hash.end()) 
        - unordered_map의 find함수는 hash의 키 값을 찾을 수 없으면 find는 end를 가리킴

        - hash안에 같은 이름의 완주가 없다는 의미로 completion[i]를 키값으로 insert하여 추가
    - else
        - 해시안에 completion[i]와 같은 이름의 완주가 있다는 의미로 value값만 +1증가 시킴

2. 완주자를 저장한 hash를 돌며 참가자와 비교
    - hash[participant[i]]--
        - 존재하지 않는 key에 대한 value는 기본값 , int면 0
    - if (hash[participant[i]] < 0)
        - 미완주자는 한명이기 때문에, hash[participant[i]]가 음수가 되는 경우는 단 한번
        - 그 값을 answer에 저장한 후 break


→ 완주자 목록에 없는 참가자를 찾아는 방법
### 출처
https://programmers.co.kr/learn/courses/30/lessons/42576