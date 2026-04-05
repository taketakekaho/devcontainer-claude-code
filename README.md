# devcontainer-claude-code

Claude Code + Node.js 22 + Python 3.12 を組み込んだ Dev Container テンプレートです。

Docker コンテナの中に開発環境を丸ごと作るので、Mac 本体を汚さずに開発できます。チームで使えば、全員が同じ環境で作業できます。

## Dev Container とは？

Dev Container は「開発環境をまるごと Docker コンテナに入れる仕組み」です。

```
従来の開発:
  Mac に Node.js, Python, 各種ツールを直接インストール
  → Mac の環境が汚れる
  → チームメンバーごとに環境が微妙に違う
  → 「自分の PC では動くのに...」が起きる

Dev Container を使った開発:
  Docker コンテナの中に全部入っている
  → Mac はクリーンなまま
  → 全員が同じ環境
  → 「Reopen in Container」するだけで開発開始
```

## 前提条件

以下の 2 つが Mac にインストールされている必要があります。

### 1. Docker Desktop

コンテナを動かすためのアプリです。

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) からダウンロード・インストール
- インストール後、アプリを起動しておく（画面上部のメニューバーにクジラのアイコンが出ていれば OK）

### 2. VS Code + Dev Containers 拡張

- [VS Code](https://code.visualstudio.com/) をインストール
- VS Code を開き、左サイドバーの拡張機能アイコン（四角が 4 つ並んだマーク）をクリック
- 検索欄に「Dev Containers」と入力し、Microsoft 製の拡張機能をインストール

## 使い方

### このテンプレートから新しいプロジェクトを作る

#### 方法 A: GitHub の「Use this template」を使う（おすすめ）

1. このリポジトリの GitHub ページを開く
2. 緑色の「**Use this template**」ボタン → 「Create a new repository」をクリック
3. リポジトリ名を入力して作成
4. 作成されたリポジトリを clone する

```bash
git clone https://github.com/あなたのユーザー名/新しいリポジトリ名.git
cd 新しいリポジトリ名
```

#### 方法 B: `.devcontainer/` フォルダだけコピーする

既存のプロジェクトに Dev Container を追加したい場合はこちら。

```bash
# 既存プロジェクトのディレクトリに移動
cd ~/code/your-project

# .devcontainer フォルダをコピー
cp -r ~/code/20260329_devcontainer-template/.devcontainer .
```

### コンテナを起動する

1. VS Code でプロジェクトフォルダを開く

```bash
code ~/code/新しいプロジェクト名
```

2. VS Code の左下に緑色の `><` アイコンがあるのでクリック（または `Cmd+Shift+P` でコマンドパレットを開く）

3. 「**Dev Containers: Reopen in Container**」を選択

4. 初回はコンテナの構築が始まります（数分かかります）

5. 左下の表示が「Dev Container: Node.js + Python Dev Environment」に変わったら準備完了です

### コンテナ内で開発する

コンテナが起動したら、VS Code のターミナル（`` Ctrl+` ``）がコンテナ内のシェルになっています。

```bash
# Node.js が使える
node --version    # → v22.x.x

# Python が使える
python3 --version # → Python 3.12.x

# Claude Code が使える
claude            # → Claude Code が起動

# Codex CLI が使える
codex             # → Codex CLI が起動

# GitHub CLI が使える
gh --version      # → gh version x.x.x
```

### コンテナを終了する

- VS Code の左下の緑アイコン → 「**Reopen Folder Locally**」でMacのローカルに戻る
- または VS Code を閉じるだけでもOK（コンテナは自動で停止します）

### コンテナを再起動する

再度 VS Code でプロジェクトを開けば、「Reopen in Container」で前回と同じ環境が立ち上がります。2 回目以降はキャッシュがあるため数秒で起動します。

## 含まれるもの

| ツール | バージョン | 用途 |
|--------|-----------|------|
| Node.js | 22 | TypeScript / JavaScript の実行環境 |
| Python | 3.12 | Python の実行環境 |
| pnpm | latest | Node.js のパッケージ管理（npm より高速） |
| GitHub CLI | latest | ターミナルから PR や Issue を操作 |
| Claude Code | latest | AI ペアプログラミングツール |
| Codex CLI | latest | AI コーディングエージェント（OpenAI） |

## 個人設定の引き継ぎ

Mac 側の `~/.claude/` と `~/.codex/` がコンテナ内に自動マウントされます。これにより、Mac で設定した以下の内容がコンテナ内でもそのまま使えます。

### Claude Code
- **スキル** — `/explain` などのカスタムコマンド
- **ルール** — コーディング規約、セキュリティルール
- **エージェント** — code-reviewer、tdd-guide など
- **フック** — 自動フォーマット、安全チェック
- **認証情報** — API キーの再設定は不要

### Codex CLI
- **認証トークン** — Mac 側で ChatGPT サインイン済みなら再認証不要
- 未認証の場合は、先に Mac 側で `codex` を起動して ChatGPT サインインを済ませてください

## チームでの利用

1. このテンプレートからプロジェクトを作成
2. `.devcontainer/` フォルダをリポジトリにコミット
3. チームメンバーはリポジトリを clone して VS Code で開くだけ
4. 「Reopen in Container」で全員が同じ環境を取得

```bash
# チームメンバーの操作（これだけで開発環境が整う）
git clone https://github.com/your-org/your-project.git
code your-project
# → VS Code で「Reopen in Container」
```

## カスタマイズ

プロジェクトに合わせて `.devcontainer/devcontainer.json` を編集できます。

### 言語やツールを追加する

`features` セクションに追加します。利用可能な Feature は [公式カタログ](https://containers.dev/features) で探せます。

```jsonc
"features": {
  // 例: Go を追加
  "ghcr.io/devcontainers/features/go:1": {
    "version": "1.22"
  },
  // 例: AWS CLI を追加
  "ghcr.io/devcontainers/features/aws-cli:1": {}
}
```

### セットアップコマンドを変更する

`postCreateCommand` はコンテナ作成時に一度だけ実行されるコマンドです。

```jsonc
// 例: プロジェクト固有の依存関係もインストール
"postCreateCommand": "curl -fsSL https://claude.ai/install.sh | bash && npm install -g pnpm && pnpm install && echo 'Ready!'"
```

### ポートを追加する

開発サーバーのポートを `forwardPorts` に追加すると、コンテナ内のサーバーに Mac のブラウザからアクセスできます。

```jsonc
"forwardPorts": [3000, 5173, 8000, 8080]
```

### VS Code 拡張機能を追加する

チーム全員に入れてほしい拡張機能を指定できます。

```jsonc
"customizations": {
  "vscode": {
    "extensions": [
      "anthropic.claude-code",
      "bradlc.vscode-tailwindcss"  // 例: Tailwind CSS
    ]
  }
}
```

## トラブルシューティング

### 「Reopen in Container」が表示されない

- Docker Desktop が起動しているか確認（メニューバーにクジラアイコン）
- VS Code の Dev Containers 拡張がインストールされているか確認

### コンテナの構築に失敗する

```bash
# コンテナをリビルドする（キャッシュなし）
# VS Code コマンドパレット（Cmd+Shift+P）→
# 「Dev Containers: Rebuild Container Without Cache」
```

### Claude Code のフックでエラーが出る

フック内のスクリプトが bash 専用の構文を使っている場合、コンテナの `/bin/sh`（dash）では動きません。`#!/bin/bash` をスクリプト先頭に付けるか、POSIX 互換な書き方に修正してください。

## ライセンス

MIT
