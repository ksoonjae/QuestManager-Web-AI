# API 명세 설계

## 1. 목적

Quest Manager 프로젝트에서 사용할 기본 API 명세를 정의한다.

이 문서는 프론트엔드와 백엔드가 NPC, Quest, Reward 데이터를 어떤 방식으로 주고받을지 정리하기 위한 문서이다.

초기 MVP에서는 관리자 웹에서 사용하는 CRUD API를 중심으로 설계한다.

---

## 2. API 설계 기준

### 기본 URL

```http
/api
```

### 응답 형식

모든 API 응답은 JSON 형식을 사용한다.

### 공통 성공 응답 예시

```json
{
  "success": true,
  "data": {}
}
```

### 공통 실패 응답 예시

```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "요청한 데이터를 찾을 수 없습니다."
  }
}
```

---

## 3. API 목록 요약

| 도메인         | Method | Endpoint                                          | 설명                |
| ----------- | ------ | ------------------------------------------------- | ----------------- |
| NPC         | GET    | `/api/npcs`                                       | NPC 목록 조회         |
| NPC         | GET    | `/api/npcs/{npcId}`                               | NPC 상세 조회         |
| NPC         | POST   | `/api/npcs`                                       | NPC 생성            |
| NPC         | PATCH  | `/api/npcs/{npcId}`                               | NPC 수정            |
| NPC         | DELETE | `/api/npcs/{npcId}`                               | NPC 삭제            |
| Quest       | GET    | `/api/quests`                                     | Quest 목록 조회       |
| Quest       | GET    | `/api/quests/{questId}`                           | Quest 상세 조회       |
| Quest       | POST   | `/api/quests`                                     | Quest 생성          |
| Quest       | PATCH  | `/api/quests/{questId}`                           | Quest 수정          |
| Quest       | DELETE | `/api/quests/{questId}`                           | Quest 삭제          |
| Reward      | GET    | `/api/rewards`                                    | Reward 목록 조회      |
| Reward      | GET    | `/api/rewards/{rewardId}`                         | Reward 상세 조회      |
| Reward      | POST   | `/api/rewards`                                    | Reward 생성         |
| Reward      | PATCH  | `/api/rewards/{rewardId}`                         | Reward 수정         |
| Reward      | DELETE | `/api/rewards/{rewardId}`                         | Reward 삭제         |
| QuestReward | POST   | `/api/quests/{questId}/rewards`                   | Quest에 Reward 연결  |
| QuestReward | DELETE | `/api/quests/{questId}/rewards/{rewardId}`        | Quest에서 Reward 제거 |
| PlayerQuest | GET    | `/api/players/{playerId}/quests`                  | 플레이어 퀘스트 목록 조회    |
| PlayerQuest | PATCH  | `/api/players/{playerId}/quests/{questId}/status` | 플레이어 퀘스트 상태 변경    |

---

## 4. NPC API

## 4.1 NPC 목록 조회

### Request

```http
GET /api/npcs
```

### Query Parameters

| 이름       | 타입      | 필수 여부 | 설명        |
| -------- | ------- | ----- | --------- |
| keyword  | string  | 선택    | NPC 이름 검색 |
| isActive | boolean | 선택    | 사용 여부 필터  |

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "마을 경비병",
      "description": "마을 입구를 지키는 경비병이다.",
      "role": "QUEST_GIVER",
      "location": "Village Entrance",
      "imageUrl": "/images/npcs/guard.png",
      "isActive": true,
      "createdAt": "2026-07-01T10:00:00",
      "updatedAt": "2026-07-01T10:00:00"
    }
  ]
}
```

---

## 4.2 NPC 상세 조회

### Request

```http
GET /api/npcs/1
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "마을 경비병",
    "description": "마을 입구를 지키는 경비병이다.",
    "role": "QUEST_GIVER",
    "location": "Village Entrance",
    "imageUrl": "/images/npcs/guard.png",
    "isActive": true,
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T10:00:00"
  }
}
```

---

## 4.3 NPC 생성

### Request

```http
POST /api/npcs
```

### Request Body

```json
{
  "name": "마을 경비병",
  "description": "마을 입구를 지키는 경비병이다.",
  "role": "QUEST_GIVER",
  "location": "Village Entrance",
  "imageUrl": "/images/npcs/guard.png",
  "isActive": true
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "마을 경비병",
    "description": "마을 입구를 지키는 경비병이다.",
    "role": "QUEST_GIVER",
    "location": "Village Entrance",
    "imageUrl": "/images/npcs/guard.png",
    "isActive": true,
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T10:00:00"
  }
}
```

---

## 4.4 NPC 수정

### Request

```http
PATCH /api/npcs/1
```

### Request Body

```json
{
  "name": "수정된 경비병",
  "location": "Town Gate",
  "isActive": true
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "수정된 경비병",
    "description": "마을 입구를 지키는 경비병이다.",
    "role": "QUEST_GIVER",
    "location": "Town Gate",
    "imageUrl": "/images/npcs/guard.png",
    "isActive": true,
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T11:00:00"
  }
}
```

---

## 4.5 NPC 삭제

### Request

```http
DELETE /api/npcs/1
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1
  }
}
```

MVP에서는 실제 DB 삭제 대신 `isActive`를 `false`로 변경하는 soft delete 방식도 고려할 수 있다.

---

# 5. Quest API

## 5.1 Quest 목록 조회

### Request

```http
GET /api/quests
```

### Query Parameters

| 이름      | 타입     | 필수 여부 | 설명              |
| ------- | ------ | ----- | --------------- |
| npcId   | number | 선택    | 특정 NPC의 퀘스트만 조회 |
| status  | string | 선택    | Quest 운영 상태 필터  |
| type    | string | 선택    | Quest 유형 필터     |
| keyword | string | 선택    | 제목 검색           |

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "npcId": 1,
      "title": "마을 주변 정리",
      "description": "마을 주변에 나타난 슬라임을 처치해 주세요.",
      "objective": "슬라임 5마리 처치",
      "type": "KILL",
      "status": "ACTIVE",
      "requiredLevel": 1,
      "isRepeatable": false,
      "createdAt": "2026-07-01T10:00:00",
      "updatedAt": "2026-07-01T10:00:00"
    }
  ]
}
```

---

## 5.2 Quest 상세 조회

### Request

```http
GET /api/quests/1
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "npcId": 1,
    "title": "마을 주변 정리",
    "description": "마을 주변에 나타난 슬라임을 처치해 주세요.",
    "objective": "슬라임 5마리 처치",
    "type": "KILL",
    "status": "ACTIVE",
    "requiredLevel": 1,
    "isRepeatable": false,
    "rewards": [
      {
        "id": 1,
        "name": "골드",
        "type": "GOLD",
        "amount": 500
      },
      {
        "id": 2,
        "name": "경험치",
        "type": "EXP",
        "amount": 100
      }
    ],
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T10:00:00"
  }
}
```

---

## 5.3 Quest 생성

### Request

```http
POST /api/quests
```

### Request Body

```json
{
  "npcId": 1,
  "title": "마을 주변 정리",
  "description": "마을 주변에 나타난 슬라임을 처치해 주세요.",
  "objective": "슬라임 5마리 처치",
  "type": "KILL",
  "status": "DRAFT",
  "requiredLevel": 1,
  "isRepeatable": false
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "npcId": 1,
    "title": "마을 주변 정리",
    "description": "마을 주변에 나타난 슬라임을 처치해 주세요.",
    "objective": "슬라임 5마리 처치",
    "type": "KILL",
    "status": "DRAFT",
    "requiredLevel": 1,
    "isRepeatable": false,
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T10:00:00"
  }
}
```

---

## 5.4 Quest 수정

### Request

```http
PATCH /api/quests/1
```

### Request Body

```json
{
  "title": "마을 주변 슬라임 처치",
  "status": "ACTIVE",
  "requiredLevel": 2
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "npcId": 1,
    "title": "마을 주변 슬라임 처치",
    "description": "마을 주변에 나타난 슬라임을 처치해 주세요.",
    "objective": "슬라임 5마리 처치",
    "type": "KILL",
    "status": "ACTIVE",
    "requiredLevel": 2,
    "isRepeatable": false,
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T11:00:00"
  }
}
```

---

## 5.5 Quest 삭제

### Request

```http
DELETE /api/quests/1
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1
  }
}
```

---

# 6. Reward API

## 6.1 Reward 목록 조회

### Request

```http
GET /api/rewards
```

### Query Parameters

| 이름      | 타입     | 필수 여부 | 설명       |
| ------- | ------ | ----- | -------- |
| type    | string | 선택    | 보상 유형 필터 |
| keyword | string | 선택    | 보상 이름 검색 |

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "골드",
      "type": "GOLD",
      "value": 500,
      "description": "퀘스트 완료 보상으로 지급되는 골드",
      "iconUrl": "/images/rewards/gold.png",
      "createdAt": "2026-07-01T10:00:00",
      "updatedAt": "2026-07-01T10:00:00"
    }
  ]
}
```

---

## 6.2 Reward 상세 조회

### Request

```http
GET /api/rewards/1
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "골드",
    "type": "GOLD",
    "value": 500,
    "description": "퀘스트 완료 보상으로 지급되는 골드",
    "iconUrl": "/images/rewards/gold.png",
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T10:00:00"
  }
}
```

---

## 6.3 Reward 생성

### Request

```http
POST /api/rewards
```

### Request Body

```json
{
  "name": "골드",
  "type": "GOLD",
  "value": 500,
  "description": "퀘스트 완료 보상으로 지급되는 골드",
  "iconUrl": "/images/rewards/gold.png"
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "골드",
    "type": "GOLD",
    "value": 500,
    "description": "퀘스트 완료 보상으로 지급되는 골드",
    "iconUrl": "/images/rewards/gold.png",
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T10:00:00"
  }
}
```

---

## 6.4 Reward 수정

### Request

```http
PATCH /api/rewards/1
```

### Request Body

```json
{
  "value": 700,
  "description": "수정된 골드 보상"
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "골드",
    "type": "GOLD",
    "value": 700,
    "description": "수정된 골드 보상",
    "iconUrl": "/images/rewards/gold.png",
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T11:00:00"
  }
}
```

---

## 6.5 Reward 삭제

### Request

```http
DELETE /api/rewards/1
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1
  }
}
```

---

# 7. QuestReward API

Quest와 Reward는 다대다 관계를 가질 수 있으므로, Quest에 Reward를 연결하는 API를 별도로 둔다.

## 7.1 Quest에 Reward 연결

### Request

```http
POST /api/quests/1/rewards
```

### Request Body

```json
{
  "rewardId": 1,
  "amount": 500
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "questId": 1,
    "rewardId": 1,
    "amount": 500,
    "createdAt": "2026-07-01T10:00:00",
    "updatedAt": "2026-07-01T10:00:00"
  }
}
```

---

## 7.2 Quest에서 Reward 제거

### Request

```http
DELETE /api/quests/1/rewards/1
```

### Response

```json
{
  "success": true,
  "data": {
    "questId": 1,
    "rewardId": 1
  }
}
```

---

# 8. PlayerQuest API

PlayerQuest API는 플레이어별 퀘스트 진행 상태를 관리하기 위한 API이다.

MVP 관리자 페이지에서는 필수 구현 대상은 아니지만, Quest 상태 흐름 설계를 반영하기 위해 명세 초안만 작성한다.

## 8.1 플레이어 퀘스트 목록 조회

### Request

```http
GET /api/players/10/quests
```

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "playerId": 10,
      "questId": 1,
      "status": "IN_PROGRESS",
      "currentCount": 2,
      "requiredCount": 5,
      "acceptedAt": "2026-07-02T09:00:00",
      "completedAt": null,
      "rewardedAt": null
    }
  ]
}
```

---

## 8.2 플레이어 퀘스트 상태 변경

### Request

```http
PATCH /api/players/10/quests/1/status
```

### Request Body

```json
{
  "status": "COMPLETED"
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "playerId": 10,
    "questId": 1,
    "status": "COMPLETED",
    "currentCount": 5,
    "requiredCount": 5,
    "acceptedAt": "2026-07-02T09:00:00",
    "completedAt": "2026-07-02T09:30:00",
    "rewardedAt": null
  }
}
```

---

# 9. 상태 값 정리

## 9.1 Quest 운영 상태

Quest 데이터 자체의 운영 상태이다.

| 상태       | 설명   |
| -------- | ---- |
| DRAFT    | 작성 중 |
| ACTIVE   | 사용 중 |
| INACTIVE | 비활성화 |
| ARCHIVED | 보관됨  |

---

## 9.2 PlayerQuest 진행 상태

플레이어별 퀘스트 진행 상태이다.

| 상태          | 설명       |
| ----------- | -------- |
| AVAILABLE   | 수락 가능    |
| ACCEPTED    | 수락함      |
| IN_PROGRESS | 진행 중     |
| COMPLETED   | 완료 조건 달성 |
| REWARDED    | 보상 지급 완료 |

---

# 10. 에러 응답

## 10.1 데이터를 찾을 수 없는 경우

```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "요청한 데이터를 찾을 수 없습니다."
  }
}
```

## 10.2 잘못된 요청 값

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "입력값이 올바르지 않습니다.",
    "details": [
      {
        "field": "name",
        "message": "NPC 이름은 필수입니다."
      }
    ]
  }
}
```

## 10.3 서버 오류

```json
{
  "success": false,
  "error": {
    "code": "INTERNAL_SERVER_ERROR",
    "message": "서버 오류가 발생했습니다."
  }
}
```

---

# 11. MVP 구현 범위

초기 MVP에서는 다음 API를 우선 구현한다.

| 우선순위 | API                                  | 이유                   |
| ---- | ------------------------------------ | -------------------- |
| 1    | `GET /api/npcs`                      | NPC 목록 화면 구현에 필요     |
| 2    | `GET /api/npcs/{npcId}`              | NPC 상세 화면 구현에 필요     |
| 3    | `POST /api/npcs`                     | NPC 생성 기능에 필요        |
| 4    | `PATCH /api/npcs/{npcId}`            | NPC 수정 기능에 필요        |
| 5    | `GET /api/quests`                    | Quest 목록 화면 구현에 필요   |
| 6    | `POST /api/quests`                   | Quest 생성 기능에 필요      |
| 7    | `GET /api/rewards`                   | Quest 생성 시 보상 선택에 필요 |
| 8    | `POST /api/quests/{questId}/rewards` | Quest에 Reward 연결 필요  |

PlayerQuest API는 실제 게임 연동 또는 Unity 클라이언트 연동 단계에서 구현한다.

---

# 12. 설계 결정

## 12.1 REST 방식 사용

API는 REST 방식으로 설계한다.

이유는 다음과 같다.

* URL만 봐도 어떤 리소스를 다루는지 이해하기 쉽다.
* CRUD 기능을 표현하기 좋다.
* 포트폴리오 프로젝트에서 설명하기 쉽다.

---

## 12.2 Quest 운영 상태와 PlayerQuest 진행 상태 분리

Quest의 `status`는 관리자용 운영 상태이다.

예를 들어 `DRAFT`, `ACTIVE`, `INACTIVE`, `ARCHIVED`가 있다.

반면 PlayerQuest의 `status`는 플레이어별 진행 상태이다.

예를 들어 `AVAILABLE`, `ACCEPTED`, `IN_PROGRESS`, `COMPLETED`, `REWARDED`가 있다.

두 상태를 분리하는 이유는 같은 Quest라도 플레이어마다 진행 상태가 다를 수 있기 때문이다.

---

## 12.3 COMPLETED와 REWARDED 분리

퀘스트 완료와 보상 지급은 별도의 단계로 처리한다.

이렇게 분리하면 다음 장점이 있다.

* 완료했지만 아직 보상을 받지 않은 상태를 표현할 수 있다.
* 보상 중복 지급을 방지할 수 있다.
* 보상 지급 실패 상황을 처리할 수 있다.
