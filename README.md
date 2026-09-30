# 9/30 (5주차) 202230137 최원재
## Route 방식 비교

### 1. React vs Next.js 라우팅 방식의 차이

| 항목 | React (기본) | Next.js |
| :--- | :--- | :--- |
| **라우팅 방식** | 수동 (사용자가 직접 설정) | 자동 (폴더/파일 기반) |
| **라우터 도구** | `react-router-dom` 같은 외부 라이브러리 필요 | 자체 내장된 파일 기반 라우팅 시스템 |
| **라우트 정의 방식** | 코드에서 직접 `<Route>`로 정의 | 파일/폴더 이름으로 라우트가 자동 매핑됨 |
| **예시** | `<Route path="/about" element={<About />} />` | `pages/about.js` $\rightarrow$ `/about` 경로 자동 생성<br>`app/about/page.tsx` $\rightarrow$ `/about` 경로 자동 생성 |

### 2. Next.js의 라우팅 방식: Pages Router vs App Router

| 항목 | Pages Router | App Router |
| :--- | :--- | :--- |
| **도입 시기** | 초기부터 존재 (Next.js 1~12) | Next.js 13부터 도입 |
| **루트 디렉토리** | `pages/` | `app/` |
| **파일 기반** | `pages/about.js` $\rightarrow$ `/about` | `app/about/page.tsx` $\rightarrow$ `/about` |
| **특징** | 간단하고 익숙함 (기존 React 방식과 유사) | 더 유연하고 강력한 기능 지원 |
| **대표 기능** | 동적 라우트, `getStaticProps` 등 | 레이아웃 중첩, 서버 컴포넌트, 로딩 UI, 병렬 라우트 등 |
| **추천 여부** | 유지보수 중 (권장하지 않음) | Next.js 14부터 기본 권장 방식 |

* **App Router의 강력한 기능들**

| 기능 | 설명 |
| :--- | :--- |
| **중첩 레이아웃** | 여러 레벨의 `layout.js` 파일을 통해 레이아웃을 계층적으로 구성 가능 |
| **서버 컴포넌트 (RSC)** | 서버에서만 렌더링되는 컴포넌트로 성능 최적화 가능(React Server Component) |
| **로딩 UI** | 페이지 전환 중 보여줄 `loading.js` 파일 제공 |
| **에러 UI** | 특정 경로에서만 발생하는 에러를 처리할 `error.js` 제공 |
| **병렬 라우팅** | 하나의 경로 안에서 탭 같은 독립적인 뷰를 병렬로 렌더링 가능 |

* **프로젝트 별 추천 방식**

| 상황 | 추천 방식 |
| :--- | :--- |
| **새 프로젝트 시작** | **App Router (`app` 디렉토리 기반)** |
| **기존 프로젝트 유지보수** | `pages/` 계속 사용 가능하지만, 점차 마이그레이션 필요 |
| **React처럼 수동 라우팅이 필요한 경우** | `React + react-router-dom` 사용 가능 (Next.js는 자동 라우팅이 기본) |

### 1. How navigation works(네비게이션 작동 방식)

  * Server Rendering(서버 렌더링)
  * Prefetching(프리페칭)
  * Streaming(스트리밍)
  * Client-side transitions(클라이언트 측 전환)

### 1-1. Server Rendering(서버 렌더링)

* Next.js에서 레이아웃(layout)과 페이지(page)는 기본적으로 React 서버 컴포넌트 입니다. (React문서 확인)
* 초기 네비게이션 및 후속 네비게이션 할 때, 서버 컴포넌트 페이로드는 클라이언트로 전송되기 전에 서버에서 생성됩니다.

* 서버 렌더링에는 발생 시점에 따라 두 가지 유형이 있습니다.
  * 정적 렌더링(또는 사전 렌더링)은 빌드 시점이나 재검증 중에 발생하며, 결과는 캐시(cache)됩니다.
    * 재검증을 사용하면 전체 애플리케이션을 다시 빌드하지 않고도 캐시 항목을 업데이트할 수 있습니다.
  * 동적 렌더링은 클라이언트 요청에 대한 응답으로 요청 시점에 발생합니다.

* 서버 렌더링의 단점은 클라이언트가 새 경로를 표시하기 전에 서버의 응답을 기다려야 한다는 것입니다.

* Next.js는 사용자가 방문할 가능성이 높은 경로를 미리 가져 오고(prefetching), 클라이언트 측 전환(client-side transitions)을 수행하여 지연 문제를 해결합니다.

### 1-3. Streaming(스트리밍)

* 스트리밍을 사용하면 서버가 전체 경로가 렌더링될 때까지 기다리지 않고, 동적 경로의 일부가 준비되는 즉시 클라이언트에 전송할 수 있습니다.
* 즉, 페이지의 일부가 아직 로드 중이더라도 사용자는 더 빨리 콘텐츠를 볼 수 있습니다.
* 동적 경로의 경우, 부분적으로 미리 가져올 수 있다는 뜻입니다.
* 즉, 공유 레이아웃과 로딩 스켈레톤을 미리 요청할 수 있습니다.

### 1-4. Client-side transitions(클라이언트 측 전환)

* 일반적으로 서버 렌더링 페이지로 이동하면 전체 페이지가 로드됩니다.
  $\rightarrow$ 이로 인해 state가 삭제되고, 스크롤 위치가 재설정되며, 상호작용이 차단됩니다.

* Next.js는 `<Link>` 컴포넌트를 사용하는 클라이언트 측 전환을 통해 이를 방지합니다. 페이지를 다시 로딩하는 대신 다음과 같은 방법으로 콘텐츠를 동적으로 업데이트합니다:
  * 공유 레이아웃과 UI를 유지합니다.
  * 현재 페이지를 미리 가져온(prefetching) 로딩 상태 또는 사용 가능한 경우 새 페이지로 바꿉니다.

* 클라이언트 측 전환은 서버에서 렌더링된 앱을 클라이언트에서 렌더링된 앱처럼 느껴지게 하는 요소입니다.
* 또한 프리페칭 및 스트리밍과 함께 사용하면 동적 경로에서도 빠른 전환이 가능합니다.

### 2. 전환을 느리게 만드는 요인은 무엇일까요?

* Next.js는 최적화를 통해 네비게이션 속도가 빠르고 반응성이 뛰어납니다.
* 하지만 특정 조건에서는 전환 속도가 여전히 느릴 수 있습니다.
* 다음은 몇 가지 일반적인 원인과 사용자 경험을 개선하는 방법입니다.

### 2-1. 동적 경로 없는 loading.tsx

* 동적 경로로 이동할 때 클라이언트는 결과를 표시하기 전에 서버의 응답을 기다려야 합니다.
  $\rightarrow$ 이로 인해 사용자는 앱이 응답하지 않는다는 인상을 받을 수 있습니다.

* 부분 프리페칭을 활성화하고, 즉시 네비게이션을 트리거하고, 경로가 렌더링되는 동안 로딩 UI를 표시하려면 동적 경로에 `loading.tsx`를 추가하는 것이 좋습니다.

# 9/23 (4주차) 202230137 최원재
## Link Component 기본 사용법
- Link는 HTML a 요소를 확장하여 프레페칭(prefetching)과 라우트 간 클라이언트 사이드 내비게이션 기능을 제공하는 React 컴포넌트
- Next.js에서 라우트 간 이동을 위해 주로 사용되는 방법

### 1.href (required)
- 이동할 경로 또는 URL을 prop으로 전달
~~~tsx
import Link from 'next/link'
 
// Navigate to /about?name=test
export default function Page() {
  return (
    <Link
      href={{
        pathname: '/about',
        query: { name: 'test' },
      }}
    >
      About
    </Link>
  )
}
~~~

### 2. Creation a layout(레이아웃 만들기)
- index 페이지를 자식으로 허용하는 레이아웃을 만들려면 app디렉토리에 layout 파일을 추가
- Rootlayout component는 반드시 있어야함, subpage의 layout은 없어도 상관 없음
 

### 3. Creating a nested route (중첩 라우트 만들기)

* 중첩 라우트는 다중 URL 세그먼트로 구성된 라우트입니다.
* 예를 들어, `/blog/[slug]` 경로는 세 개의 세그먼트로 구성됩니다.
  * `/` (Root Segment)
  * `blog` (Segment)
  * `[slug]` (Leaf Segment)

### [ Next.js에서 ]

* 폴더는 URL 세그먼트에 매핑되는 경로 세그먼트를 정의하는 데 사용됩니다.
  > **# 즉, 폴더가 URL 세그먼트가 된다는 의미입니다.**

* 파일(예: `page` 및 `layout`)은 세그먼트에 표시되는 UI를 만드는 데 사용됩니다.
* 폴더를 중첩하면 중첩된 라우트를 만들 수 있습니다.

---

> **# URL Segment란**  
> URL에서 특정 리소스에 대한 경로를 구성하는 부분을 의미

### 문서의 코드를 복사하면 오류가 나옵니다.

- @/lib/posts와 @/ui/post를 작성하지 않았기 때문에 오류가 발생

- 문서에서 별도의 library를 사용한 것은 blog폴더 하나에는 하나의 URL 세그먼트만 존재하지만, 많은 양의 post를 각기 다른 주소로 호출하기 위한 동적 라우팅인 [slug]를 설명하기 위해서

### [slug]의 이해

- slug는 사이트의 특정 페이지를 쉽게 읽을 수 있는 형태로 식별하는 URL의 일부
- 신문이나 잡지 등에서 핵심 의미를 포함하는 단어만을 조합해 간단 명료하게 제목을 작성하는 것을 슬러그라고 하는 것에서 유래
- 문사의 경로 /blog/[slug]의 [slug] 부분은 불러올 데이터의 key를 말함

### Rendering with search params(검색 매개변수를 사용한 렌더링)

- 페이지에 대한 데이터를 로드하기 위해 검색 매개변수가 필요한 경우(예: 페이지 매김, 데이터베이스에서 필터링) `searchParams` prop을 사용
- 검색 매개변수가 클라이언트에서만 사용되는 경우(예: props를 통해 이미 로딩된 목록을 필터링하는 경우) `useSearchParams`를 사용
- 콜백 이나 이벤트 핸들러에서 `new URLSearchParams(window.location.search)`를 사용하여 리렌더링을 하지 않고도 검색 매개변수를 읽어올 수 있다

---

>앞에서 살펴본 params와 searchParams의 차이는 다음과 같다

>>params는 동적 세그먼트 [slug]에서 가져오는 값으로 URL의 path 부분에 포함된 데이터를 의미합니다

>>searchParams는 query string에서 가져오는 값으로 URL의 ? 이후에 붙는 key=value 데이터를 의미합니다.


# 9/16 (3주차) 202230137 최원재

## Opting for loading skeletons a specific route
~~~tsx
export default function Loading() {
    return (
        <div>
            Loading...
        </div>
    )
}
---------------------------
export default async function BlogPage() {
    await new Promise((resolve) => setTimeout(resolve, 3000)); // 유용하게 자주 쓰임
    return (
        <div>
            Blog 페이지
        </div>
    )
}
~~~


## Organizing your project(프로젝트 구성하기)
- UI 로직과 라우팅 로직을 분리
- 프로젝트와 Next.js 생태계 전반에서 내부 파일을 일관되게 구성
- 코드 편집기에서 파일을 정렬하고 그룹화
- 향후 Next.js 파일 규칙과 관련된 잠재적인 이름 충돌을 방지

## Folder and file conventions(폴더 및 파일 규칙)
**[병렬 및 가로채기 라우팅]**
- 이러한 기능은 슬롯 기반 레이아웃이나 모달 라우팅과 같은 특정 UI 패턴에 적합하다
## Open Graph Protocol
- 웹사이트타 페이스북, 카카오톡 등에 링크를 전달할 때 '미리보기'를 생성하는 프로토콜
- 페이스북이 주도하는 표준화 규칙으로 대부분 sns 플랫폼에서 사용됨

# 9/9 (2주차)

# Next.js의 동적 라우팅

- Next.js의 동적 라우팅은 3가지로 구분됩니다.
- 핵심적인 차이는 **"하위 경로(Depth)를 어디까지 허용할 것인가"**와 **"동적 세그먼트가 없는 기본 경로를 처리할 수 있는가"**에 있습니다.

---

### 1. 일반 동적 라우팅 (Dynamic Segments)

- **디렉토리 구조**: `[slug]`
- **작동 방식**: 1개의 특정 경로 세그먼트만 동적으로 매칭합니다.
- **매칭 예시**:
  - `/posts/abc` → 매칭 성공 (`slug = 'abc'`)
  - `/posts/123` → 매칭 성공 (`slug = '123'`)
  - `/posts` → 매칭 실패 (404 에러)
  - `/posts/abc/def` → 매칭 실패 (하위 경도가 더 있어서 404 에러)

*\* 슬러그(Slug) : 신문이나 잡지 등에서 제목을 쓸 때, 중요한 의미를 포함하는 단어만을 이용해 제목을 작성하는 방법을 말한다.*


### [ 최상위 파일 ] Top-level files

- 최상위 파일은 애플리케이션 구성, 종속성 관리, 프록시 실행, 모니터링 도구 통합, 환경 변수 정의에 사용됩니다.
> **참고:** 다음 파일이 프로젝트 생성과 동시에 모두 생성되는 것은 아닙니다.

| 파일명 | 설명 (한글) | Description (영문) |
| :--- | :--- | :--- |
| `next.config.js` | Next.js에 대한 구성 파일 | Configuration file for Next.js |
| `package.json` | 프로젝트 종속성 및 스크립트 | Project dependencies and scripts |
| `instrumentation.ts` | OpenTelemetry 및 계측 파일 | OpenTelemetry and Instrumentation file |
| `proxy.ts` | Next.js 요청 프록시 | Next.js request proxy |
| `.env` | 환경 변수 | Environment variables |
| `.env.local` | 로컬 환경 변수 | Local environment variables |
| `.env.production` | 프로덕션 환경 변수 | Production environment variables |
| `.env.development` | 개발 환경 변수 | Development environment variables |
| `.eslintrc.json` | ESLint에 대한 구성 파일 | Configuration file for ESLint |
| `.gitignore` | 무시할 Git 파일 및 폴더 | Git files and folders to ignore |
| `next-env.d.ts` | Next.js에 대한 TypeScript 선언 파일 | TypeScript declaration file for Next.js |
| `tsconfig.json` | TypeScript용 구성 파일 | Configuration file for TypeScript |
| `jsconfig.json` | JavaScript용 구성 파일 | Configuration file for JavaScript |

### .eslintrc.json vs eslint.config.mjs

- **JSON**은 주석, 변수, 조건문 등을 쓸 수 없기 때문에 복잡한 설정이 어렵습니다.  
(JavaScript Object Notation)
- **mjs**는 ESLint가 새롭게 도입한 방식으로, ESM(ECMAScript 모듈) 형식입니다.
- 확장자 `.mjs`는 "module JavaScript"를 의미합니다.
- **ESLint v9 이상**에서 공식 권장 방식입니다.
- 조건문, 변수, 동적 로딩 등 코드처럼 유연한 설정이 가능합니다.
- 다른 설정 파일을 `import` 해서 재사용을 할 수 있습니다.
- 프로젝트 규모가 커질수록 유지보수에 유리합니다.

| 항목 | `.eslintrc.json` | `eslint.config.mjs` |
| :--- | :--- | :--- |
| **포맷** | JSON 형식 | JavaScript 모듈 (ESM) |
| **실행 방식** | 정적인 설정 파일 | 동적인 설정도 가능 (함수, 변수 사용 등) |
| **호환성** | 구버전 ESLint와 호환 | ESLint v9부터 공식 권장 |
| **특징** | 간단하고 직관적 | 더 유연하고 모듈화 가능 |
| **사용 여부** | 여전히 사용 가능 | 최신 Next.js에서 기본값 |

### 오류 처리
- 문서의 지시 대로만 처리하면 오류가 발생
- 타입스크립트 환경이 아니기 때문
- 타입스크립트 환경에서 react와 react-dom을 사요알 수 있도록 타입 정의를 제공하는 패키지를 설치해야 한다.

### 1. Folder and file conventions (폴더 및 파일 규칙)

| 구분 | 일반 설치<br>`pnpm add <pkg>` | 개발용 설치<br>`pnpm add -D <pkg>` |
| :--- | :--- | :--- |
| **등록 위치** | `package.json` **➔** `dependencies` | `package.json` **➔** `devDependencies` |
| **용도** | 실제 서비스 구동에 **반드시 필요한** 패키지 | 코드 빌드, 테스트, 린팅 등 **개발 중에만** 필요한 패키지 |
| **배포 환경** | 빌드 결과물에 포함되거나 프로덕션 서버에 설치 | `--production` 옵션 등으로 빌드/배포 시 제외 |
| **대표 예시** | `React`, `Vue`, `Express`, `Axios`, `Lodash` 등 | `TypeScript`, `ESLint`, `Prettier`, `Vite`, `Jest` 등 |
# 9/2 (1주차)

## Getting Started (Docs의 개요)

### App Router와 Page Router
> Next.js에는 두 개의 서로 다른 라우터가 있다.
>>1. App Router : 서버 컴포넌트처럼 새로운 React 기능을 지원하는 최신 라우터
>>2. Pages Router : 조기 라우터로, 현재도 지원되고 개선되고 있다.

### 사전 지식
- React를 처음 사용하거나 다시 배우고 싶다면 React 기초 과정과 Next.js 기초 과정 부터 시작하는 것이 좋다.

### 접근성
- 화면 판독기를 사용할 때 최상의 환경을 얻으려면 Firefox와 NVDA 또는 Safari와 VoiceOver를 사용하는 것이 좋다.
- 접근성(Accessibility)은 웹 접근성을 의미한다.
- Safari와 VoiceOver는 built in screen reader 이다.

### pnpm
>- pnpm은 Performant(효율적인) NPM의 역자로 고성능 Node 패키지 매니저이다.
>> 특징
>>1. 하드 링크 기반의 효율적인 저장 공간 사용
>>2. 빠른 패키지 속도 설치 속도
>>3. 엄격하고 효율적인 종속성 관리
>>4. 다른 패키지 매니저의 비효율성 개선한 패키지 관리자 

>### Hard link vs Symbolic link(Soft link)
>- pnpm의 특징 중에 하드 링크를 사용해서 디스크 공간을 효율적으로 사용할 수 있다고 한다.
>> 하드 링크(Hard link)
>>- 우리가 "파일"이라고 부르는 것은 세 부분으로 나뉘어 있다.
>>1. Directory Enty : 파일 이름과 해당 inode 번호를 매핑 정보가 있는 특수한 파일
>>2. inode : 파일 또는 디렉토리에 대한 모든 메타데이터를 저장하는 구조체
>>3. data blocks : 실제 데이터가 존재하는 영역
>>- 하드링크를 생성하면 디렉토리에 엔트리에 매핑 정보가 추가 되어, 동일한 inode를 가리키게 된다.
- 디렉토리 엔트리에 있는 원본과 하드링크는 같은 inode를 참조하므로 데이터 블록을 100% 공유한다.
