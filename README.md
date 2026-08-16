# Soundock Frontend

음악을 좋아하는 사람들이 자기만의 유튜브 영상 및 플레이리스트를 공유하고, 마음에 드는 게시글에 후원까지 할 수 있는 커뮤니티 플랫폼 **Soundock** 프론트엔드입니다.

완료된 팀 프로젝트에 추가적인 기능 구현 및 별도의 배포를 위한 Clone 저장소로, 팀 개발 당시 커밋 이력이 보존되어 있으며 PR기반 협업 이력은 원본 저장소에서 확인이 가능합니다.

**DOPAMINE** | 4명 | 2026.01.02. ~ 2026.03.06. (63일)

- 백엔드 저장소: [2stepslow/soundock-back](https://github.com/2stepslow/soundock-back)
- (원본 BE 저장소): [ningsoo/dpm-project-back](https://github.com/ningsoo/dpm-project-back)
- (원본 FE 저장소): [ningsoo/dpm-project-front-v2](https://github.com/ningsoo/dpm-project-front-v2)

## 담당 역할

- **FrontEnd** 전담: 사용자 & 인증
- **BackEnd** 풀스택 구현: 마이페이지 활동내역 & 게시판 검색
- **FE** 품질개선 & **BE** 보안 스캔 / 분석 / 보완
- 프로젝트 종료 후 추가기능 별도 구현

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
