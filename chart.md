erDiagram
    USERS {
        SERIAL id PK
        VARCHAR email UK
        VARCHAR phone UK
        VARCHAR password
        VARCHAR role
        TIMESTAMP created_at
        TIMESTAMP updated_at
        VARCHAR status
        INT vip_level
        INT risk_flag_score
        INT login_try_count
        VARCHAR active_session_id FK
        TIMESTAMP last_login_at
    }

    SESSION {
        VARCHAR id PK
        INT user_id FK
        VARCHAR device_id
        TEXT device_info
        INET ip_address
        TIMESTAMP created_at
        TIMESTAMP expires_at
        TIMESTAMP revoked_at
        VARCHAR revoked_reason
        BOOLEAN is_active
    }

    KYC_DOCUMENTS {
        INT user_id PK,FK
        VARCHAR national_id
        VARCHAR first_name
        VARCHAR last_name
        DATE birthdate
        VARCHAR gender
        TEXT address
        VARCHAR postal_code
        VARCHAR phone_fixed
        TEXT user_photo
        TEXT id_card_photo
        TEXT selfie_video
        TEXT tos_photo
        VARCHAR user_photo_status
        VARCHAR email_status
        VARCHAR id_card_status
        VARCHAR selfie_video_status
        VARCHAR tos_upload_status
        TIMESTAMP updated_at
    }

    VIP_SETTINGS {
        INT vip_level PK
        INT credit
        INT payment_timeout
        INT daily_trade_limit
    }

    KYC_VIP_SETTINGS {
        INT id PK
        VARCHAR kyc_approval
        INT min_vip_level
    }

    WALLET {
        SERIAL id PK
        INT user_id FK
        VARCHAR asset
        DECIMAL balance
        DECIMAL locked_balance
        DECIMAL cumulative_balance
        UNIQUE user_id_asset
    }

    ORDERS {
        SERIAL id PK
        INT user_id FK
        VARCHAR side
        VARCHAR asset
        DECIMAL amount
        DECIMAL unit_price
        DECIMAL total_price GENERATED
        VARCHAR type
        VARCHAR status
        VARCHAR payment
        DECIMAL order_locked_balance
        DECIMAL order_credit_used
        TIMESTAMP created_at
        TIMESTAMP updated_at
        TIMESTAMP expires_at
        INT last_transaction_id
    }

    TRADES {
        SERIAL id PK
        INT user_id FK
        INT order_id FK,UK
        VARCHAR type
        VARCHAR asset
        DECIMAL quantity
        DECIMAL unit_price
        DECIMAL total_price
        DECIMAL fee
        DECIMAL developer_fee
        TIMESTAMP executed_at
        INT wallet_id FK
        DECIMAL balance_after
    }

    TRANSACTIONS {
        SERIAL id PK
        INT user_id FK
        VARCHAR type
        VARCHAR channel
        VARCHAR asset
        INT wallet_id FK
        DECIMAL amount
        VARCHAR status
        VARCHAR status_physical
        VARCHAR transaction_hash UK
        INT processed_by_user_id FK
        VARCHAR processed_by_type
        TIMESTAMP created_at
        TIMESTAMP updated_at
        TIMESTAMP approved_at
        TIMESTAMP completed_at
        TIMESTAMP delivered_at
        DECIMAL balance_before
        TEXT receipt_photo
        JSONB metadata
    }

    WITHDRAWAL_ADDRESS {
        SERIAL id PK
        INT user_id FK
        VARCHAR account_type
        TEXT account_address
        VARCHAR bank_card_no
        VARCHAR bank_name
        VARCHAR bank_branch
        BOOLEAN is_whitelisted
        TIMESTAMP created_at
        TIMESTAMP updated_at
        UNIQUE user_id_account_type
    }

    PAYMENT_ADDRESS {
        SERIAL id PK
        VARCHAR account_type
        TEXT account_address
        VARCHAR bank_card_no
        UNIQUE account_type_account_address
    }

    PHYSICAL_TREASURE {
        SERIAL id PK
        DECIMAL weight
        INT hallmark
        VARCHAR laboratory_code
        VARCHAR laboratory_name
        VARCHAR status
        VARCHAR status_delivery
        INT nominated_user_id FK
        TEXT photo
        TEXT comment
    }

    NOTIFICATIONS {
        SERIAL id PK
        INT user_id FK
        VARCHAR source
        VARCHAR type
        TEXT message
        VARCHAR status
        TIMESTAMP created_at
        TIMESTAMP read_at
    }

    PRICES {
        SERIAL id PK
        VARCHAR item
        DECIMAL open
        DECIMAL high
        DECIMAL low
        DECIMAL close
        VARCHAR source
        BOOLEAN is_valid
        TIMESTAMP recorded_at
        TIMESTAMP expires_at
    }

    USER_SCORE_CATEGORIES {
        SERIAL id PK
        VARCHAR category UK
    }

    USER_SCORE {
        INT user_id PK,FK
        INT score_category_id PK,FK
        INT score
    }

    USER_SCORE_INTERVAL {
        INT user_id PK,FK
        DATE start_date
        DATE end_date
        INT total_score
        INT last_week_score
        TIMESTAMP updated_at
    }

    USER_SCORE_PATTERN {
        SERIAL id PK
        INT score_category_id FK,UK
        INT value
        TIMESTAMP updated_at
    }

    VERIFICATION_CODE {
        SERIAL id PK
        INT user_id FK
        VARCHAR code
        VARCHAR purpose
        TIMESTAMP created_at
        TIMESTAMP expires_at
        BOOLEAN is_used
    }

    ADMIN_SETTINGS {
        SERIAL id PK
        VARCHAR item UK
        VARCHAR unit
        VARCHAR affected_period
        DECIMAL value
        TIMESTAMP updated_at
    }

    SUPER_ADMIN_SETTINGS {
        SERIAL id PK
        VARCHAR item UK
        VARCHAR unit
        DECIMAL value
        TIMESTAMP updated_at
    }

    MESSAGES_TEMPLATE {
        SERIAL id PK
        VARCHAR message_type
        VARCHAR channel
        VARCHAR title
        TEXT body
        JSONB variables
        BOOLEAN is_active
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    SYSTEM_STATUS {
        SERIAL id PK
        VARCHAR mode
        TIMESTAMP updated_at
    }

    ADMIN_NOTE {
        SERIAL id PK
        INT admin_id FK
        INT user_id FK
        TEXT note
        TIMESTAMP created_at
    }

    ADMIN_ADJUSTMENT {
        SERIAL id PK
        INT admin_id FK
        INT user_id FK
        INT wallet_id FK
        DECIMAL amount
        TEXT reason
        TIMESTAMP created_at
    }

    ACTIVITY_LOG {
        SERIAL id PK
        INT user_id FK
        VARCHAR action
        INET ip_address
        JSONB metadata
        TIMESTAMP created_at
    }

    SYSTEM_LOG {
        SERIAL id PK
        INT user_id FK
        INET ip_address
        VARCHAR action
        JSONB metadata
        TIMESTAMP created_at
    }

    HOLIDAY_CALENDAR {
        SERIAL id PK
        DATE date UK
        TEXT description
    }

    WORK_HOURS {
        SERIAL id PK
        SMALLINT weekdays UK
        TIME buy_start_time
        TIME buy_stop_time
        TIME sell_start_time
        TIME sell_stop_time
        BOOLEAN is_buy_active
        BOOLEAN is_sell_active
    }

    SUPPORT_PERMISSIONS {
        SERIAL id PK
        INT user_id FK,UK
        BOOLEAN kyc_approve
        BOOLEAN order_approve
    }

    USERS ||--o{ SESSION : has
    USERS ||--o{ KYC_DOCUMENTS : has
    USERS ||--o{ WALLET : has
    USERS ||--o{ ORDERS : places
    USERS ||--o{ TRADES : participates
    USERS ||--o{ TRANSACTIONS : makes
    USERS ||--o{ WITHDRAWAL_ADDRESS : has
    USERS ||--o{ NOTIFICATIONS : receives
    USERS ||--o{ VERIFICATION_CODE : requests
    USERS ||--o{ ADMIN_NOTE : "written about"
    USERS ||--o{ ADMIN_ADJUSTMENT : "adjusted by"
    USERS ||--o{ ACTIVITY_LOG : generates
    USERS ||--o{ SYSTEM_LOG : "related to"
    USERS ||--o{ SUPPORT_PERMISSIONS : "has permissions"

    ORDERS ||--o| TRADES : "results in"
    WALLET ||--o{ TRADES : "updated in"
    WALLET ||--o{ TRANSACTIONS : "affects"
    WALLET ||--o{ ADMIN_ADJUSTMENT : "modified by"

    USER_SCORE_CATEGORIES ||--o{ USER_SCORE : defines
    USER_SCORE_CATEGORIES ||--o| USER_SCORE_PATTERN : defines

    USERS ||--o{ USER_SCORE : has
    USERS ||--o| USER_SCORE_INTERVAL : has