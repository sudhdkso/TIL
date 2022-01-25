## 다리를 지나는 트럭



### ***problem***
트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.

예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

제한 조건
- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.


### ***Solution***

```c++
#include <string>
#include <vector>
#include<queue>
#include<iostream>
using namespace std;

int solution(int bridge_length, int weight, vector<int> truck_weights) {
	int answer = 0, count = 0;
	int sum = 0;
	queue<int> q;
	while (1) {

		if (count > truck_weights.size() - 1)
			break;
            
		if (sum + truck_weights[count] <= weight) {
			if (q.size() < bridge_length) {
				sum += truck_weights[count];
				q.push(truck_weights[count]); count++;
				answer++;
			}
			else {
				sum -= q.front();
				q.pop();
			}

		}
		else {
			if (q.size() < bridge_length) {
				q.push(0);
				answer++;
			}
			else {
				sum -= q.front();
				q.pop();
			}
		}

	}

	return answer + bridge_length;
}
```

- sum은 다리를 지나가는 트럭의 총 무게를 저장하는 변수
- answer는 모든 트럭이 다리를 지나가는데 걸리는 시간을 저장하는 변수
- q는 다리에 올라간 트럭을 저장하는 큐


### 출처
https://programmers.co.kr/learn/courses/30/lessons/42583