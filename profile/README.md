<div align="center">

# 🍚 온고지식 (溫故지식)
### 한국 전통 음식 AI 추천 서비스

공공데이터와 생성형 AI를 결합하여, 누구나 자연어로 한국 전통 음식을 탐색하고
조리법·유래·역사적 배경까지 한 번에 만나는 서비스입니다.

[![Frontend](https://img.shields.io/badge/Frontend-React%20Native%20%2F%20Expo-345237?style=flat-square)](https://github.com/OngoJishik)
[![Backend](https://img.shields.io/badge/Backend-Spring%20Boot%204-962E22?style=flat-square)](https://github.com/OngoJishik)
[![Database](https://img.shields.io/badge/Database-MySQL-D1AE5D?style=flat-square)](https://github.com/OngoJishik)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions%20%E2%86%92%20AWS%20EC2-345237?style=flat-square)](https://github.com/OngoJishik)

[📱 다운로드(Play Store)](https://play.google.com/store/apps/details?id=com.ongo.jishik) - 현재는 심사중에 있습니다.

</div>

---

## 📖 소개

**온고지식**은 지식재산처가 보유한 「한국전통 식품정보」 공공데이터를 활용하여, 사용자가 맛·재료·상황·형태 등을 자연어로 입력하면 AI가 의도를 분석해 알맞은 전통 음식을 추천하는 모바일 서비스입니다.

음식명을 몰라도 괜찮습니다. *"매콤한 국물 음식"*, *"명절에 먹는 음식"*, *"드라마에서 본 화려한 떡"* 처럼 떠오르는 특징만 입력하면, AI가 조리법·식재료·역사적 배경을 스토리텔링 형태로 함께 제공합니다.

분산되어 있고 접근하기 어려운 전통 음식 데이터를 통합하고, 한국어·영어·일본어·중국어 4개 언어를 지원하여 국내외 사용자 모두의 전통 음식 탐색 장벽을 낮추는 것을 목표로 합니다.

> 제4회 문화체육관광 인공지능·데이터 활용 공모전 — 문화데이터 활용 분야 출품작

---

## ✨ 핵심 기능

| 기능 | 설명 |
|---|---|
| 🔍 **자연어 음식 추천** | 음식명을 몰라도 상황·취향 문장 입력만으로 적합한 전통 음식을 추천 |
| 📚 **상세 정보 & 스토리텔링** | 조리법, 식재료, 문헌 원문·번역문 기반의 유래·역사적 배경 제공 |
| 🎨 **AI 생성 이미지** | 사진 자료가 부족한 전통 음식도 이름·재료·특징 기반 이미지로 시각화 |
| 🏪 **전통시장 연계** | 추천 음식의 식재료 카테고리 기준으로 주변 전통시장 거리순 안내 |
| 💬 **커뮤니티** | 후기·레시피·질문 카테고리의 게시글, 좋아요, 즐겨찾기, 댓글 |
| 🌐 **다국어 지원** | 한국어 / 영어 / 일본어 / 중국어 런타임 전환 |
| 🔐 **소셜 로그인** | Google ID 토큰 서버 검증 + 자체 JWT 액세스·리프레시 토큰 발급 |

---

## 🧠 AI 기술 — 환각 없는 추천 구조

생성형 AI가 추천 결과를 직접 만들어내지 않도록, **분석(AI)과 추천(알고리즘)을 분리**한 것이 핵심 설계입니다.

1. **폐쇄형 어휘 기반 특징 추출**
   맛 26종, 색감 15종, 온도·무게감 10종, 조리방식 24종, 요리형태 28종, 상황·목적 33종 — 총 **136개 특징 어휘 × 19개 카테고리**로 구성된 폐쇄형 분류체계를 시스템 프롬프트로 Gemini에 제공하고, 목록에 없는 label은 서버에서 즉시 오류 처리해 환각을 원천 차단합니다.
2. **결정론적 매칭/랭킹 알고리즘**
   검증된 특징·카테고리를 한국전통지식포털 기반 Food DB와 비교하여, 음식명 직접 매칭 → 특징 일치 개수 → 카테고리 일치 여부 순으로 우선순위를 산정합니다. 추천 결과 자체는 LLM이 생성하지 않습니다.
3. **추천 근거 시각화**
   사용자의 입력 문장이 어떤 특징으로 해석되었는지 분석 결과 카드로 제공하여, 추천 과정을 블랙박스가 아닌 설명 가능한 형태로 제시합니다.
4. **생성형 AI 이미지 보완**
   사진 자료가 부족한 음식에 한해 이름·재료·조리법·특징을 반영한 보조 이미지를 생성합니다(원문 사진의 대체가 아닌 이해를 돕는 시각 자료).

---

## 🏗️ 서비스 아키텍처

```
[Client App]            [Backend Server]              [External]
React Native/Expo        Spring Boot 4 + Nginx          Google Gemini API
TanStack Query · Jotai   JWT Auth · Spring Security      V-WORLD (전통시장)
AsyncStorage · i18next   AWS S3 (이미지)
        │                       │
        │      REST API         │
        └──────────────────────►│
                                 │
                          [MySQL Database]
                                 ▲
                                 │
                     GitHub Actions (CI/CD)
                     main push → Gradle Build → AWS EC2
```

- **프론트엔드**: Expo Router 파일 기반 라우팅, TanStack Query로 캐싱·무한 스크롤·낙관적 업데이트, Jotai로 클라이언트 상태 분리 관리, Figma 기반 한국 전통 디자인 토큰(한지 베이지 · 단청 적갈색 · 산림 초록) 적용
- **백엔드**: Spring Boot 4, Google ID 토큰 검증 후 자체 JWT 발급, Spring Security 필터 기반 인가, AWS S3 이미지 업로드, Springdoc(OpenAPI/Swagger) 문서 자동화
- **CI/CD**: GitHub Actions가 `main` 브랜치 푸시를 감지해 Gradle 빌드 후 AWS EC2로 자동 배포

---

## 🛠️ 기술 스택

<table>
<tr>
<td valign="top" width="50%">

**Frontend**
- React Native (Expo SDK 52)
- Turborepo + pnpm 모노레포
- Expo Router
- TanStack Query v5
- Jotai
- NativeWind
- i18next (4개 언어)
- AsyncStorage

</td>
<td valign="top" width="50%">

**Backend**
- Spring Boot 4 / Spring AI
- Spring Security + JWT
- Google API Client (OAuth)
- MySQL
- AWS S3 / AWS EC2
- Springdoc (OpenAPI/Swagger)
- GitHub Actions (CI/CD)

</td>
</tr>
</table>

**AI / 외부 연동**: Google Gemini API(자연어 특징 추출), Nano Banana(이미지 생성), V-WORLD OpenAPI(전통시장 정보)

---

## 🗂️ 활용 문화데이터

| 데이터명 | 제공기관 | 출처 플랫폼 |
|---|---|---|
| 지식재산처_한국전통 식품정보 | 행정안전부 | [공공데이터포털](https://www.data.go.kr/data/15002149/openapi.do) |
| 전통시장현황 | 국토교통부 | [V-WORLD](https://www.vworld.kr/dev/v4dv_2ddataguide2_s002.do) |

공공데이터는 원본을 그대로 사용하지 않고, 음식 단위 재구성 → 식재료·조리 과정 파싱 → 폐쇄형 카테고리/특징 부여의 전처리·가공 과정을 거쳐 추천 및 콘텐츠 데이터로 활용됩니다.

---

## 📂 Repository

| Repository | 설명 |
|---|---|
| `OngoJishik-FE` | React Native / Expo 프론트엔드 모노레포 |
| `OngoJishik-BE` | Spring Boot 백엔드 서버 |

---

## 📱 화면 미리보기

> 로그인 · 홈 · 마이페이지 · 커뮤니티 · 음식 검색 · 음식 상세 · 전통시장 · 다국어 지원 화면 등은 [데모 자료](https://drive.google.com/drive/folders/1LkBdjfRcD4nkmWi6s2MbsdbyVXf2SHfr)에서 확인하실 수 있습니다.

---

<div align="center">

**온고지식** — 옛것을 익혀 새로운 식문화를 잇다 🍶

</div>
