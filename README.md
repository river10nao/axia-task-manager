# AXIA タスク管理アプリ

チーム全員がURLで開けるタスク管理アプリ。単一HTML（`index.html`）を **GitHub Pages** で配信し、データは **Googleスプレッドシート**（Google Apps Script Web App経由）に保存します。

## URL・場所

| 項目 | 値 |
|------|----|
| 公開URL | https://river10nao.github.io/axia-task-manager/ |
| リポジトリ | `river10nao/axia-task-manager`（旧名 jinks-task-manager からrename） |
| ローカル本体 | `~/Downloads/index.html` |
| 配信 | GitHub Pages（`main` ブランチ） |

> ⚠️ `~/Downloads` には個人ファイルが多数あります。コミット対象は **`index.html` と `README.md` だけ**。`git add .` は絶対に使わないこと（ファイル名を明示してadd）。

## バックエンド（GAS + スプレッドシート）

- スプレッドシートID: `1UxPhzqkSSC8WXTGZMjqreO1dtrxDwMJw9sBhNOvw4-I`
- 使用タブ: `進捗管理`（gid=`113920740`）
- GAS Web App URL: `.../macros/s/AKfycbxnWXHi_v-MEFvJTNvlc7KPXEil9yaumSTxHg6VBwF7p8T7FCtMltdd38KY-yEFTXeJ/exec`
- シート見出し（A〜I列、**1行目=ヘッダー固定**）:
  `ID, タスク名, ステータス, 優先度, カテゴリ, 担当者, 期限, メモ, 作成日`
- GASアクション: `get / add / update / delete / status`
- アプリ設定の保存先 localStorage キー: `jinks_gas` / `jinks_sid` / `jinks_sn`
  - ⚠️ キー名は小文字 `jinks_` のまま変更しない（変えると全員の保存設定が消える）

## 「通信エラー」が出たら

- 症状: アプリで「通信エラー」。`curl <exec URL>` が `accounts.google.com` にリダイレクトされる
- 原因: GASデプロイのアクセス権が「全員」になっていない
- 対処: GASデプロイ設定を **実行=自分 / アクセスできるユーザー=全員** にして再デプロイ

## スプシ → アプリのリンク

`進捗管理` タブの **J1**（作成日=I列のすぐ右の空きセル）に次を貼る:

```
=HYPERLINK("https://river10nao.github.io/axia-task-manager/","📋 タスク管理アプリを開く")
```

> ⚠️ 1行目の上に行を挿入しない／A〜I列の中に入れないこと（アプリの読み取りが壊れる）。Jより右の空き列なら可。

## 主な機能

- ステータス絞り込み（全て / 未着手 / 進行中 / 完了）
- 担当者で絞り込むドロップダウン（`#assignee-filter`、データから自動生成）
- タスクの追加・編集・削除、KPI表示、検索

## 改修フロー

1. 編集前に `git commit`（クリーンな復帰点を作る）
2. `index.html` を編集 → JS構文確認（`node --check` 相当）
3. Chromeヘッドレスで目視確認
4. `git add index.html`（必要ならREADMEも） → `git commit` → `git push`（数分でPagesに反映）
