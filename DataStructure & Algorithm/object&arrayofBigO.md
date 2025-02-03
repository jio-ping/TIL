## 객체의 시간복잡도

### 객체의 사용

1. 정렬이 필요없는 데이터
2. 빠른 접근, 입력과 제거

### 객체의 입력, 접근, 제거, 탐색

📌 <b>입력(Insertion), 접근(Access), 제거(Removal)</b> ➡️ O(1)<br/>
📌 <b>탐색(Searching)</b> ➡️ O(N)

### 객체 메소드의 시간복잡도

📌 <b>Object.keys( ), Object.values( ), Object.entries( ) </b></br>
:객체의 키, 값, 키-값쌍을 배열로 반환하므로 <b>O(n)</b> <br/>

📌<b> hasOwnProperty( ) </b></br>: 특정 속성의 존재여부를 즉시 확인해 <b>O(1)</b><br/>

<br/>

## 배열의 시간복잡도

### 배열의 사용

1. 정렬이 필요할 때

### 배열의 입력, 접근, 제거, 탐색

📌 <b>Insertion / Removal</b></b>

- 끝에 추가 및 삭제하는 경우 O(1)
- 첫번째 추가 및 삭제하는 경우 인덱스 재조정 필요 ➡️ O(n)

📌 <b>Searching</b></br>

: 배열 안의 특정 값을 찾고자할 때 모든 엘리먼트를 확인해야 할 수 있음 <b>O(n)</b>
</br>

📌 <b>Access</b></br>
: 인덱스를 알면 즉시 해당 메모리 위치를 계산할 수 있음 <b>O(1)</b>

### 배열 메소드의 시간복잡도

📌 <b>push( ), pop( )</b></br>
: 배열 맨 끝 추가, 삭제는 크기와 무관 <b>O(1)</b></br>
📌 <b>shift( ), unShift( )</b></br>
: 인덱스 재조정 필요 <b>O(n)</b></br>
📌 <b>concat( )</b></br>
: length N인 배열과 M인 배열을 합치면 O(N+M) ➡️ <b>O(n)</b></br>
📌 <b>shift( ), unShift( )</b></br>
: 인덱스 재조정 필요 <b>O(n)</b></br>
📌 <b>slice( ), splice( )</b></br>
: 작업 요소의 개수에 비례 <b>O(n)</b></br>
📌 <b>forEach, map, filter, reduce</b></br>
: 배열의 크기와 비례 <b>O(n)</b></br>
📌 <b>sort</b></br>
: 배열 정렬 알고리즘(대체로 quickSort)으로 <b>O(n\*log(n))</b></br>
