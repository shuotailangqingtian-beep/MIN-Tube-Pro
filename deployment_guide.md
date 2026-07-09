# MIN-Tube-Pro マルチプラットフォーム・デプロイガイド

GitHubリポジトリ [MIN-Tube-Pro](https://github.com/shuotailangqingtian-beep/MIN-Tube-Pro) を、元のコードを変更することなく、主要なクラウドプラットフォーム（Render, Vercel, Railway, Replit, CodeSandbox, StackBlitz）にデプロイするための設定ファイルをまとめました。

これらのファイルをプロジェクトのルートディレクトリに追加することで、各プラットフォームでの自動認識とスムーズな起動が可能になります。

---

## 各プラットフォーム用設定ファイル一覧

| プラットフォーム | 設定ファイル名 | 役割 |
| :--- | :--- | :--- |
| **Vercel** | `vercel.json` | Node.jsランタイムの指定とルーティング設定 |
| **Render** | `render.yaml` | サービス構成、ビルド・起動コマンド、Nodeバージョンの定義 |
| **Railway** | `railway.json` | 環境変数（Nodeバージョン）と起動コマンドの定義 |
| **Replit** | `.replit` | 実行コマンドと使用するモジュールの定義 |
| **CodeSandbox** | `sandbox.config.json` | テンプレート（Node）と起動コマンドの指定 |
| **StackBlitz** | `.stackblitzrc` | インストールと起動プロセスの自動化 |

---

## 設定ファイルの詳細内容

各ファイルの内容は以下の通りです。これらをコピーして、それぞれのファイル名でリポジトリのルートに保存してください。

### 1. Vercel (`vercel.json`)
VercelでExpressアプリケーションを動作させるための設定です。

```json
{
  "version": 2,
  "builds": [
    {
      "src": "index.js",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "index.js"
    }
  ]
}
```

### 2. Render (`render.yaml`)
Renderの「Blueprint」機能を使用して、ボタン一つでデプロイ可能にします。

```yaml
version: 1
services:
  - type: web
    name: min-tube-pro
    env: node
    plan: free
    buildCommand: npm install
    startCommand: npm start
    autoDeploy: true
    envVars:
      - key: NODE_VERSION
        value: 18
```

### 3. Railway (`railway.json`)
Railwayでのビルド環境と実行プロセスを最適化します。

```json
{
  "build": {
    "env": {
      "NODE_VERSION": "18"
    }
  },
  "start": {
    "command": "node index.js"
  }
}
```

### 4. Replit (`.replit`)
Replitのワークスペースで自動的にサーバーが起動するように設定します。

```toml
run = "npm start"
entrypoint = "index.js"
modules = ["nodejs-18"]
```

### 5. CodeSandbox (`sandbox.config.json`)
CodeSandboxのコンテナ環境をNode.js用に最適化します。

```json
{
  "template": "node",
  "start": "node index.js"
}
```

### 6. StackBlitz (`.stackblitzrc`)
WebContainers上でプロジェクトを即座に実行可能にします。

```json
{
  "install": "npm install",
  "start": "node index.js"
}
```

---

## デプロイ時の注意点

1.  **ポート番号**: 元のコードは `process.env.PORT || 3000` を使用しており、全てのプラットフォームで標準的な設定です。
2.  **ホストバインド**: `index.js` 内で `0.0.0.0` にバインドされているため、外部からのアクセスも問題ありません。
3.  **Node.js バージョン**: 安定した動作のため、全てのプラットフォームで **Node.js 18以上** を指定しています。

これらの設定ファイルをリポジトリに追加するだけで、コード本体には一切手を加えずにデプロイを完了させることができます。
