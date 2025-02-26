## App router vs Page router

- App router(Next.js 13 부터 도입)
  v13은 Approuter 뿐만 아니라 React Server Component Server Fetching 등 다양한 변화
- Page router(Next.js 12 이하 방식)<br/>

### Page router(pages/) 방식

: getServerSideProps, getStaticProps 등의 함수로 데이터를 가져오며 <b>페이지 단위</b>로만 동작

#### 📌 server side data fetching

- <b>Static Site Generation - getStaticProps()</b> <br>
  : 빌드 타임(next build)에 실행 , 내용이 모두 채워진 정적인 HTML<br/>
  다시 build 할 때까지 유지됨 (데이터 변경이 반영 되지 않음)<br/>
  CDN에서 제공이 가능해 속도가 빠름 <br/>
  예시 : 블로그 글, 문서 페이지, 제품 상세 페이지

  ```javascript
  export default function StaticPage({ data }: Props) {
    return (
      <div>
        <h1> getStaticProps </h1>
        <p>{data}~</p>
      </div>
    );
  }
  // getStaticProps 함수 (빌드 시 실행됨)
  export const getStaticProps: GetStaticProps = async () => {
    const data = "이 데이터는 빌드 시 생성되었습니다!"; // 예제 데이터 (API 호출 가능)
    return {
      props: { data }, // 페이지에 props 전달
    };
  };
  ```

- <b>Server Side Rendering - getServerSideProps()</b> <br>
  : 요청시점에 실행 , 동적으로 HTML 파일 생성<br/>
  요청시 마다 재실행 (데이터 변경이 반영)<br/>
  예시: 로그인한 사용자의 대시보드, 실시간 뉴스, 최신 댓글 목록

  ```javascript
  export default function ServerPage({ data }: Props) {
    return (
      <div>
        <h1>getServerSideProps</h1>
        <p>{data}</p>
      </div>
    );
  }

  // getServerSideProps 함수 (요청 시마다 실행됨)
  export const getServerSideProps: GetServerSideProps = async () => {
    const data = `이 데이터는 ${new Date().toLocaleTimeString()}에 생성되었습니다!`; // API 요청 가능
    return {
      props: { data }, // 페이지에 props 전달
    };
  };
  ```

#### 📌 getStaticProps, getServerSideProps 단점

1. 무조건 최상단 위치
2. 내부 컴포넌트에선 예측이 어려움

<details>
<summary>📌 빌드시 로그</summary>

```bash
Page                                                           Size     First Load JS
┌ ○ /static-page                                              3.45 kB        67.1 kB
├ ● /server-page                                              3.51 kB        67.2 kB
├   ├ f /dynamic/[id]                                         3.65 kB        68.0 kB
```

○(원 모양) 아이콘 : 정적으로 사전 렌더링된(Prerendered) 페이지 - getStaticProps 사용</br>
●(채워진 원 모양) 아이콘 : 서버에서 동적으로 렌더링되는(Server-rendered) 페이지 - getServerSideProps 사용</br>
f /dynamic/[id] : 동적경로를 사용하는 서버 렌더링 페이지 -getServerSideProps 또는 API 라우트에서 서버에서 데이터를 가져와 동적으로 페이지를 생성(특정 경로에 따라 페이지가 다르게 렌더링 됨을 의미)

</details>

### App router (app/ 방식)

: React Server Component(React) + ServerAction(Next.js) 활용해 컴포넌트 단위에서도 서버 사이드 데이터 페칭이 가능

#### 📌 server component vs client component

- <b>서버 컴포넌트에서 직접 데이터 가져오기</b> <br>
  : fetch()를 서버 컴포넌트 내부에서 직접 실행 가능 (useEffect 등 사이드 이펙트 함수 사용❌) <br/>
  ➡️ 불필요한 API 호출을 줄이고 성능 최적화 가능

  ```javascript
  async function Page() {
    // 기본적으로 fetch()는 빌드타임에 캐싱되나 (기본값이 SSG)
    const res = await fetch("https://api.example.com/data", {
      cache: "no-store",
    }); // cache: "no-store"로 SSR 처럼 요청마다 새로운 데이터를 가져옴
    const data = await res.json();
    return <div>{JSON.stringify(data)}</div>;
  }
  export default Page;
  ```

- <b> 클라이언트 컴포넌트에서 직접 데이터 가져오기</b> <br>
  : 리액트 단독 사용 마냥 useEffect 사용해야 데이터 페칭 가능 </br>
  서버에서 처리할 필요 없는 데이터에 적합

  ```javascript
  "use client";
  import { useEffect, useState } from "react";

  function ClientComponent() {
    const [data, setData] = useState(null);

    useEffect(() => {
      fetch("https://api.example.com/data")
        .then((res) => res.json())
        .then((data) => setData(data));
    }, []);

    return <div>{JSON.stringify(data)}</div>;
  }
  export default ClientComponent;
  ```

<details>
<summary>📌 빌드시 로그</summary>

```bash
Route (app)                           Size     First Load JS
┌ ○ / (Server)                        0 B             70.1 kB
├   /_not-found                        0 B             0 B
└ ○ /layout                            0 B             0 B

● /components/ClientComponent (Client)   2.1 kB        2.1 kB

```

○ / (Server) → 서버 컴포넌트 </br>
● /components/ClientComponent (Client) → 클라이언트 컴포넌트<br/>
<br/>

- 서버컴포넌트는 JS 번들 크기가 0B로 표시되고, 클라이언트컴포넌트는 번들 크기가 표시됨 <br/>
  ➡️ 서버 컴포넌트는 빌드시 HTML로 변환돼 브라우저에선 JS가 필요없음 (클라이언트 측에서 실행될 javascript가 없음)<br/>
  ➡️ 클라이언트 컴포넌트는 실행될 JS가 브라우저로 전달돼 번들에 포함됨 불필요한 클라이언트 컴포넌트는 줄이는 것이 next.js의 추구미 😉

</details>
```
