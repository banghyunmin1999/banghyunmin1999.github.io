# 방현민 Dev Blog

https://banghyunmin1999.github.io — Jekyll Chirpy 테마.

---

## 📚 카테고리 체계 (이 규칙을 따를 것)

구조: `categories: [기술 도메인, 세부 기술]` + `tags: [글 성격, 세부키워드...]`
(Chirpy는 카테고리 최대 2단 — 기술 트리를 카테고리로, 글 성격은 태그로)

| 1단 (기술 도메인) | 2단 (세부 기술) 예시 |
|---|---|
| **Python** | FastAPI, Pydantic, Logging, Pandas |
| **AI** | YOLO, vLLM, LLM, OpenCV |
| **DevOps** | Docker, K8s, WSL, Linux, AWS |
| **Web** | JavaScript, Spring |
| **DB** | MySQL, ETL |
| **회고** | (2단 없음 — 주간/월간/일상) |

글 성격은 **태그 맨 앞**에: `학습` / `트러블슈팅` / `사이드프로젝트`
나머지 태그: 소문자 세부기술 (`python`, `fastapi`, `vllm` ...)

### front matter 템플릿
```yaml
---
title: 글 제목
date: YYYY-MM-DD HH:MM:00 +0900
categories: [Python, FastAPI]
tags: [학습, python, fastapi]
---
```

---

## 🚫 회사 관련 글 규칙 (발행 전 필수 체크)

회사 업무에서 나온 경험은 **완전히 일반화**해서 쓴다.
글만 보면 어느 회사인지, 무슨 제품인지 알 수 없어야 함.

**절대 금지:**
- [ ] 회사 코드 원문
- [ ] 내부 시스템명·제품명·고객사명
- [ ] 내부 아키텍처 다이어그램, 실제 성능 수치·데이터
- [ ] 사내에서만 아는 도메인 정보

**허용:**
- 일반화된 문제 구조, 공개 기술의 사용법, 내가 배운 교훈

예시: "OO사 열화상 냙상감지 모델이..." ❌ → "열화상 영상 기반 객체 탐지에서 정규화 이슈" ✅

---

## 🔗 Notion 개발일지 → 블로그 매핑

| Notion 일지 분류 | 블로그 |
|---|---|
| 트러블슈팅 | 태그 `트러블슈팅` (일반화 필터 통과 후) |
| 학습 | 태그 `학습` |
| 회고 | 카테고리 `회고` |
| 아이디어·결정 | 원칙적 비공개 (Notion에만) |

운영 흐름: 매일 Notion 개발일지 + til 커밋 → 1~2주마다 글감 골라 블로그 발행 (Claude가 초안→확인→커밋)
