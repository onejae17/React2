# 9/16 (3주차)

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
