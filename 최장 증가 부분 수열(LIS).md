## 최장 증가 부분 수열(LIS)

최장 증가 부분 수열(LIS : Longest Increasing Subsequence)

주어진 수열에서 오름차순으로 정렬된 가장 긴 부분 수열을 찾는다.

여기서 부분 수열은 **연속적이거나, 유일할 필요는 없다**.

아래의 수열이 존재한다고 가정하면

```
1 5 2 3 1 7
```

여러 증가 부분 수열 중 

첫 번째 인덱스 원소를 기준으로 증가 수열을 하나 찾으면 

아래와 같은 수열이 된다.
```
1 5 7
```

이러한 증가 부분 수열중에서 최장 증가 부분 수열을 찾으면
```
1 2 3 7 
```
로서 길이 4로 최장 증가 부분 수열이 된다.

아래의 문제는 주어진 수열 ```[4 2 3 1 5 6]``` 에서

최장 증가 부분 수열의 길이를 구하는 문제를 풀어본다.

### O(N^2)의 시간복잡도인 동적계획법을 활용한 풀이

동적계획법은 특정 범위까지의 최적해(상위 문제)를 구하기 위하여 

다른 범위까지의 최적해(하위 문제)를 이용하여 효율적으로 해를 구하는 알고리즘 설계 기법이다.

즉, 이전에 구한 값을 기반으로 규칙성을 파악하여 다음 값을 구하는 것이라 생각하면 된다.

우선 LIS 를 구할 DP 테이블을 정의한다.

전체 중에서 가장 긴 증가 부분 수열의 길이를 구하려면 

```각 인덱스별 LIS값을 구하는것을 부분문제```로 생각해 볼 수 있다.

따라서 LIS를 구할 수열의 길이와 동일한 1차원 배열을 생성하고 첫 번째 인덱스부터 접근해본다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[]
```
첫 번째 인덱스를 생각해보면 본인 자신이 우선 증가 부분 수열의 원소가 될 수 있으므로 1개를 누적한다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1]
```
두 번째 인덱스도 우선 본인 자신이 증가부분 수열의 원소가 될 수 있다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 1]
```
여기서 우리는 모든 인덱스의 LIS 값에 대하여 

본인자신이 원소가 되어야 하므로 1로 초기화 할 수 있다는점을 알 수 있다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 1, 1, 1, 1, 1]
```
다시 두 번째 인덱스를 보았을 때,

이전 인덱스의 원소값 보다 작으므로 4을 기준으로 증가 부분 수열에 포함시킬 수 없다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 1, 1, 1, 1, 1]
```
세번째 인덱스를 보았을 때,

이전 인덱스의 원소값들 중 4보단 작지만 2보단 크므로 2를 포함한 증가 부분 수열에 포함시킬 수 있다.

따라서 기준 인덱스 번호의 원소값을 2로 저장한다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 1, 2, 1, 1, 1]
```
네번째 인덱스를 보았을 때,

이전 인덱스의 원소값들 중 기준 원소보다 작은건 존재하지 않으므로 증가 부분 수열이 포함시킬 수 없다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 1, 2, 1, 1, 1]
```
다섯번째 인덱스를 보았을 때,

이전 인덱스의 원소값들 중 4, 2, 3, 1 모두 작으므로 이중 최장 증가 부분 수열이 될 수있는 값을 찾는다.

찾는방법은 지금까지의 인덱스별 증가부분수열은 

각 인덱스까지의 부분수열들 중에서 가장 길게 만들어 졌을 때의 길이를 갱신하고 있으므로

세번째 인덱스까지의 최장 증가 부분 수열의 길이에서 5를 포함시킨 길이가 새로운 최대값이 된다.

여기서 우리는 dp테이블 기준으로 

i번째 인덱스까지의 각 인덱스별 LIS값들중 최대값에 대하여 그 값의 +1한 값을 

dp 테이블에서 i번째 인덱스의 원소로 넣으며 갱신하면 된다는 규칙성을 찾을 수 있다.

```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 1, 2, 1, dp[2]+1 > dp[4] = 3, 1]
```
마지막 6번째 인덱스를 보았을 때,

이전 인덱스의 원소값들 중 다섯번째 인덱스의 원소값이 가장 크고 그기서 6을 추가한것이 

기준 인덱스의 LIS가 되므로 ```dp[5]  = dp[4] + 1 ```이 된다.

```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 1, 2, 1, 3, dp[4]+1 > dp[5] = 4]
```
위의 상황에서 점화식을 도출하면

DP 테이블기준 인덱스별로 1로 초기화해야하고

1번째 원소부터 i번째 원소 전까지 검사하여 

DP테이블 원소값+1 중에서 DP[i]보다 큰 값을 DP[i]에 저장한다.

### JAVA 코드

```java
class Main{
    public static void main(String []args){
        int [] arr = {4,2,3,1,5,6};
        int [] dp = new int[arr.length];
        int max = 1;
        for(int i= 0;i<arr.length;i++){
            dp[i] = 1; // 인덱스별 1로 초기화
            for(int k = 0;k<i;k++){ // 기준 인덱스전까지의 원소값 탐색
                if(dp[k] < dp[i]){ // 증가 부분 수열에 포함시킬수 있는 원소를 찾을경우
                    dp[i] = Math.max(dp[k]+1, dp[i]);
                    // 각 인덱스별 LIS값들에서 자신을 포함한 값을 새로운 LIS로 갱신
                }
            }
            Math.max(max, dp[i]); 
            // 기준 인덱스의 LIS 길이를 구했으므로 최대값을 갱신
        }
        System.out.println(max);
    }
}
```

### O(NlogN)의 시간복잡도인 동적계획법을 활용한 풀이

앞선 O(N^2) 의 시간복잡도 풀이에서 

기준 인덱스 전까지 기준 인덱스의 원소값보다 작은 값을 탐색하는 과정을 줄일 수 있을까? 라는 의문에서 시작한다.

똑같이 DP로 풀기 때문에 DP 테이블 정의를 한다.

현재까지 조사한 앞 원소들에 대하여, 부분 수열을 만들 수 있는 최적의 수의 조합을 저장하자.

예를 들어 1~ 기준원소 전 인덱스까지의 원소에서 i 번째 원소가 부분 수열에 포함 될 수 있는지 

쉽게 파악하기 위한 배열을 만든다

결국 이러한 배열을 끝 인덱스까지 탐색하여 완성시킨다면 

해당 배열의 길이가 곧 LIS가 된다.

이전의 O(n^2)같은 경우는 일반적인 dp문제와 달리 dp 배열에서 최대값을 찾아줘야 하지만

이 방법의 경우 dp 배열의 길이가 LIS가 된다.

우선 첫 번째 원소의 값을 dp에 곧바로 채워준다.

```
Arr
[4, 2, 3, 1, 5, 6]

DP
[4]
```
두 번째 원소의 경우에는, dp 테이블에 있는 원소보다 작으므로 

dp테이블의 첫 번째 인덱스 값을 기준 원소값으로 갱신 해 주어야한다.

왜냐하면 dp테이블은 현재 최적의 LIS를 구성 해야 하기 때문이다.

만약에 갱신해주지 않고 dp테이블에 해당원소를 추가 해 버린다면 

다음 원소들이 증가 부분 수열에 포함 될 수 있는지 재대로 파악 할 수 없게 되고

결국 최장 증가 부분 수열을 이룰 수 없게 된다.

따라서 dp 테이블의 ```원소보다 작거나 같으면 현재 가리키고 있는 dp인덱스의 원소를 기준 원소로 갱신 한다```.

```
Arr
[4, 2, 3, 1, 5, 6]

DP
[2]
```
다음 세번째 원소값을 보면 3으로 dp 테이블의 원소들 보다 크므로 증가 부분 수열에 포함시킬 수 있다.

언제까지나 dp테이블은 다음 원소가 증가 부분 수열의 최적의 원소인지 파악할 수 있어야 한다.

```
Arr
[4, 2, 3, 1, 5, 6]

DP
[2, 3]
```
다음 네번째 원소값을 보면 1으로 dp테이블의 원소들 중에서 가장 작은 원소보다 작으므로

가장 작은 원소와 기준 원소를 교체 해 준다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 3]
```
다음 다섯번째 원소값을 보면 5로 dp테이블의 원소들 중에서 가장 크므로

증가 부분 수열에 포함시킨다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 3, 5]
```
다음 마지막 원소값을 보면 6으로 dp테이블의 원소들 중에서 가장 크므로

증가 부분 수열에 포함시킨다.
```
Arr
[4, 2, 3, 1, 5, 6]

DP
[1, 3, 5, 6]
```

이제 dp 배열의 길이를 구하면 LIS 값 4로 결정된다.

이제 앞선 과정들에게서 규칙성을 찾아서 점화식을 도출한다.

원소를 추가할 때 마다 dp 테이블에서 기준 원소보다 큰 값을 찾는다.

그런 값이 존재한다면 그런 값들 중 가장 작은값과 기준 원소값을 교체 해 주고

그런 값이 존재하지 않는다면, dp 테이블에 해당 기준 원소값을 추가 해 준다.

이 때 중요한 점은 dp 테이블에서 탐색 할 원소들은 ```오름차순으로 정렬```되어 있으므로

```이분탐색```을 통해 빠르게 접근이 가능하다.

```java
import java.util.*;
import java.io.*;

public class Main {
	static int N;
	static int [] arr;
	static StringTokenizer stk;
	public static void main(String[] args) throws Exception{
		BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
		N = Integer.parseInt(bf.readLine());
		arr = new int[N];
		stk = new StringTokenizer(bf.readLine());
		for(int i= 0;i<N;i++) {
			arr[i] = Integer.parseInt(stk.nextToken());
		}
		int [] dp = new int[N+1];
		dp[0] = arr[0];
		
		int LIS = 0;
		for(int i= 1;i<N;i++) {
            // 무조건 lower을 검사하는 것이 아니다.
            // 이미 LIS-1번째 원소보다 arr의 i번째가 더 크다면 그냥 안찾고 뒤에 붙이면 된다.
            if(dp[LIS] < arr[i]){
                dp[++LIS] = arr[i];
            }else{
                int idx = lower(dp,0,LIS, arr[i]);    
                dp[idx] = arr[i];
            }
		}
//		System.out.println(Arrays.toString(dp));
		System.out.println(LIS+1);
	}
	static int lower(int [] arr, int start, int end, int find) {
		while(start < end) {
			int mid = (end+start)/2;
			if(arr[mid] < find) {
				start = mid+1;
			}else {
				end = mid;
			}
		}
		return end;
	}

}
 
```
### LIS의 원소까지 찾아야 할 때

만약 본인이 nlogn 방식으로 최장 증가부분 수열을 구했다면 길이는 문제가 되지 않지만

dp배열을 출력하면 이상한 값이 나오게 된다.
```
input : 
4
1 4 5 3
```
```
output :
1 3 5

answer :
1 4 5
```
왜냐하면 이분탐색을 통해 계속 위치를 강제로 바꾸기 때문에 증가부분수열을 이루는것을 항상 보장하지 못한다.

그래서 증가부분수열을 구하는 dp배열에서 끝나는 것이 아니라

추가적인 배열이 필요하게 된다.

해당 배열은 원본 배열의 원소 인덱스를 기준으로 증가부분 수열을 이루었을 때의 인덱스를 담아야 한다.

결국 해당 배열은 증가부분수열에서의 위치 후보가 담기게 되는데

그렇다고 해서 앞에서부터 증가부분수열에 포함시키게 된다면 반례가 위의 상황과 비슷하게 터질 수 있다.

따라서 뒤에서 부터 즉, 증가부분수열이 크기가 4일 때 3번째 증가부분수열 원소부터 찾아 나서고

스택에 넣어놓고 하나씩 다 빼면 된다.

```java
import java.util.*;
import java.io.*;

public class Boj12015 {
	static int [] arr;
	static int [] v;
	static int N;
	static StringTokenizer stk;
	public static void main(String[] args) throws Exception{
		BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
		N = Integer.parseInt(bf.readLine());
		arr = new int[N];
		int []  dp = new int[N+1];
		stk = new StringTokenizer(bf.readLine());
		for(int i= 0;i<N;i++) {
			arr[i] = Integer.parseInt(stk.nextToken());
		}
		dp[0] = arr[0];
		
		v = new int[N];
		v[0] = 0;
		int LIS = 0;
		for(int i= 1;i<N;i++) {
			if(dp[LIS] < arr[i]) {
				// 현재 dp 마지막번째보다 크다면 그냥 뒤에 붙이면된다.
				dp[++LIS] = arr[i];
				v[i] = LIS;
			}
			else {
				int idx = lower(dp, 0, LIS, arr[i]);
				v[i] = idx;
				dp[idx] = arr[i];
			}
			
		}
		
		
//		System.out.println(Arrays.toString(dp));
//		System.out.println(Arrays.toString(v));
		Stack<Integer> res = new Stack<>();
		// 뒤에서 부터 꺼내야 반례가 없게된다.
		// 현재 v배열에는 각 원소의 증가부분수열에서의 인덱스가 담겨있다.
		// 뒤에서부터 꺼내어서 stack에 담으면 3번째 2번째 1번째 0번째로 들어가고
		// stack에서 isEmpty까지 pop하여 꺼내면 증가부분수열이 된다.
		int t = LIS;
		for(int i= arr.length-1 ;i>-1;i--) {
			if(t == v[i]) {
				res.add(arr[i]);
				--t;
			}
		}
		System.out.println(LIS+1);
		while(!res.isEmpty()) {
			System.out.print(res.pop()+" ");
		}
		
		
	}
	static int lower(int [] arr, int start, int end, int find) {
		while(start < end) {
			int mid = (start+end)/2;
			if(arr[mid] < find) {
				start = mid+1;
			}else {
				end = mid;
			}
		}
		return end;
	}

}

```
### 가장 큰 증가하는 부분수열

수열에서 증가하는 부분수열의 합 중 가장 큰 것을 구해야 한다.

일반적으로 증가부분 수열을 N^2으로 구하는 방법에서 살짝 응용이 들어간 문제이다.

우리가 N^2 의 방식을 할 수 있는지 먼저 파악해야한다.

```java
// dp 배열의 사용도를 정의한다.
// 가장 큰 증가하는 부분수열은 즉, 증가부분수열이면서 새 원소가 들어올 때
// 최대가 될 때만 부분수열에 포함시켜야 한다.
// 그러기 위해서는 모든 원래 N^2의 증가부분수열을 구하는 방법에서 모든 dp값을 1로 초기화했던것 처럼
// 자기 자신의 원소값을 저장 한다.
// 왜냐하면 원소를 증가부분수열에 포함 할지 말지를 결정하는데 있어서 있다.
// 어떤 원소를 이제 큰 증가부분수열에 포함시켜야 할 때
// 해당 원소보다 작은 인덱스에서의 부분수열을 형성한 합들 중
// 자기네들 부분수열에 포함시킨것이 이득인 애들을 갱신해야 한다.
 
import java.util.*;
import java.io.*;

public class Main {
	static long [] arr;
	static long [] dp;
	static int N;
	static int [] v;
	static long max;
	static StringTokenizer stk;
	public static void main(String[] args) throws Exception{
		BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
		max = Long.MIN_VALUE;
		N = Integer.parseInt(bf.readLine());
		stk = new StringTokenizer(bf.readLine());
		arr = new long[N];
		dp = new long[N];
		v = new int[N];
		for(int i= 0;i<N;i++) {
			long value = Long.parseLong(stk.nextToken());
			arr[i] = value;
			dp[i] = arr[i];
		}
		// LIS
		max = dp[0];
		//
		/*
		 * 0  1  2  3  4  5  6  7
		 * 1  9  3  6  6  7  2  4
		 * 1 10  4 10 10 17    
		 * 0  1  1  2  2  3  1  2
		 */
		
		for(int i=1;i<N;i++) {
			for(int j = 0;j<i;j++) {
				if(arr[j] < arr[i]) {
					dp[i] = Math.max(dp[j]+arr[i], dp[i]);
					max = Math.max(max, dp[i]);
				}
			}
		}
		System.out.println(max);

	}

}

```

출처 : 

[[Java]동적 계획법(Dynamic Programming)](https://sskl660.tistory.com/87)

[[Java]최장 증가 부분 수열(LIS, Longest Increasing Subsequence)](https://sskl660.tistory.com/89)

