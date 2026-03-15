# GPURental SaaS Kit

> **GPU時間貸しSaaSをゼロから立ち上げるための完全パッケージ**  
> Developed by [METADATALAB.INC](https://metadatalab.jp)

---

## 🚀 概要

GPURental SaaS Kit は、GPU時間貸しプラットフォームを独自ブランドで展開したい事業者向けのホワイトラベルパッケージです。RunPodと同等の機能を日本市場向けに最適化した状態で提供します。

### 主な機能

| 機能 | 説明 |
|---|---|
| 🖥️ GPU管理 | プロバイダーのGPUをリモート管理（nvidia-smi連携） |
| 📅 予約・課金 | カレンダー予約 + ポイント制課金 |
| 💳 決済連携 | GMOイプシロン・Stripe対応（切り替え可能） |
| 🐳 Dockerテンプレ | PyTorch/ComfyUI/JupyterLab等の即起動環境 |
| 🎟️ クーポン | 割引コード発行・管理 |
| 🔑 APIキー | プログラマティックアクセス（gpr_xxx形式） |
| 📊 RunPod価格監視 | 競合比較・推奨価格自動計算 |
| 👤 マルチロール | 管理者・プロバイダー・ユーザーの3ロール |

---

## 📦 ディレクトリ構造

```
gpurental-saas-kit/
├── docs/
│   ├── 01_architecture.md      # システムアーキテクチャ
│   ├── 02_deployment.md        # 本番デプロイ手順
│   ├── 03_whitelabel.md        # ホワイトラベル設定
│   ├── 04_payment.md           # 決済連携ガイド
│   └── 05_api_reference.md     # API リファレンス
├── legal/
│   ├── terms_template.md       # 利用規約テンプレート
│   └── privacy_template.md     # プライバシーポリシーテンプレート
├── scripts/
│   ├── setup.sh                # 初期セットアップスクリプト
│   └── backup.sh               # バックアップスクリプト
└── src/                        # ソースコードへのシンボリックリンク
```

---

## ⚡ クイックスタート

### 必要環境

- Node.js 18+
- Ubuntu 22.04 (推奨) / Windows Server 2022
- NVIDIA GPU + Driver 525+
- Domain + SSL証明書

### 1分セットアップ

```bash
git clone https://github.com/taichiyao333/gpu-rental.git
cd gpu-rental
cp .env.example .env.production
# .env.production を編集
npm install
node server/index.js
```

---

## 💰 ライセンス・販売

このパッケージは **METADATALAB.INC** が開発・販売しています。

| プラン | 価格 | 内容 |
|---|---|---|
| **スターター** | ¥300,000 | ソースコード + セットアップサポート1回 |
| **ビジネス** | ¥800,000 | スターター + カスタマイズ20h + 6ヶ月保守 |
| **エンタープライズ** | 要相談 | フルカスタム + 専任サポート |

お問い合わせ: contact@metadatalab.jp

---

## 🔗 関連リポジトリ

- [gpu-rental](https://github.com/taichiyao333/gpu-rental) — メインプラットフォーム
- [gpurental-docker-templates](https://github.com/taichiyao333/gpurental-docker-templates) — GPU環境Dockerテンプレート集
