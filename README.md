# Multi-Approach Development Workflow

## 概要
このワークフローシステムは、1つの要求に対して10種類の異なるアプローチを設計・実装し、最適なものを選択してPRを作成する完全自動化されたシステムです。

## ファイル構成

```
.claude/
├── commands/
│   └── multi-approach.md       # メインのオーケストレーターコマンド
└── agents/
    ├── design-agent.md         # 設計フェーズ: 10個のアプローチを考案
    ├── implementation-agent.md # 実装フェーズ: 各アプローチを実装
    ├── decision-agent.md       # 決定フェーズ: 最適なアプローチを選択
    ├── review-agent.md         # レビューフェーズ: PRをレビュー
    └── final-implementation-agent.md # 最終実装: レビュー内容を反映
```

## ワークフローの流れ

### 前提条件
- worktreeを1つ作成し、作業ブランチに移動している
- 例: `git worktree add ../worktrees/work-feature -b work/feature && cd ../worktrees/work-feature`

### 実行手順

1. **ワークフロー開始**
   ```
   /multi-approach
   ```
   その後、実装したい要求を入力

2. **フェーズ1: 設計 (Design Phase)**
   - design-agentが起動
   - 10個の異なるアプローチを考案
   - トレードオフを考慮して分析
   - 結果を `plan.md` に出力

3. **フェーズ2: 実装 (Implementation Phase)**
   - `../worktrees/` ディレクトリを作成
   - 10個のworktreeを作成（例: work-feature-1 〜 work-feature-10）
   - 各worktreeに .envrc をコピー
   - 10個のimplementation-agentが並列で各アプローチを実装
   - 各worktreeにコミット

4. **フェーズ3: 決定 (Decision Phase)**
   - decision-agentが起動
   - 10個の実装を全てレビュー
   - 最適なアプローチを選択
   - 結果を `decision.md` に出力

5. **フェーズ4: PR作成**
   - 選択されたworktreeからPRを作成
   - PRのURLを記録

6. **フェーズ5: レビュー (Review Phase)**
   - 10個のreview-agentが異なる観点でPRをレビュー
     - Security, Performance, Maintainability, Testing, Architecture, etc.
   - 全てのレビューを `review.md` に集約

7. **フェーズ6: 最終実装 (Final Implementation)**
   - final-implementation-agentが起動
   - review.mdの内容を実装
   - 変更をコミット
   - ワークフロー完了

## ディレクトリ構成例

```
project/
├── worktrees/                    # 全てのworktreeがここに作成される
│   ├── work-feature/             # 元の作業worktree
│   ├── work-feature-1/           # アプローチ1の実装
│   ├── work-feature-2/           # アプローチ2の実装
│   ├── ...
│   └── work-feature-10/          # アプローチ10の実装
└── your-repo/                    # 元のリポジトリ
    └── .claude/
        ├── commands/
        └── agents/
```

## 成果物

- `plan.md` - 10個のアプローチの設計書
- `decision.md` - 最適アプローチの選定理由
- `review.md` - 10個の観点からのPRレビュー
- 各worktree内:
  - `IMPLEMENTATION.md` - 実装の詳細
  - `REVIEW_IMPLEMENTATION.md` - レビュー対応の詳細

## 使用例

```bash
# 1. 作業ブランチのworktreeを作成
git worktree add ../worktrees/work-add-user-auth -b work/add-user-auth
cd ../worktrees/work-add-user-auth

# 2. Claude Codeでワークフロー実行
/multi-approach

# 3. 要求を入力
"ユーザー認証機能を追加してください。JWT、OAuth、Session-basedなど複数の方式を検討してください。"

# 4. 自動的に全フェーズが実行され、最終的なPRが作成される
# worktreesディレクトリに以下が作成される:
# - work-add-user-auth-1 (アプローチ1)
# - work-add-user-auth-2 (アプローチ2)
# - ...
# - work-add-user-auth-10 (アプローチ10)
```

## 注意事項

- 各agentは完全に自律的に動作します
- worktreeは `../worktrees/` ディレクトリに自動作成されます
- worktreeの削除は手動で行ってください
- .envrc が存在する場合は自動的にコピーされます
- 全てのworktreeはローカルに保持され、remoteにはpushされません（最終PRのみ）

## クリーンアップ

ワークフロー完了後、不要なworktreeを削除:

```bash
# decision.mdで選ばれなかったworktreeを削除
git worktree remove ../worktrees/work-feature-1
git worktree remove ../worktrees/work-feature-2
# ... (選ばれたもの以外を削除)

# ブランチも削除
git branch -D work/feature-1
git branch -D work/feature-2
# ...

# 全て削除する場合
rm -rf ../worktrees/work-feature-*
git worktree prune
```
