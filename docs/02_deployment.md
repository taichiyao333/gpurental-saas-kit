# 本番デプロイ手順

## 必要要件

| 項目 | 最小要件 | 推奨 |
|---|---|---|
| OS | Ubuntu 22.04 | Ubuntu 22.04 LTS |
| CPU | 2コア | 4コア以上 |
| RAM | 4GB | 8GB以上 |
| ストレージ | 50GB SSD | 100GB SSD |
| Node.js | 18.x | 20.x |
| ドメイン | 必須 | サブドメイン可 |

## 1. サーバー初期設定

```bash
# Node.js 20.x インストール
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# PM2 (プロセスマネージャー) インストール
npm install -g pm2

# リポジトリクローン
git clone https://github.com/taichiyao333/gpu-rental.git /var/www/gpurental
cd /var/www/gpurental
npm install
```

## 2. 環境変数設定

```bash
cp .env.example .env.production
nano .env.production
```

```env
NODE_ENV=production
PORT=3000
JWT_SECRET=<ランダムな64文字以上の文字列>
ADMIN_EMAIL=admin@yourdomain.com
ADMIN_PASSWORD=<強力なパスワード>
SITE_URL=https://yourdomain.com

# GMOイプシロン
EPSILON_CONTRACT_CODE=<契約番号>
EPSILON_MERCHANT_ID=<マーチャントID>

# Cloudflare (任意)
CF_TUNNEL_TOKEN=<トークン>
```

## 3. PM2での起動

```bash
# 起動
NODE_ENV=production pm2 start server/index.js --name gpurental

# 自動起動設定
pm2 startup
pm2 save

# ログ確認
pm2 logs gpurental
```

## 4. Nginx リバースプロキシ設定

```nginx
server {
    listen 443 ssl http2;
    server_name yourdomain.com;

    ssl_certificate /etc/ssl/yourdomain.crt;
    ssl_certificate_key /etc/ssl/yourdomain.key;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## 5. Cloudflare Tunnel（代替）

```bash
# cloudflaredインストール
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb

# トンネル起動
cloudflared tunnel --url http://localhost:3000
```

## 6. データベースバックアップ

```bash
# 日次バックアップ (crontab)
0 3 * * * cp /var/www/gpurental/data/gpu_rental.db /backup/gpu_rental_$(date +%Y%m%d).db
```

## よくある問題

| 問題 | 解決策 |
|---|---|
| 503エラー | `pm2 status` でプロセス確認 |
| DB権限エラー | `chmod 770 data/` で権限修正 |
| WebSocket接続失敗 | Nginxのプロキシ設定でUpgradeヘッダー確認 |
| 決済コールバック届かない | イプシロン管理画面でURLを正しく設定 |
