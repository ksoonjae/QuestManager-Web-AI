# Quest Manager Web AI

Unity 게임 프로젝트에서 사용하던 NPC 퀘스트 시스템을 웹 서비스와 AI 기능으로 확장한 포트폴리오 프로젝트입니다.

## 프로젝트 목표

기존 Unity 기반 Quest/NPC/Reward 시스템을 웹 서비스 관점에서 재설계하고,  
관리자 페이지와 AI 퀘스트 생성 기능을 추가하여 웹/AI 애플리케이션 개발 역량을 보여주는 것을 목표로 합니다.

## 주요 기능

### MVP 기능
- NPC 관리
- Quest 관리
- Reward 관리
- 일일 할당량 퀘스트 관리
- 퀘스트 완료 처리
- 보상 지급 처리

### AI 확장 기능
- AI 기반 퀘스트 설명 생성
- AI 기반 보상 추천
- 퀘스트 난이도 자동 분류
- NPC 성격에 맞는 대사 생성

## 기술 스택 예정

### Frontend
- React
- TypeScript

### Backend
- Spring Boot 또는 FastAPI

### Database
- MySQL 또는 PostgreSQL

### AI
- OpenAI API 또는 LLM API

## 폴더 구조

```txt
quest-manager-web-ai/
├── README.md
├── .gitignore
├── docs/
│   ├── planning.md
│   ├── features.md
│   ├── database.md
│   ├── api.md
│   └── troubleshooting.md
├── frontend/
├── backend/
└── assets/
    ├── screenshots/
    └── diagrams/
