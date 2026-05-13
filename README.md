# GitHub 講座② 実践リポジトリ

> issue 駆動開発の基本フローを、実際に手を動かして体験しよう。

---

## このリポジトリでやること

- **実践①**：issue → branch → 編集 → PR → merge を一周する
- **実践②**：ペアでわざとコンフリクトを起こして解消する

---

## 実践① — clone から merge までを体験

### Step 1：リポジトリを clone する

**操作場所：ターミナル**

まず作業用ディレクトリに移動してから clone します。

```bash
cd ~/workspace
git clone git@github.com:kogureshinpei/github-lecture-issue-driven-2.git
cd github-lecture-issue-driven-2
```

> **注意**：SSH 形式（`git@github.com:...`）で clone します。SSH の設定が済んでいない場合はトラブルシューティングを参照してください。

---

### Step 2：issue を1つ選んで Assignee を自分にする

**操作場所：GitHub ブラウザ**

1. このリポジトリの **「Issues」タブ** を開く
2. 一覧から担当したい issue を1つ選んでクリック
3. 右側の **「Assignees」** をクリックし、自分のアカウントを設定する
4. issue 番号（例：`#1`）を確認しておく（次のステップで使います）

---

### Step 3：ブランチを切る

**操作場所：ターミナル**

issue 番号に合わせたブランチを作成します。

```bash
git switch -c feature/issue-番号-説明
```

**例（issue #1 でヘッダーを追加する場合）：**

```bash
git switch -c feature/issue-1-header
```

> **ブランチ名のルール**：`feature/issue-番号-一言説明` の形式で付けます。
> 例：`feature/issue-2-member-cards`、`feature/issue-3-works-section` など

---

### Step 4：ファイルを編集する

**操作場所：VSCode**

1. VSCode で `index.html` を開く
2. 担当 issue の内容に合わせて編集する
   - 例：ナビゲーションを追加する issue なら `<!-- TODO: ナビゲーションを追加する -->` の部分を実装する
3. **保存を忘れずに（`Cmd+S` / `Ctrl+S`）**

---

### Step 5：add / commit / push する

**操作場所：ターミナル**

3つのコマンドを順番に実行します。

```bash
# 変更ファイルをステージに追加（コミット対象として選択する操作）
git add .

# 変更内容をコミット（「何をしたか」を簡潔に書く）
git commit -m "ヘッダーにナビゲーションを追加"

# GitHub にプッシュ（ブランチ名は自分が切ったものに合わせる）
git push origin feature/issue-1-header
```

> **コミットメッセージの例**：「ヘッダーにナビゲーションを追加」「メンバーカードを実装」など、「何をしたか」が伝わるメッセージにしましょう。

---

### Step 6：Pull Request を作成する

**操作場所：GitHub ブラウザ**

1. `git push` 後に GitHub のリポジトリページを開くと、黄色いバナーで **「Compare & pull request」** ボタンが表示されます
2. クリックして PR 作成画面へ
3. **タイトル**：何をしたかを書く（例：「ヘッダーにナビゲーションを追加 #1」）
4. **本文**：必ず以下を書くこと

```
Closes #1
```

> **ポイント**：`Closes #番号` と書くと、PR が merge されたときに対応する issue が**自動でクローズ**されます。issue 番号は Step 2 で確認したものを使います。

5. 右側の **「Reviewers」** から隣の人を指定する
6. **「Create pull request」** をクリック

---

### Step 7：レビューを受けて merge する

**操作場所：GitHub ブラウザ**

1. レビュアーに指定された人は PR を開き、変更内容を確認する
2. 問題なければ **「Review changes」→「Approve」→「Submit review」** をクリック
3. Approve されたら作成者が **「Merge pull request」→「Confirm merge」** をクリック

merge 後は、ローカルの main を最新の状態に更新しておきましょう。

```bash
git switch main
git pull origin main
```

---

## 実践② — コンフリクトを起こして解消する

### 概要

ペアを組んで、同じファイルの同じ箇所をそれぞれ別の内容で編集します。
これにより**コンフリクト（競合）**が意図的に発生します。
実際の開発現場でよく起こる状況なので、解消手順を体験しておきましょう。

---

### ペアの役割分担

| 役割 | やること |
|---|---|
| **Person A** | `branch-a` を切って `style.css` の背景色を `red` に変更し、**先に** merge する |
| **Person B** | `branch-b` を切って `style.css` の同じ行の背景色を `blue` に変更し、**後から** PR を作るとコンフリクトが発生する |

---

### Person A の手順

**操作場所：ターミナル → GitHub ブラウザ**

```bash
# 1. ブランチを切る
git switch -c conflict/branch-a

# 2. style.css を開いて編集（VSCode で行う）
#    「CONFLICT PRACTICE」コメント直下の background-color を red に変更
#    変更前：background-color: gray;
#    変更後：background-color: red;

# 3. add / commit / push
git add .
git commit -m "背景色を red に変更"
git push origin conflict/branch-a
```

4. GitHub で PR を作成して merge する
5. Person B に **「merge したよ」** と伝える

---

### Person B の手順

**操作場所：ターミナル → GitHub ブラウザ**

```bash
# 1. 最新の main から始める
git switch main
git pull origin main

# 2. ブランチを切る
git switch -c conflict/branch-b

# 3. style.css を開いて編集（VSCode で行う）
#    「CONFLICT PRACTICE」コメント直下の background-color を blue に変更
#    変更前：background-color: gray;
#    変更後：background-color: blue;

# 4. add / commit / push
git add .
git commit -m "背景色を blue に変更"
git push origin conflict/branch-b
```

5. GitHub で PR を作成する
6. **「This branch has conflicts that must be resolved」** と表示される → これが正常です！

---

### コンフリクトの解消手順

**操作場所：ターミナル → VSCode → ターミナル → GitHub ブラウザ**

#### 1. main を最新にしてから、自分のブランチに main をマージする

```bash
git switch conflict/branch-b
git merge main
```

コンフリクトが発生した旨のメッセージが表示されます：

```
Auto-merging style.css
CONFLICT (content): Merge conflict in style.css
Automatic merge failed; fix conflicts and then commit the result.
```

#### 2. VSCode でコンフリクトファイルを開く

`style.css` を開くと、コンフリクト箇所が赤くハイライトされています。

#### 3. コンフリクトマーカーを見つける

```
<<<<<<< HEAD
  background-color: blue;
=======
  background-color: red;
>>>>>>> main
```

- `<<<<<<< HEAD`：自分（Person B）の変更
- `=======`：区切り線
- `>>>>>>> main`：相手（Person A の変更が merge 済みの main）の変更

#### 4. どちらを残すか相談して決める

チームで話し合って、残したい内容に編集します。

**例：blue を残す場合**

```css
  background-color: blue;
```

> **重要**：`<<<<<<<`・`=======`・`>>>>>>>` のマーカーを**全部消す**こと！マーカーが残っているとコードが壊れます。

#### 5. ファイルを保存して push する

```bash
git add .
git commit -m "コンフリクトを解消"
git push origin conflict/branch-b
```

#### 6. GitHub の PR に戻って確認

PR ページを開くと **「This branch has no conflicts with the base branch」** に変わっています。
あとは **「Merge pull request」** をクリックして完了です！

---

## よくある質問 / トラブルシューティング

**Q. `git push` でエラーが出た**

SSH の設定ができているか確認しましょう。以下を実行して `Hi ユーザー名!` と表示されれば OK です。

```bash
ssh -T git@github.com
```

---

**Q. ブランチを間違えて作ってしまった**

main に戻って、正しいブランチ名で作り直しましょう。

```bash
git switch main
git switch -c 正しいブランチ名
```

---

**Q. コミットメッセージを間違えた（まだ push していない場合）**

以下のコマンドで上書きできます。

```bash
git commit --amend -m "正しいメッセージ"
```

---

**Q. `git push` 後に PR ボタンが出ない**

GitHub のリポジトリページを再読み込みしてください。
それでも出なければ **「Pull requests」タブ → 「New pull request」** をクリックして手動で作成できます。

---

**Q. コンフリクト解消後も「conflicts」と表示される**

マーカー（`<<<<<<<`、`=======`、`>>>>>>>`）が残っていないか確認してください。
1文字でも残っていると解消されていないと判断されます。

---

**Q. 詰まったら**

Discord の質問チャンネルに以下を投稿してください。
- エラーメッセージのスクリーンショット
- 「何をしようとしたか」の説明

一人で悩まず、気軽に質問してください！
