---
title:  "순열 함수 next_permutation(), prev_permutaion()"           # 포스팅 이름
excerpt: "순열 함수 next_permutation(), prev_permutaion() 및 재귀함수 구현"
typora-root-url: ../            # typora에서 이미지 사용시 images에 자동 저장
categories: function              # 카테고리 지정
tag: [cpp]                     # tag: [python, C++] 여러 개의 tag 추가 방식
use_math: true                  # mathjax LaTeX 수식 표현 적용
---

# <span style = 'color: #008000'>순열</span>
순열이란 순서가 정해진 임의의 집합을 다른 순서로 섞는 연산을 말한다.  
주로 n개의 숫자 중에서 r개를 뽑아 나열하는 경우에 사용한다.

경우의 수를 구하는 공식은 아래와 같다.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$_nP_r = \frac{n!}{(n - r)!}$

## <span style = 'color: #008000'>next_permutation(), prev_permutation()</span>

C++에서는 순열을 수행하는 next_permutation(), prev_permutation() 함수가 존재한다.

```c++
bool next_permutation (BidirectionalIterator first, BidirectionalIterator last);
bool prev_permutation (BidirectionalIterator first, BidirectionalIterator last);
```

**next_permutation()**: **오름차순**의 배열을 기반으로 순열을 수행하는 함수  
**prev_permutation()**: **내림차순**의 배열을 기반으로 순열을 수행하는 함수

[first, last) 범위의 요소들로 순열을 진행한다.

두 함수는 다음 순서가 존재하면 true를 반환하고 다음 순서가 없다면 false를 반환한다.  
따라서 while문을 이용해서 순열을 진행할 수 있다.

### <span style = 'color: #008000'>코드 구현</span>

```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> a{ 3, 1, 2 };

    //next_permutation
    cout << "next_permutation\n";
    sort(a.begin(), a.end());

    do {
        for (int i : a) {
            cout << i << " ";
        }
        cout << "\n";
    } while (next_permutation(a.begin(), a.end()));
    
    //prev_permutation
    cout << "\nprev_permutation\n";
    sort(a.begin(), a.end(), greater<int>());
    do {
        for (int i : a) {
            cout << i << " ";
        }
        cout << "\n";
    } while (prev_permutation(a.begin(), a.end()));

    return 0;
}
```

### <span style = 'color: #008000'>실행 결과</span>

```bash
next_permutation
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1

prev_permutation
3 2 1
3 1 2
2 3 1
2 1 3
1 3 2
1 2 3
```

## <span style = 'color: #008000'>재귀를 이용한 순열</span>

next_permutation(), prev_permutation() 함수를 이용하지 않고 재귀 함수로도 구현이 가능하다.

### <span style = 'color: #008000'>구현 원리</span>

1. 현재 위치가 배열의 크기보다 크면 출력하고 리턴한다.
2. 현재 위치부터 배열의 끝까지 순서대로 현재 위치와 자리를 바꾼 후 현재 위치에 고정한다.
3. 다음 위치를 매개변수로 넣어 재귀로 함수를 호출한다.
4. 2에서 고정했던 숫자의 고정을 풀고 바꿨던 자리를 원위치 시킨다.

1은 재귀함수의 탈출 조건(기저 사례)이며  
2 ~ 4에서 현재 위치부터 배열의 끝까지 반복문 및 재귀함수 호출이 진행된다.<br>

간단하게 이해할 수 있도록 말로 설명하자면 모든 자리의 숫자를 고정시켜서 출력하는 것이 목표이며  
이를 위해 앞에서부터 고정되지 않은 숫자들을 순서대로 자리를 바꿔가며 고정시키는 일련의 과정이다.<br>
최하단의 접어둔 디버깅 코드를 보면 더 쉽게 이해가 가능하다.

### <span style = 'color: #008000'>사용 예시</span>

```c++
#include <bits/stdc++.h>
using namespace std;
int a[3] = { 1, 2, 3 };
int n = 3, r = 3;

void permutation(int n, int r, int depth) {
    if (r == depth) {
        for (int i = 0; i < r; i++) {
            cout << a[i] << " ";
        }
        cout << "\n";
        return;
    }
    for (int i = depth; i < n; i++) {
        swap(a[i], a[depth]);
        permutation(n, r, depth + 1);
        swap(a[i], a[depth]);
    }
    return;
}
int main() {
    permutation(n, r, 0);
    return 0;
}
```
### <span style = 'color: #008000'>실행 결과</span>

```bash
1 2 3
1 3 2
2 1 3
2 3 1
3 2 1
3 1 2
```
<br>

<details>
<summary> <span style = 'color: #008000'>디버깅 코드 접기/펼치기</span></summary>
<div markdown="1">

### <span style = 'color: #008000'>이해를 돕기 위한 디버깅 코드</span>

디버깅 코드에서 배열의 자리를 나타낼 때 사용한 'N번째' 라는 표현은 0번째 부터가 아닌 1번째 부터다.

즉 int a[3] = { 100, 200, 300 }; 에서  
1번째 = 100<br>
2번째 = 200<br>
3번째 = 300<br>

재귀함수이므로 좀 더 단계가 직관적으로 보이도록 단계별로 간격을 맞춰서 작성했다.

```c++
#include <bits/stdc++.h>
using namespace std;
int a[3] = { 100, 200, 300 };
int n = 3, r = 3;

void st(int depth) {
	for (int i = 0; i < depth; i++)
		cout << "  ";

	if (depth == 0) {
		cout << "- [고정된 위치 없음] ";
	}
	else {
		cout << "- [" << depth << "번째까지 고정] ";
	}
}

void makePermutation(int n, int r, int depth) {
	if (r == depth) {
		st(depth);
		cout <<  depth + 1 << "번째는 없으므로 출력 후 리턴한다.\n";
		for (int i = 0; i < r; i++) {
			cout << a[i] << " ";
		}
		cout << "\n";
		return;
	}
	st(depth);
	cout <<  depth + 1 << "번째부터 " << n << "번째까지 고정 시작\n";
	for (int i = depth; i < n; i++) {
		st(depth);
		cout <<  depth + 1 << "번째와 " << i + 1 << "번째를 바꾸고 "<< depth + 1 << "번째에 " << a[depth] << "를 고정한다\n";
		swap(a[i], a[depth]);
		makePermutation(n, r, depth + 1);
		st(depth);
		cout <<  i + 1 << "번째와 " << depth + 1 << "번째를 다시 바꾸고 "<< depth + 1 << "번째에 고정을 푼다\n";
		swap(a[i], a[depth]);
	}
	return;
}
int main() {
	makePermutation(n, r, 0);
	return 0;
}
```

### <span style = 'color: #008000'>디버깅 코드 실행 결과</span>

```bash
- [고정된 위치 없음] 1번째부터 3번째까지 고정 시작
- [고정된 위치 없음] 1번째와 1번째를 바꾸고 1번째에 100를 고정한다
  - [1번째까지 고정] 2번째부터 3번째까지 고정 시작
  - [1번째까지 고정] 2번째와 2번째를 바꾸고 2번째에 200를 고정한다
    - [2번째까지 고정] 3번째부터 3번째까지 고정 시작
    - [2번째까지 고정] 3번째와 3번째를 바꾸고 3번째에 300를 고정한다
      - [3번째까지 고정] 4번째는 없으므로 출력 후 리턴한다.
100 200 300
    - [2번째까지 고정] 3번째와 3번째를 다시 바꾸고 3번째에 고정을 푼다
  - [1번째까지 고정] 2번째와 2번째를 다시 바꾸고 2번째에 고정을 푼다
  - [1번째까지 고정] 2번째와 3번째를 바꾸고 2번째에 200를 고정한다
    - [2번째까지 고정] 3번째부터 3번째까지 고정 시작
    - [2번째까지 고정] 3번째와 3번째를 바꾸고 3번째에 200를 고정한다
      - [3번째까지 고정] 4번째는 없으므로 출력 후 리턴한다.
100 300 200
    - [2번째까지 고정] 3번째와 3번째를 다시 바꾸고 3번째에 고정을 푼다
  - [1번째까지 고정] 3번째와 2번째를 다시 바꾸고 2번째에 고정을 푼다
- [고정된 위치 없음] 1번째와 1번째를 다시 바꾸고 1번째에 고정을 푼다
- [고정된 위치 없음] 1번째와 2번째를 바꾸고 1번째에 100를 고정한다
  - [1번째까지 고정] 2번째부터 3번째까지 고정 시작
  - [1번째까지 고정] 2번째와 2번째를 바꾸고 2번째에 100를 고정한다
    - [2번째까지 고정] 3번째부터 3번째까지 고정 시작
    - [2번째까지 고정] 3번째와 3번째를 바꾸고 3번째에 300를 고정한다
      - [3번째까지 고정] 4번째는 없으므로 출력 후 리턴한다.
200 100 300
    - [2번째까지 고정] 3번째와 3번째를 다시 바꾸고 3번째에 고정을 푼다
  - [1번째까지 고정] 2번째와 2번째를 다시 바꾸고 2번째에 고정을 푼다
  - [1번째까지 고정] 2번째와 3번째를 바꾸고 2번째에 100를 고정한다
    - [2번째까지 고정] 3번째부터 3번째까지 고정 시작
    - [2번째까지 고정] 3번째와 3번째를 바꾸고 3번째에 100를 고정한다
      - [3번째까지 고정] 4번째는 없으므로 출력 후 리턴한다.
200 300 100
    - [2번째까지 고정] 3번째와 3번째를 다시 바꾸고 3번째에 고정을 푼다
  - [1번째까지 고정] 3번째와 2번째를 다시 바꾸고 2번째에 고정을 푼다
- [고정된 위치 없음] 2번째와 1번째를 다시 바꾸고 1번째에 고정을 푼다
- [고정된 위치 없음] 1번째와 3번째를 바꾸고 1번째에 100를 고정한다
  - [1번째까지 고정] 2번째부터 3번째까지 고정 시작
  - [1번째까지 고정] 2번째와 2번째를 바꾸고 2번째에 200를 고정한다
    - [2번째까지 고정] 3번째부터 3번째까지 고정 시작
    - [2번째까지 고정] 3번째와 3번째를 바꾸고 3번째에 100를 고정한다
      - [3번째까지 고정] 4번째는 없으므로 출력 후 리턴한다.
300 200 100
    - [2번째까지 고정] 3번째와 3번째를 다시 바꾸고 3번째에 고정을 푼다
  - [1번째까지 고정] 2번째와 2번째를 다시 바꾸고 2번째에 고정을 푼다
  - [1번째까지 고정] 2번째와 3번째를 바꾸고 2번째에 200를 고정한다
    - [2번째까지 고정] 3번째부터 3번째까지 고정 시작
    - [2번째까지 고정] 3번째와 3번째를 바꾸고 3번째에 200를 고정한다
      - [3번째까지 고정] 4번째는 없으므로 출력 후 리턴한다.
300 100 200
    - [2번째까지 고정] 3번째와 3번째를 다시 바꾸고 3번째에 고정을 푼다
  - [1번째까지 고정] 3번째와 2번째를 다시 바꾸고 2번째에 고정을 푼다
- [고정된 위치 없음] 3번째와 1번째를 다시 바꾸고 1번째에 고정을 푼다
```
</div>
</details>