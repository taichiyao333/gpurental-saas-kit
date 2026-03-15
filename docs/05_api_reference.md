# API リファレンス

**Base URL**: `https://gpurental.jp/api`  
**認証**: `Authorization: Bearer <token_or_api_key>`

---

## 認証 (Auth)

### ログイン
```http
POST /api/auth/login
Content-Type: application/json

{ "email": "user@example.com", "password": "..." }
```
**レスポンス**:
```json
{
  "token": "eyJhbGci...",
  "user": { "id": 1, "username": "...", "role": "user" }
}
```

### ユーザー登録
```http
POST /api/auth/register
Content-Type: application/json

{ "username": "...", "email": "...", "password": "..." }
```

---

## ポイント (Points)

### 残高確認
```http
GET /api/points/balance
Authorization: Bearer <token>
```
```json
{ "point_balance": 500, "yen_value": 5000 }
```

### 利用履歴
```http
GET /api/points/logs
Authorization: Bearer <token>
```

---

## GPU

### GPU一覧
```http
GET /api/gpus
```
```json
[
  {
    "id": 1, "name": "NVIDIA RTX A4500",
    "status": "available",
    "price_per_hour": 800,
    "vram_total": 20480
  }
]
```

---

## 予約 (Reservations)

### 予約作成
```http
POST /api/reservations
Authorization: Bearer <token>
Content-Type: application/json

{
  "gpu_id": 1,
  "start_time": "2026-03-16T10:00:00+09:00",
  "end_time": "2026-03-16T14:00:00+09:00",
  "notes": "AI/機械学習",
  "docker_template": "pytorch"
}
```

### 予約一覧
```http
GET /api/reservations
Authorization: Bearer <token>
```

---

## APIキー (API Keys)

### 発行
```http
POST /api/user/apikeys
Authorization: Bearer <token>
Content-Type: application/json

{ "name": "CI/CD用" }
```
```json
{
  "success": true,
  "api_key": "gpr_xxxxxxxxxxxx...",
  "key_prefix": "gpr_xxxxxxxx...",
  "note": "このキーは一度しか表示されません"
}
```

### 一覧
```http
GET /api/user/apikeys
Authorization: Bearer <token>
```

### 削除
```http
DELETE /api/user/apikeys/:id
Authorization: Bearer <token>
```

---

## クーポン (Coupons)

### バリデーション
```http
POST /api/coupons/validate
Content-Type: application/json

{ "code": "BETA2025", "amount": 1000 }
```
```json
{
  "valid": true,
  "discount_type": "percent",
  "discount_value": 20,
  "discount_amount": 200,
  "final_amount": 800
}
```

---

## 料金 (Prices)

### GPU料金一覧
```http
GET /api/prices
```
```json
[
  { "gpu_name": "RTX 3070", "price_per_hour": 600 }
]
```

---

## エラーレスポンス

| コード | 説明 |
|---|---|
| 400 | バリデーションエラー |
| 401 | 認証失敗（トークン無効/期限切れ） |
| 403 | 権限不足 |
| 404 | リソースが見つからない |
| 429 | レート制限 |
| 500 | サーバーエラー |

すべてのエラーは `{ "error": "メッセージ" }` 形式で返します。
