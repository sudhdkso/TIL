## 기능개발


### ***problem***
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

제한 사항
- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.


### ***Solution***

```c++
#include <string>
#include <vector>
#include<iostream>
using namespace std;

vector<int> solution(vector<int> progresses, vector<int> speeds) {
    vector<int> answer={0};
    int time = (99 - progresses[0]) / speeds[0] +1,count=0;
    int t1;
    

    answer[0] = 1;

    for (int i = 1; i < progresses.size(); i++) {
        t1 = (99 - progresses[i]) / speeds[i]+1;
        if (time < t1) {
            answer.push_back(0);
            answer[++count]++;
            time = t1;

        }
        else {
            answer[count]++;
        }

    }
    return answer;
}

```

- time을 progresses 벡터의 0번째의 기능이 완성되는데 걸리는 시간으로 초기화
- 그 다음 반복문은  progresses 벡터의 1부터 반복하는데 이때 걸리는 시간을 t1으로 둠
    1. time과 비교하여 t1이 크면 앞의 기능이 다 완성되고 이번 기능이 완성되는 것이기 때문에 answer의 count를 증가시키고 안의 값을 증가 시킴
    2. time과 비교하여 t1이 작으면 앞의 기능이 다 완성되기 전에 이번 기능이 완성되기 때문에 answer[count]를 1 증가

- 기능이 완성되는데 걸리는 시간은 (99 - progresses[i]) / speeds[i]+1

### 출처
https://programmers.co.kr/learn/courses/30/lessons/42586