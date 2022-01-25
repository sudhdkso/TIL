## 주식가격


### ***problem***
초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

제한사항
- prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.


### ***Solution***

```c++
#include <string>
#include <vector>
#include<queue>
#include<iostream>
using namespace std;

vector<int> solution(vector<int> prices) {
    vector<int> answer;
    queue<int> q;
    int count=0;
    while (answer.size() < prices.size()) {
        count = 0;
        for (int i = answer.size(); i < prices.size(); i++) {
            count++;
            if(q.empty())
                q.push(prices[i]);
            else if (q.front() <= prices[i]) {
                q.push(prices[i]);
            }
            else {
                break;
            }

            //cout << q.back() << " ";
        }
        answer.push_back(count-1);

        while (!q.empty())
            q.pop();
        cout << endl;

    }
   
    

    
    return answer;
}
```

### 출처
https://programmers.co.kr/learn/courses/30/lessons/42584