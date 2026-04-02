# learning-journal

## 2026/03/16 学習記録 — 開発環境構築

### やったこと

- Homebrew のインストール確認（すでに導入済みだった）
- Git の設定（user.name / user.email の登録）
- Node.js を fnm 経由でインストール（v22系）
- VS Code インストール + 拡張機能5つを導入
  - ESLint
  - Prettier
  - Tailwind CSS IntelliSense
  - ES7 React Snippets
  - GitHub Copilot
- GitHub アカウント作成 + SSH鍵（ed25519）の設定
- リポジトリ `learning-journal` を作成してローカルにclone、VS Codeで開く

---

### 詰まったところ・エラーと解決

**エラー1：SSH鍵生成時のミス**
```
Saving key "cat /Users/****/.ssh/id_ed25519" failed: No such file or directory
```
- 原因：`ssh-keygen` の保存先入力欄に、次のコマンドである `cat ~/.ssh/id_ed25519.pub` まで貼り付けてしまった
- 解決：誤ったパスのファイルを削除して `ssh-keygen` をやり直し、質問にはすべてEnterだけで通過

**エラー2：GitHub SSH初回接続時の入力ミス**
```
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
- 原因：`yes` の前に先にEnterを押してしまい、`y` が連続入力され続けた
- 解決：`Control + C` で中断して再度 `ssh -T git@github.com` を実行、`yes` と入力してEnterで認証成功

**VS Code 信頼確認ダイアログ**
- `Do you trust the authors of the files in this folder?` が表示された
- 自分で作ったフォルダなので「Yes, I trust the authors」を選択

---

### 確認コマンドまとめ

```bash
brew --version      # Homebrew バージョン確認
node --version      # v22.x.x
npm --version       # 10.x.x
git config --global --list  # name/email の確認
ssh -T git@github.com       # GitHub SSH接続確認
```



## 2026/03/18 学習記録 — Next.js 環境構築

### やったこと

- Next.jsプロジェクト（my-app）を作成
- npm run dev で開発サーバーを起動
- localhost:3000 でブラウザ表示を確認
- page.tsx を編集してホットリロードを体験
- READMEへの情報編集と再push

### 気づいたこと

- ファイル保存と同時にブラウザが自動更新される
- PHPのときと違って手動リロード不要

---

### 詰まったところ・エラーと解決

**エラー1：npx コマンドが見つからない**
```
zsh: command not found: npx
```
- 原因：新しいターミナルを開いたことで fnm のパスが引き継がれていなかった
- 解決：以下を実行してパスを反映、さらに ~/.zshrc に追記して恒久対応
```
basheval "$(fnm env --use-on-cd)"
echo 'eval "$(fnm env --use-on-cd)"' >> ~/.zshrc
source ~/.zshrc
```

**エラー2：fnm 自体も認識されていなかった**
```
zsh: command not found: fnm
zsh: command not found: node
zsh: command not found: npx
```
- 原因：前回のセッションでインストールしたが、新しいターミナルに反映されていなかった
- 解決：brew で fnm を再インストールし、Node.js も再度セットアップ
```
bashbrew install fnm
echo 'eval "$(fnm env --use-on-cd)"' >> ~/.zshrc
source ~/.zshrc
fnm install 22
fnm use 22
fnm default 22
```
- 詰まった点：VS Code内ターミナルの開き方（JIS配列Mac）

- Control + ``  `` `（バッククォート）はJIS配列では押しにくい
- 解決：メニューの「ターミナル」→「新しいターミナル」から開く

---

### 確認コマンドまとめ
```
bashnode --version     # v22.x.x
npx --version      # バージョン確認
npm run dev        # 開発サーバー起動
# 停止は Control + C
```

---
## 2026/04/02 学習記録 — Next.js ページ作成・ルーティング

### やったこと

- Next.jsのフォルダ構造を把握（app/, public/, package.json）
- app/about/page.tsx を作成して /about ページを追加
- aboutページに自己紹介コンテンツを記述
- トップページに Link コンポーネントでナビゲーションを追加

---

### 学んだ概念
# ファイルベースルーティング
- app/ 配下にフォルダを作り page.tsx を置くだけでURLになる
- 例：app/about/page.tsx → localhost:3000/about

```
bash
# aboutフォルダを作成
mkdir app/about
# VS Codeのファイルツリーで app/about を右クリック → 新しいファイル → page.tsx
```

# Linkコンポーネント
- Next.js専用のリンク。通常の <a> タグより高速なページ遷移ができる
```
tsx
import Link from "next/link"

<Link href="/about">Aboutページへ</Link>
```

# Tailwind CSSクラスの基本
- text-4xl font-bold → 大きな太文字
- mb-8 → 下マージン
- text-gray-600 → グレーのテキスト
- hover:bg-gray-800 → ホバー時に背景色変化

# 開発サーバーの起動・停止
```
bash# 起動
cd ~/dev/my-app
npm run dev
```

# 停止
```
Control + C
学習記録のpush
bashcd ~/dev/learning-journal
git add .
git commit -m "コミットメッセージ"
git push
```

### 作成・編集したファイル
```
my-app/
├── app/
│   ├── page.tsx         ← Linkを追加
│   └── about/
│       └── page.tsx     ← 新規作成
```
---

### 気づいたこと

- フォルダ構造がそのままURLになる感覚が直感的でわかりやすい
- 保存するたびにブラウザが即更新されるので、試行錯誤がしやすい

---
