### 빈도수 세기

: 빠른 접근/검색 O(1)를 위해 객체를 사용하는 것이 좋음 , 중복확인, 순열/anagram확인<br/><br/>
📌 <b>유의할 점</b></br>

- 객체의 키 타입 주의(숫자는 문자열로 변환)
- undefined/null 체크
- 빈 문자열/ 배열 처리
- 대소문자 구분 여부 확인

```javascript
function validAnagram(str1, str2) {
  // 길이 체크
  if (str1.length !== str2.length) return false;

  // 문자 빈도수를 저장할 객체
  const lookup = {};

  //  첫 번째 문자열의 문자 빈도수 기록
  for (let letter of str1) {
    // lookup[letter]가 undefined면 0으로 시작
    lookup[letter] = (lookup[letter] || 0) + 1;
  }

  //  두 번째 문자열과 비교
  for (let letter of str2) {
    // lookup[letter]가 0이거나 undefined면 false
    if (!lookup[letter]) {
      return false;
    } else {
      lookup[letter] -= 1; // 빈도수 감소
    }
  }

  return true;
}
```

<br/>

### 다중 포인터

: 정렬된 배열에서의 검색, 두 요소의 합/차를 찾는 문제, 부분 <b>배열/문자열</b> 관련 문제 => 불필요한 반복과 조기종료 조건 활용 고려 <br/>
<br/>
📌 <b>유의할 점</b></br>

- 포인터 초기위치 결정
- 포인터 이동 조건의 명확한 설정
- 경계조건 (배열의 처음과 끝) 처리
- 포인터가 서로 교차하지 않도록 주의
- 무한루프 방지를 위한 종료조건

```javascript
// 정렬된 배열에서 합이 0이 되는 두 숫자를 찾는 함수

function refactorSumZero(arr) {
  // 배열의 시작 지점
  let left = 0;
  // 배열의 끝 지점
  let right = arr.length - 1;

  // 같은 값을 빼면 0 이 나오므로 left와 right의 수가 달라야함 -> 포인터가 교차하기 전까지 반복
  while (left < right) {
    let sum = arr[left] + arr[right];
    // 합이 0이면 해당 쌍 반환
    if (sum === 0) {
      return [arr[left], arr[right]];
    } else if (sum > 0) {
      // 합이 양수면 오른쪽 값이 크다는 것
      right--;
    } else {
      // 합이 음수면 왼쪽 값이 너무 작음
      left++;
    }
  }
}
```

<br/>

### sliding window

: 연속된 요소들의 부분 배열을 다루는 문제, 조건을 만족하는 부분 배열을 다루는 문제, 최소/최대값을 찾는 문제<br/>
<br/>
📌 <b>유의할 점</b></br>

- 고정/가변 크기인지 파악 </br>
- 윈도우 크기가 배열 길이를 초과하는 예외케이스 처리

```javascript
// 배열에서 연속된 n개 숫자의 최대 합을 찾는 함수

function refactorMaxSumbArraySum(arr, num) {
  // 최대합 저장할 변수
  let maxSum = 0;
  // 현재 윈도우의 합을 저장할 임시변수
  let tempSum = 0;
  // 배열의 길이가 num 보다 작으면 null 반환
  if (arr.length < num) return null;

  // 첫번째 윈도우(인덱스 - 0:num-1) 만큼의 합 계산
  for (let i = 0; i < num; i++) {
    maxSum += arr[i];
  }
  tempSum = maxSum;

  // 슬라이딩 윈도우 이동
  for (let i = num; i < arr.length; i++) {
    // 윈도우에서 첫번째 숫자를 제거한 뒤 새로 숫자를 추가
    tempSum = tempSum - arr[i - num] + arr[i];
    maxSum = Math.max(maxSum, tempSum);
  }
  return maxSum;
}
```
