# Soundock Frontend

음악을 좋아하는 사람들이 자기만의 유튜브 영상 및 플레이리스트를 공유하고, 마음에 드는 게시글에 후원까지 할 수 있는 커뮤니티 플랫폼 **Soundock** 프론트엔드입니다.

완료된 팀 프로젝트에 추가적인 기능 구현 및 별도 배포를 진행한 Clone 저장소로, 팀 개발 당시 커밋 이력이 보존되어 있으며 PR기반 협업 이력은 원본 저장소에서 확인이 가능합니다.

**DOPAMINE** | 4명 | 2026.01.02. ~ 2026.03.06. (63일)

- 백엔드 저장소: [2stepslow/soundock-back](https://github.com/2stepslow/soundock-back)
- 원본 저장소: [ningsoo/dpm-project-back](https://github.com/ningsoo/dpm-project-back) | [ningsoo/dpm-project-front-v2](https://github.com/ningsoo/dpm-project-front-v2)

## 데모 안내

- 재배포 링크: [SOUNDOCK](https://soundock-front-indol.vercel.app/)
- 로그인 화면의 '데모 계정 사용하기' 버튼으로 쉽게 로그인 가능합니다
- 결제는 토스페이먼츠 테스트 모드로 동작하며 실제 결제가 발생하지 않습니다
- 데모 환경에서는 유튜브 계정 연동이 제한되며, 플레이리스트는 사전 연동된 데이터로 표시됩니다
- Spotlight 게시판의 컨텐츠는 데모용 가상 데이터입니다
- Passwordless 기능은 외부 인증 서버가 화이트리스트 방식이므로 시연 영상으로 대체합니다 - [Passwordless 기능](https://youtu.be/IFLcF_gfqS8)

## 담당 역할

- **FrontEnd** 전담: 사용자 & 인증
- **BackEnd** 풀스택 구현: 마이페이지 활동내역 & 게시판 검색
- **FE** 품질개선 & **BE** 보안 스캔 / 분석 / 보완
- 프로젝트 종료 후 기상청 API 연동해 습도에 따른 악기관리 안내기능 별도 풀스택 구현

### 재배포 구성

중단된 당시 AWS 배포 구성을 조건에 맞게 재구성 (콜드스타트 없음 / 저비용 / 구축 시간 최소화)

| 구성          | 사용 서비스                          | 비고                                           |
| ------------- | ------------------------------------ | ---------------------------------------------- |
| 백엔드        | Render (Docker, 512MB 단일 인스턴스) | JVM 메모리 상한을 실측 기반으로 고정           |
| DB            | TiDB Cloud (MySQL 호환)              | 백엔드와 동일 리전(싱가포르)                   |
| 캐시          | Upstash Redis (TLS)                  | 인기글 랭킹 집계, 조회 처리, 인증 보조         |
| 이미지 저장소 | Cloudflare R2 (S3 호환 API)          | 기존 S3 연동 코드를 엔드포인트 전환으로 재사용 |
| 프론트        | Vercel (Next.js)                     | OAuth 콜백과 API를 동일 출처 프록시로 구성     |

작업내용: 배포 환경 차이에 따른 코드 수정(도커 빌드 구성, 저장소 엔드포인트 전환, Redis TLS 연결, 쿠키와 보안 정책, OAuth 콜백 프록시), 데모 시딩 준비, 512MB 제약 검증(부하 측정으로 JVM 옵션 확정), 의존성 취약점 점검 및 조치

<!-- TODO: 대표 화면 스크린샷 1~2장 삽입 위치 -->

## 기술 스택

- **Framework**: Next.js 15 (App Router), React 18, TypeScript
- **상태관리**: Redux Toolkit, React Redux
- **HTTP 통신**: Axios
- **결제**: Toss Payments SDK
- **아이콘**: Lucide React
- **코드품질**: ESLint, Prettier, husky + lint-staged (커밋 전 자동 검사)

## 주요 화면과 기능

### 메인

- Spotlight 캐러셀, 인기 Showcase, 인기 Playlist 노출

### 인증

- 로그인, 이메일 인증 회원가입, 이메일/비밀번호 찾기 & 재설정
- Passwordless 인증
- 관리자 로그인

### 게시판

- Category별 게시글 목록/상세/작성/수정
- Youtube Playlist 첨부, 이미지 업로드
- 좋아요, 댓글

### 마이페이지

- 내 프로필 조회/수정, 비밀번호 변경, 회원 탈퇴
- 활동내역(내 게시글, 내 댓글, 좋아요한 게시글) 조회
- Youtube계정 연동 / Playlist 관리
- Pop 충전 (토스페이먼츠 연동, 성공/실패 처리)
- 후원(Donation) 내역, 정산 내역
- 문의 내역, 신고

### 알림 & 메시지

- 알림 드롭다운
- 쪽지함 모달

### 공지사항 & 문의

- 공지사항 목록/상세
- 1:1 문의 작성

### 관리자 (`/adm1n`)

- 게시글 / 댓글 관리
- 공지사항 작성/수정
- 후원 취소 요청 처리
- 문의 / 신고 관리
- 정산 관리, 이용 제재(penalty) 관리
- 회원 관리

## 보안 적용 사항

- 프로덕션 환경에서 nonce 기반 CSP(Content Security Policy)를 middleware에서 적용
- 결제, 유튜브 연동(구글 OAuth), 웹소켓 등 외부 리소스는 허용 목록 방식으로 제한

<!-- TODO: 화면별 스크린샷 삽입 위치 -->

## 프로젝트 구조

```
src/
├── app/                 # Next.js App Router 페이지
│   ├── auth/            # 로그인, 회원가입, 비밀번호/이메일 찾기
│   ├── boards/           # 게시판 목록/상세/작성/수정
│   ├── mypage/           # 마이페이지 (프로필, 충전, 정산, 문의 등)
│   ├── announcement/     # 공지사항
│   ├── inquiry/          # 1:1 문의
│   ├── event/             # 이벤트 페이지
│   └── adm1n/             # 관리자 페이지
├── components/           # 공통/도메인별 UI 컴포넌트
│   ├── common/            # Header, Footer, Toast, Carousel 등
│   ├── board/              # 게시글 카드, 작성/수정 폼
│   ├── home/                # 메인 페이지 섹션
│   └── adm1n/                # 관리자 전용 컴포넌트
├── api/                    # 도메인별 API 클라이언트 (axios)
├── store/                  # Redux Toolkit 스토어 & 슬라이스
├── contexts/               # React Context
├── utils/                  # 공통 유틸 함수
└── middleware.ts           # CSP nonce 적용 등 보안 헤더 처리
```
