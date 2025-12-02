# Social Login Community Service

소셜 로그인 기반 커뮤니티 플랫폼 기획

## 문제 정의

접근성이 높은 커뮤니티 경험을 제공하고, 직관적인 탐색 UX를 설계하며, 운영자 관리 비용을 최소화하는 것이 필요했습니다.

## 해결 방법

1. **소셜 로그인 통합 설계**: 카카오/네이버/구글 OAuth 2.0 인증을 통합하여 사용자 접근성을 향상시켰습니다.
2. **정보구조(IA) 설계**: 카테고리 기반 게시판 구조로 직관적인 탐색 UX를 설계했습니다.
3. **사용자 플로우 설계**: 핵심 사용자 플로우를 정의하고 예외 상황에 대한 처리 방안을 설계했습니다.
4. **사용자 행동 분석 기반 UX 개선**: 실제 사용자 행동 데이터를 분석하여 Pain Point를 도출하고 개선안을 제시했습니다.

## 성과

- 요구사항 분석부터 기능명세서 작성까지 전체 기획 프로세스 수행
- 카카오/네이버/구글 OAuth 2.0 통합 설계 완료
- 게시판, 댓글, 좋아요, 신고 등 기본 커뮤니티 기능 명세 완료
- API 명세서, 기능명세서, UX 플로우 다이어그램 등 체계적인 문서화 완료

## 주요 산출물

- **IA (정보구조도)**: 전체 서비스 구조 설계 및 ERD 작성
- **UX Flow**: 핵심 사용자 플로우 다이어그램
- **Wireframes**: Figma를 활용한 와이어프레임 제작
- **Requirements**: FUNC-001 ~ FUNC-005 기능명세서
- **UX Research**: 사용자 행동 분석 및 Before → After 개선안

## 프로젝트 구조

```
01_Social_Login_Community_Service/
├── IA/
│   └── ERD.md              # 데이터베이스 ERD
├── UX_Flow/
│   ├── 01-User-Login-Flow.md
│   ├── 02-Post-Write-Flow.md
│   └── 03-Report-Flow.md
├── Requirements/
│   ├── FUNC-001-Authentication.md
│   ├── FUNC-002-Posts.md
│   ├── FUNC-003-Comments.md
│   ├── FUNC-004-Reports.md
│   └── FUNC-005-Admin.md
└── UX_research/
    └── UX-Improvement-Before-After.md
```

## 기술 스택

`React` `Flask` `OAuth2.0` `MySQL` `JWT`

## 프로젝트 위치

`../../Product-UX-Community-Platform/`
