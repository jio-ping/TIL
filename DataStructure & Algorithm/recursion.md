## 재귀(Recursion)

: 함수 스스로 자신을 호출해 문제를 해결하는 기법<br/>
📌 <b>구성요소</b></br>

- 재귀 단계(동일한 함수 호출): 함수가 자신을 호출하는 부분
- 종료조건 (base case): 재귀 호출을 멈추는 조건

📌 <b>장점</b></br>

- 코드의 간결성: 복잡한 문제 단순히 표현 가능
- 문제의 분할 : 작은 부분으로 나누어 해셜할 수 있음

📌 <b>유의할 점</b></br>

- 종료조건 명시 : 재귀호출을 멈추는 기저사례를 명확히 정의해아함
- 재귀는 항상 효율적인 것은 아님, 성능 고려 필요
- 스택오버플로우 방지

<hr/>

### pure recursion

: 함수 직접 자신을 호출하느 형태

```javascript
function collectOddValues(arr) {
  let newArr = [];
  if (arr.length === 0) return newArr;
  if (arr[0] % 2 !== 0) {
    newArr.push(arr[0]);
  }
  newArr = newArr.concat(collectOddValues(arr.slice(1)));
  return newArr;
}
```

📌 <b>tip</b></br>

- 배열 사용해 헬퍼메소드 없이 재귀솔루션 작성시 <b>slice, spread,concat</b> 배열 복사 operator 사용
- 문자열 : <b>substring, slice</b>
- 객체: <b>Obeject.assign, spread연산자</b>
  </br>

---

### helper method recursion

: 헬퍼함수를 사용해 재귀호출을 수행하는 형태, 재귀 호출에 필요한 추가적인 매개변수나 상태를 관리하는 역할을 함

```javascript
function collectOddValues(arr) {
  let result = [];
  function helper(helperInput) {
    if (helperInput.length === 0) return;
    if (helperInput[0] % 2 !== 0) {
      result.push(helperInput[0]);
    }
    helper(helperInput.slice(1));
  }
  helper(arr);
  return result;
}
```
