# Database Design

## 주요 테이블 후보

### NPC
- id
- name
- role
- description

### Quest
- id
- npc_id
- title
- description
- status
- difficulty
- reward_id

### Reward
- id
- name
- type
- value

### DailyTask
- id
- quest_id
- required_count
- completed_count
- date
