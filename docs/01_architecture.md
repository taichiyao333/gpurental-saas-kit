# システムアーキテクチャ

## 全体構成図

```
[ユーザー] ──HTTPS──► [Cloudflare Tunnel]
                              │
                    ┌─────────▼─────────┐
                    │  Node.js Express   │ :3000
                    │    (サーバー)      │
                    └────┬──────┬────────┘
                         │      │
                  ┌──────▼──┐ ┌─▼──────────┐
                  │ SQLite  │ │ Socket.IO  │
                  │  (DB)   │ │ (WS)       │
                  └─────────┘ └────────────┘
                         │
              ┌──────────▼──────────┐
              │    外部サービス      │
              │  GMOイプシロン決済  │
              │  Cloudflare Tunnel  │
              │  RunPod API (監視)  │
              └─────────────────────┘
```

## コンポーネント一覧

### フロントエンド (/public)

| パス | 役割 |
|---|---|
| `/landing/` | マーケティングLP (SEO最適化) |
| `/portal/` | ユーザー向けメイン画面 (GPU予約・ポイント) |
| `/workspace/` | GPU実行環境 (xterm.js + Chart.js) |
| `/admin/` | 管理パネル (GPU/クーポン/収益管理) |
| `/mypage/` | ユーザープロフィール・APIキー管理 |
| `/provider/` | GPU提供者向けポータル |

### バックエンド (/server)

| パス | 役割 |
|---|---|
| `server/index.js` | Expressアプリ + WebSocket |
| `server/routes/` | APIエンドポイント群 |
| `server/db/` | SQLite (better-sqlite3) |
| `server/services/` | RunPod価格監視・スケジューラー |
| `server/middleware/` | JWT認証・管理者チェック |

### データベース テーブル

| テーブル | 説明 |
|---|---|
| `users` | ユーザー情報・ポイント残高 |
| `gpus` | GPU登録・スペック・価格 |
| `pods` | 実行Pod（起動中のGPU） |
| `reservations` | 予約情報 |
| `point_purchases` | ポイント購入履歴 |
| `point_logs` | ポイント増減ログ |
| `user_api_keys` | APIキー (SHA-256ハッシュ) |
| `coupons` | クーポン情報 |
| `coupon_usages` | クーポン使用履歴 |
| `runpod_pricing_snapshots` | RunPod価格スナップショット |
| `payout_requests` | 出金申請 |
| `bank_accounts` | 振込先口座 |
| `outage_events` | 障害情報 |

## セキュリティ

- **JWT認証**: HS256, 7日有効期限
- **APIキー**: SHA-256ハッシュのみDB保存（プレーンキーは発行時のみ表示）
- **パスワード**: bcrypt (saltRounds=12)
- **管理画面**: IPアドレス制限推奨 + 追加パスワード保護
- **通信**: Cloudflare経由でHTTPS強制
