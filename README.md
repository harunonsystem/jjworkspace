# JJ Workspace

これは [jj(Jujutsu)](https://martinvonz.github.io/jj/) のワークスペースです。

## 概要

Jujutsuは、Gitと互換性のある次世代バージョン管理システムです。

## JJ と Git の根本的な違い

### 🔄 リビジョン（Change）という概念

| 概念 | Git | JJ |
|------|-----|-----|
| **基本単位** | コミット（不変・SHA固定） | Change（可変・ID固定） |
| **変更時** | 新しいSHAが生成される | Change IDは変わらない |

- Gitはコミットを「不変のスナップショット」として扱う
- jjは「Change（変更セット）」という単位で、**中身を変えてもIDは維持される**
- これにより履歴の編集が自然なワークフローになる

### 📝 ステージングエリアが存在しない

```
Git:   作業ディレクトリ → git add → ステージング → git commit
JJ:    作業コピー = コミット（常に自動追跡）
```

- jjでは**作業コピー自体がコミット**として扱われる
- `git add` は不要、常にすべての変更が追跡されている
- `git stash` も不要（`jj new` で新しいコミットを作るだけ）

### ✨ 履歴の書き換えが前提の設計

- `jj split` / `jj squash` / `jj edit` で履歴を自由に編集
- 親コミットを変更すると、子孫が自動でリベースされる
- force-pushは通常運用（Gitでは危険視されがち）

### ⚠️ コンフリクトを保留できる

- Gitはマージ/リベース中にコンフリクトがあると**作業が止まる**
- jjはコンフリクトを**コミット内に記録して進める**ことができる
- 後で都合の良いタイミングで解消すればOK

### ↩️ 強力なUndo

- `jj undo` でほぼすべての操作を取り消せる
- Gitの `reflog` より直感的で安全

## Git ↔ JJ コマンド対応表

| Git | JJ | 説明 |
|-----|-----|------|
| `git add .` | 不要 | jjは自動で変更を追跡（常にスナップショット） |
| `git commit -m "msg"` | `jj commit -m "msg"` | 現在の変更をコミット |
| `git push` | `jj git push` | リモートにプッシュ |
| `git stash` | `jj new` | 新しい空のコミットを作成して作業を退避 |
| `git stash pop` | `jj squash` | 親コミットに変更をマージ |
| `git log` | `jj log` | コミット履歴を表示 |
| `git status` | `jj status` / `jj st` | 作業状態を確認 |

## 基本コマンド

```bash
# 状態確認
jj status

# 現在の変更に説明を追加（git commit -m 相当）
jj describe -m "メッセージ"

# 新しいコミットを作成して次の作業へ
jj commit -m "メッセージ"

# ログ表示
jj log

# リモートにプッシュ
jj git push

# 新しいブックマークを作成
jj bookmark create ブックマーク名
```

## リンク

- [公式ドキュメント](https://martinvonz.github.io/jj/latest/)
- [GitHub](https://github.com/martinvonz/jj)
