Commit

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
