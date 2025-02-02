performance.now()로 실행전과 실행 후의 시간차를 비교 </br>

1. 기기 마다 측정이 다를 수 있음
2. 같은 기기 여도 조금씩 다름
3. 아주 빠른 알고리즘일 때 측정 x

## 시간복잡도

: 입력 크기에 대해 어떠한 알고리즘이 실행되는데 걸리는 시간, 주요 로직의 반복 횟수를 중점으로 측정<br/>

### 빅오 표기법

: 시간 복잡도를 표현할 때 사용 <br/>

📌 <b>Constants don't mattter</b>

- O(2n) ➡️ O(n) | O(500) ➡️ O(1) <br/>

📌 <b>Smaller terms don't matter</b>

- O(2n² + 5n) ➡️ O(2n^)

<br/>

📎 <b>시간 복잡도 O(1)</b> <br/>
: 입력 크기와 관계 없이 실행시간이 일정한 경우.실제로 완전히 같지 않아도, 전반적인 추세가 같음

```javascript
function addToUp(n) {
  return (n * (n + 1)) / 2;
}
// 입력 크기와 관계 없이 고정된 연산 3번
```

📎 <b>시간 복잡도 O(n)</b> <br/>
: 입력 크기에 비례하게 실행시간이 증가하는 경우

```javascript
function addToUpLoop(n) {
  let sum = 0;
  for (let i = 1; i <= n; i++) {
    sum += i;
  }
  return sum;
}
// 입력 크기에 따라 연산 횟수가 선형적으로 증가

function countUpAndDown(n) {
  for (let i = 0; i <= n; i++) {
    console.log(i);
  } //O(n)
  for (let j = n; j >= 0; j--) {
    console.log(j);
  } //O(n)
}
// O(2n)➡️ O(n)
```

<br/>
📎 <b>시간 복잡도  O(n²)</b> <br/>
:입력크기 증가시 폭발적으로 증가,제곱 시간(중첩된 반복문)

```javascript
function printPairs(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
      console.log(arr[i], arr[j]);
    }
  }
}
// O(n) * (On)
```

<details>
<summary> 시간 복잡도의 속도비교</summary>
<img width="300px" src="https://miro.medium.com/v2/resize:fit:1400/1*FkQzWqqIMlAHZ_xNrEPKeA.png">
</details>

<br/>

## 공간복잡도

: 프로그램을 실행시켰을 때 필요로 하는 자원 공간의 양. <br/>
input value는 포함하지 않으며, 입력크기에 따라 증가하는 추가적인 공간만 고려 <br/><br/>
📌 boolean, number, undefined, null은 불변공간 <br/>
📌 Strings require O(n) space <br/>
📌 Reference types are generally O(n) (배열길이, 객체의 키값...)
<br/>

📎 <b>공간복잡도 O(1)</b><br/>

```javascript
function sum(arr) {
  let total = 0;
  for (let i = 0; i <= arr.length; i++) {
    total += i;
  }
  return total;
}
// 입력 배열은 별도로 취급, 배열의 크기가 커져도 추가로 필요한 변수의 개수는 변하지 않음
```

📎 <b>공간복잡도 O(n)</b><br/>

```javascript
function linearSpace(n) {
  let arr = new Array(n);
  for (let i = 0; i < n; i++) {
    arr[i] = i;
  }
  return arr;
}
// 입력 크기만큼의 배열 생성
```

📎 <b>공간복잡도 O(n²)</b><br/>

```javascript
function quadraticSpace(n) {
  let matrix = [];
  for (let i = 0; i < n; i++) {
    matrix[i] = new Array(n);
    for (let j = 0; j < n; j++) {
      matrix[i][j] = i * j;
    }
  }
  return matrix;
}
// n x n 2차원 배열 생성
```
