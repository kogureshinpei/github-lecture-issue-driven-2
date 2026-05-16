# コントリビューションガイド

## 作業の流れ

1. このリポジトリを **Fork** する
2. フォークしたリポジトリを自分の環境にクローンする
   ```bash
   git clone https://github.com/自分のユーザー名/リポジトリ名
   ```
3. 作業用ブランチを作成する
   ```bash
   git checkout -b feature/作業内容を表す名前
   ```
4. 変更をコミットする
   ```bash
   git add .
   git commit -m "変更内容の説明"
   ```
5. フォーク先にプッシュする
   ```bash
   git push origin feature/作業内容を表す名前
   ```
6. このリポジトリへ **Pull Request** を作成する

## ブランチ命名規則

| 用途 | 例 |
|------|----|
| 新機能・課題対応 | `feature/add-login-page` |
| バグ修正 | `fix/typo-in-readme` |

## コミットメッセージ

- 日本語・英語どちらでも可
- 何をしたか一言で分かるように書く

## 困ったときは

Issue にコメントするか、講師に声をかけてください。
