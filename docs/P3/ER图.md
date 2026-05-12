```Mermaid
erDiagram
    USERS ||--o{ DEMANDS : "publishes"
    USERS ||--o{ ORDERS : "participates in"
    USERS ||--o{ CREDIT_RECORDS : "has"
    USERS ||--o{ POINTS_TRANSACTIONS : "has"
    USERS ||--o{ REPORTS : "submits"
    DEMANDS ||--o| ORDERS : "generates"
    ORDERS ||--o| DISPUTES : "initiates"
    ORDERS ||--o| CONVERSATIONS : "creates"
    ORDERS ||--|{ REVIEWS : "receives"
    CONVERSATIONS ||--|{ MESSAGES : "contains"

    USERS {
        bigint id PK
        varchar student_id "学号(唯一)"
        varchar password_hash "BCrypt加密"
        varchar nickname
        varchar phone "AES加密存储"
        int credit_score "冗余: 快速读取"
        tinyint status "ACTIVE/BANNED"
    }

    DEMANDS {
        bigint id PK
        bigint publisher_id FK
        varchar title
        varchar category
        tinyint reward_type "POINTS/FREE/CUSTOM"
        int reward_points
        json tags
        decimal latitude "模糊化处理"
        decimal longitude "模糊化处理"
        tinyint status "OPEN/CLOSED/DELETED"
    }

    ORDERS {
        bigint id PK
        bigint demand_id FK "唯一约束: 1需求1订单"
        bigint publisher_id FK
        bigint helper_id FK
        tinyint status "状态机"
        json evidence_urls
    }

    DISPUTES {
        bigint id PK
        bigint order_id FK
        bigint initiator_id FK
        tinyint status
        varchar judge_result
    }

    CONVERSATIONS {
        bigint id PK
        bigint order_id FK
        bigint user_a_id FK
        bigint user_b_id FK
    }

    MESSAGES {
        bigint id PK
        bigint conversation_id FK
        bigint sender_id FK
        varchar content
        tinyint content_type "TEXT/IMAGE"
    }

    REVIEWS {
        bigint id PK
        bigint order_id FK
        bigint reviewer_id FK
        bigint reviewee_id FK
        tinyint rating "1-5星"
        boolean is_anonymous
    }

    CREDIT_RECORDS {
        bigint id PK
        bigint user_id FK
        int change "变动值(+/-)"
        varchar reason
    }

    POINTS_TRANSACTIONS {
        bigint id PK
        bigint user_id FK
        int amount "变动值(+/-)"
        tinyint type "DEDUCT/TRANSFER/REFUND"
        bigint order_id FK
    }

    REPORTS {
        bigint id PK
        bigint reporter_id FK
        bigint target_user_id FK
        bigint order_id FK
        json images
        tinyint status
    }
```