## Search

: 데이터 집합에서 원하는 값을 찾는 방법

### 선형검색(Linear Search)

: 해당 값을 찾거나 맨 끝에 도달할때까지 확인(처음부터/뒤에서 시작할수도 있음)<br/>
➡️ 세트 간격으로 이동하면서 한번에 하나의 항목 확인
<b>indexOf,includes,find,findIndex </b>의 알고리즘<br/>

<br/>
📌 <b>시간복잡도</b><br/>

- 최선 O(1) : 첫번째 요소가 정답일 때
- 평균 / 최악 O(n) : 마지막 요소가 정답이거나 없는 경우(배열이 길어질수록 연산이 많아짐)

<br/>
📌 <b>example</b><br/>

```javascript
function linearSearch(arr, value) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === value) return i; // 값 찾으면 인덱스 반환
  }
  return -1; // 찾지 못하면 -1 반환
}
```

➡️ 배열이 정렬되어 있지 않거나 작은 경우에 적합

### 이진검색(Binary Search)

: <b>정렬된</b> 배열 대상으로 확인할 때마다 남은 항목의 절반을 없애면서 찾는 방식
<br/>
➡️ 분류된 숫자 배열의 경우 한 숫자가 다른 숫자 보다 큰/작은지비교

<br/>
📌 <b>시간복잡도</b><br/>

- 최선 O(1) : 중간 값이 정답일때
- 평균 / 최악 O(log n) : 탐색 범위가 절반씩 줄어듦

<br/>
📌 <b>example</b><br/>

```javascript
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;

  while (left <= right) {
    let mid = Math.floor((left + right) / 2);

    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }

  return -1;
}
```

➡️ 배열이 정렬되어 있고 사이즈가 큰 경우 적합
