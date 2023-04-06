## 요세푸스 문제

### ***problem***
요세푸스 문제는 다음과 같다.

1번부터 N번까지 N명의 사람이 원을 이루면서 앉아있고, 양의 정수 K(≤ N)가 주어진다. 이제 순서대로 K번째 사람을 제거한다. 한 사람이 제거되면 남은 사람들로 이루어진 원을 따라 이 과정을 계속해 나간다. 이 과정은 N명의 사람이 모두 제거될 때까지 계속된다. 원에서 사람들이 제거되는 순서를 (N, K)-요세푸스 순열이라고 한다. 예를 들어 (7, 3)-요세푸스 순열은 <3, 6, 2, 7, 5, 1, 4>이다.

N과 K가 주어지면 (N, K)-요세푸스 순열을 구하는 프로그램을 작성하시오.

제한 사항
- 첫째 줄에 N과 K가 빈 칸을 사이에 두고 순서대로 주어진다. (1 ≤ K ≤ N ≤ 5,000)

### ***Solution***

```java
public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String[] NK = br.readLine().split(" ");
        int N = Integer.parseInt(NK[0]);
        int K = Integer.parseInt(NK[1]);
        ArrayList<Integer> arr = new ArrayList<>();

        StringBuilder sb = new StringBuilder();

        sb.append("<");

        for(int i=1; i<N+1;i++){
            arr.add(i);
        }
        int index = K-1;

        while(arr.size() > 0) {
            sb.append(arr.get(index));
            if(arr.size() > 1) {
                sb.append(", ");
            }
            arr.remove(index);

            index += K-1;

            if(index >= arr.size() && arr.size() > 0) {
                index %= arr.size();
            }
        }

        sb.append(">");
        bw.write(String.valueOf(sb));
        bw.flush();

    }
}
```
- ArrayList 사용하여 요세푸스 순열에 들어 갈 숫자의 list인 arr을 만든다.
-  arr값을 이동할 변수 index를 선언한다.
    - 변수 index는 K-1로 초기화한다.
- while을 돌면서 arr리스트에 값이 없을 때 까지 아래를 반복한다.
    - arr의 index 값을 sb에 삽입한다.
    - arr에서 index를 삭제한다.
    - 그리고 index에  K-1을 더한다.
        - 이때 index가 arr의 size보다 크면 index를 arr.size()%index한 값을 저장한다.
        

### 출처
https://www.acmicpc.net/problem/1158