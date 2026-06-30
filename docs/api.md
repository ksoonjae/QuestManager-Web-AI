# API Design

## NPC API

| Method | Endpoint | Description |
|---|---|---|
| GET | /api/npcs | NPC 목록 조회 |
| POST | /api/npcs | NPC 등록 |
| GET | /api/npcs/{id} | NPC 상세 조회 |
| PUT | /api/npcs/{id} | NPC 수정 |
| DELETE | /api/npcs/{id} | NPC 삭제 |

## Quest API

| Method | Endpoint | Description |
|---|---|---|
| GET | /api/quests | 퀘스트 목록 조회 |
| POST | /api/quests | 퀘스트 등록 |
| PUT | /api/quests/{id} | 퀘스트 수정 |
| PATCH | /api/quests/{id}/complete | 퀘스트 완료 처리 |
| DELETE | /api/quests/{id} | 퀘스트 삭제 |
