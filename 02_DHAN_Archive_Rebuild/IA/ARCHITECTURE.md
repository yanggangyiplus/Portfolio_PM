# 시스템 아키텍처

D'HAN Archive의 시스템 아키텍처 및 설계 상세 설명

## 목차

- [전체 아키텍처](#전체-아키텍처)
- [페이지 라우팅 구조](#페이지-라우팅-구조)
- [컴포넌트 계층 구조](#컴포넌트-계층-구조)
- [데이터 흐름](#데이터-흐름)
- [스타일링 아키텍처](#스타일링-아키텍처)
- [성능 최적화 전략](#성능-최적화-전략)

## 전체 아키텍처

### 시스템 다이어그램

```
┌─────────────────────────────────────────────────────────────┐
│                      사용자 (Browser)                        │
└────────────────────────────┬────────────────────────────────┘
                             │ HTTP/HTTPS
                             ▼
        ┌──────────────────────────────────────┐
        │         Next.js Application          │
        │    (SSR + Client-Side Routing)      │
        │                                      │
        │  ┌────────────────────────────────┐  │
        │  │   Next.js Router              │  │
        │  │   (동적 라우팅 처리)            │  │
        │  └────────────────────────────────┘  │
        │                                      │
        │  ┌────────────────────────────────┐  │
        │  │   React Components             │  │
        │  │   (UI 렌더링)                  │  │
        │  └────────────────────────────────┘  │
        │                                      │
        │  ┌────────────────────────────────┐  │
        │  │   Tailwind CSS                 │  │
        │  │   (스타일링)                    │  │
        │  └────────────────────────────────┘  │
        └──────────────────┬───────────────────┘
                           │
        ┌──────────────────┴───────────────────┐
        │                                      │
        ▼                                      ▼
┌───────────────┐                    ┌───────────────┐
│   Pages       │                    │  Components   │
│   (라우팅)     │                    │   (재사용)     │
│               │                    │               │
│ - index.js    │                    │ - Header.jsx  │
│ - brand.js    │                    │ - Footer.jsx  │
│ - [season]/   │                    │ - MediaGallery│
│ - contact.js  │                    └───────────────┘
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   Data Layer  │
│               │
│ - seasons.json│
│ - Static Assets│
└───────────────┘
```

### 기술 스택 계층

```
┌─────────────────────────────────────┐
│      Presentation Layer            │
│  (React Components + Tailwind CSS)  │
├─────────────────────────────────────┤
│      Application Layer              │
│  (Next.js Router + Pages)           │
├─────────────────────────────────────┤
│      Data Layer                     │
│  (JSON + Static Assets)             │
└─────────────────────────────────────┘
```

## 페이지 라우팅 구조

### 라우팅 계층

```
/ (루트)
├── /                    → Home (index.js)
│   └── 시즌 타임라인 표시
│
├── /brand               → Brand (brand.js)
│   └── 브랜드 소개 및 비전
│
├── /[season]            → Season ([season]/index.js)
│   ├── /2024FW          → 2024 F/W 컬렉션
│   │   ├── 컬렉션 이미지
│   │   ├── 디자이너 노트
│   │   └── BRAND LAUNCHING SHOW
│   │
│   └── /2021ART         → 2021 ART 컬렉션
│       ├── 컬렉션 이미지
│       └── 디자이너 노트
│
└── /contact             → Contact (contact.js)
    ├── 디자이너 프로필
    ├── 수상 내역
    └── 포트폴리오 다운로드
```

### 동적 라우팅 처리

```javascript
// pages/[season]/index.js
export default function Season() {
  const router = useRouter();
  const { season } = router.query;  // URL 파라미터 추출
  
  // seasons.json에서 해당 시즌 데이터 찾기
  const seasonData = seasonsData.seasons.find(s => s.id === season);
  
  // 데이터가 없으면 로딩 상태 표시
  // 데이터가 있으면 시즌 상세 페이지 렌더링
}
```

## 컴포넌트 계층 구조

### 컴포넌트 트리

```
App (_app.js)
│
├── Header (공통)
│   ├── 로고
│   └── 네비게이션 메뉴
│
├── Page Component (동적)
│   │
│   ├── Home (index.js)
│   │   ├── 브랜드 소개 섹션
│   │   └── 시즌 타임라인
│   │       └── 시즌 카드 (반복)
│   │
│   ├── Brand (brand.js)
│   │   ├── 브랜드 소개
│   │   ├── 브랜드 비주얼
│   │   └── 비전 섹션
│   │
│   ├── Season ([season]/index.js)
│   │   ├── 시즌 정보 헤더
│   │   ├── MediaGallery
│   │   │   ├── 이미지 그리드
│   │   │   └── 영상 플레이어
│   │   ├── 디자이너 노트
│   │   └── BRAND LAUNCHING SHOW (조건부)
│   │
│   └── Contact (contact.js)
│       ├── 프로필 사진
│       ├── 기본 정보
│       └── 수상 내역
│
└── Footer (공통)
    └── 저작권 정보
```

### 컴포넌트 재사용성

#### Header 컴포넌트
- 모든 페이지에서 공통으로 사용
- 고정 헤더 (fixed positioning)
- 반응형 네비게이션 메뉴

#### Footer 컴포넌트
- 모든 페이지에서 공통으로 사용
- 간단한 저작권 정보 표시

#### MediaGallery 컴포넌트
- 이미지와 영상을 표시하는 재사용 가능한 컴포넌트
- Props로 이미지/영상 배열 받음
- 반응형 그리드 레이아웃
- 호버 효과 및 애니메이션

## 데이터 흐름

### 정적 데이터 흐름

```
seasons.json (정적 JSON 파일)
    │
    ▼
Page Component (데이터 import)
    │
    ├── Home: 전체 시즌 목록 표시
    │   └── 연도순 정렬
    │
    └── Season: 특정 시즌 데이터 필터링
        │
        ▼
    MediaGallery Component
        │
        ├── images 배열 → 이미지 그리드 렌더링
        └── videos 배열 → 영상 플레이어 렌더링
```

### 데이터 구조

```json
{
  "seasons": [
    {
      "id": "2024FW",
      "title": "2024 F/W",
      "subtitle": "D'HAN",
      "description": "BRAND LAUNCHING",
      "year": 2024,
      "images": ["/assets/images/..."],
      "videos": ["/assets/videos/..."],
      "showImages": [],
      "showVideos": ["/assets/videos/..."],
      "showVideoThumbnails": ["/assets/images/..."]
    }
  ]
}
```

### 데이터 로딩 전략

1. 정적 데이터: `seasons.json` 파일을 직접 import하여 빌드 타임에 포함
2. 이미지 최적화: Next.js Image 컴포넌트 사용 (자동 최적화)
3. 지연 로딩: 이미지와 영상은 필요할 때만 로드

## 스타일링 아키텍처

### Tailwind CSS 구조

```
globals.css
├── @import (폰트)
│   ├── Pretendard (본문)
│   └── Noto Serif KR (제목)
│
├── @tailwind base
│   └── body 스타일 (배경색, 폰트)
│
├── @tailwind components
│   ├── .glass (Glassmorphism 효과)
│   └── .fade-in (페이드인 애니메이션)
│
└── @tailwind utilities
    └── Tailwind 유틸리티 클래스
```

### 커스텀 디자인 시스템

#### 색상 팔레트
```javascript
// tailwind.config.js
colors: {
  ivory: '#F5F5F0',    // 배경색
  beige: '#E8E3D8',    // 보조 색상
  charcoal: '#2C2C2C', // 텍스트 색상
}
```

#### 폰트 시스템
- 본문: Pretendard (sans-serif)
- 제목: Noto Serif KR (serif)

#### 애니메이션
- fade-in: 페이드인 + 약간의 위로 이동 효과
- hover 효과: 이미지 확대, 색상 변화

## 성능 최적화 전략

### Server-Side Rendering (SSR)

- Next.js의 기본 SSR 기능 활용
- 초기 로딩 속도 향상
- SEO 최적화

### 이미지 최적화

- Next.js Image 컴포넌트 사용
- 자동 이미지 최적화 및 WebP 변환
- Lazy loading 지원

### 코드 스플리팅

- 페이지별 자동 코드 분할
- 필요한 코드만 로드하여 초기 번들 크기 감소

### 정적 에셋 최적화

- 이미지: 적절한 해상도로 최적화
- 영상: 적절한 압축 및 포맷 사용

### CSS 최적화

- Tailwind CSS의 Purge 기능으로 사용하지 않는 스타일 제거
- PostCSS를 통한 CSS 최적화

## 보안 고려사항

### XSS 방지

- React의 기본 XSS 방지 기능 활용
- 사용자 입력 검증

### 정적 파일 보안

- `public/` 폴더의 파일만 공개
- 민감한 정보는 절대 `public/`에 저장하지 않음

### 환경 변수

- API 키 등 민감한 정보는 환경 변수로 관리
- `.env.local` 파일은 Git에 커밋하지 않음

## 확장 가능성

### 향후 개선 사항

1. CMS 연동: 컨텐츠 관리 시스템 연동으로 동적 콘텐츠 관리
2. 다국어 지원: i18n을 통한 다국어 지원
3. 검색 기능: 시즌 및 컬렉션 검색 기능 추가
4. 관리자 페이지: 콘텐츠 업로드 및 수정을 위한 관리자 페이지

## 참고 자료

- [Next.js 공식 문서](https://nextjs.org/docs)
- [React 공식 문서](https://react.dev/)
- [Tailwind CSS 공식 문서](https://tailwindcss.com/docs)
