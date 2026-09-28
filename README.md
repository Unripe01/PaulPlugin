# ポールくんプラグイン（PoC）

日本株を探す → 公開情報を軽く調べる → 一般向けのnote記事を書く → ログイン済みブラウザでnoteに**下書き保存**する、1つのSkillです。「公開して」と明示された場合だけ公開します。売買や注文は行いません。

## 中身

```text
.agents/plugins/marketplace.json        # GitHubからCodexへ導入するための一覧
plugins/paul-kun/
  plugin.json                           # Agent Plugins 1.0
  .codex-plugin/plugin.json             # Codex用の互換マニフェスト
  skills/stock-to-note/SKILL.md         # 実行手順
  skills/stock-to-note/references/       # 記事の目安と最低限の点検
```

専用API、MCPサーバー、追加ライブラリは使いません。株価などは取得元と基準日を確認し、見つからない数字は記事に無理に入れません。

## 導入手順

Codex CLIが使える別の環境で、このリポジトリをGitHubに置いた後、以下を実行します。`OWNER/REPO`は実際のGitHub上の場所に置き換えてください。

```powershell
codex plugin marketplace add OWNER/REPO
codex plugin add paul-kun@paul-kun-demo
codex plugin list
```

ローカルのcloneから試す場合は、cloneしたリポジトリのルートを指定します。

```powershell
git clone https://github.com/OWNER/REPO.git
codex plugin marketplace add ./REPO
codex plugin add paul-kun@paul-kun-demo
```

Codexデスクトップではプラグイン一覧から有効化し、**新しいチャット**で使ってください。プラグインの追加・更新時は、必要に応じてCodexを再起動してください。

## 利用前準備

- CodexからWeb検索とブラウザ操作が利用できること。
- 操作するブラウザで、先に[note](https://note.com/)へログインしておくこと。同じPCでも別のブラウザやプロファイルのログインは引き継がれない場合があります。
- 記事は通常、下書き保存です。公開したい場合は依頼文に「公開して」と明記してください。

## 使い方

> 今日noteに書けそうな日本株を探して、記事にしてnoteに下書きして

銘柄を指定するなら「カバー（5253）を調べてnoteに下書きして」のように頼めます。結果には銘柄・証券コード、下書き／公開の状態、確認できたURLまたは画面上の保存状態が返ります。

## 実際のテスト方法

1. CodexでWeb検索とブラウザ操作を使える状態にし、**そのブラウザで**noteにログインする。
2. 新しいチャットで上記の「今日noteに…」をそのまま依頼する。
3. 銘柄選定、出典つき調査、記事作成が行われたことを確認する。
4. ブラウザのnote新規記事画面でタイトルと本文が入力され、「下書き保存」の完了表示か、下書き一覧で記事が残っていることを確認する。
5. 公開状態になっていないことを確認する。公開のテストは別途「公開して」と明示した場合だけ行う。

ログイン画面やCAPTCHAが出たらそこで中断し、ユーザーが操作した後で続けます。保存に失敗した場合は、下書き一覧を先に見て重複作成を避けます。

## GitHubへ上げる手順

リポジトリのルートで実行します。`OWNER/REPO`を実際に作成した空のGitHubリポジトリへ置き換えてください。

```powershell
git init
git add .
git commit -m "Add Paul Kun stock-to-note PoC plugin"
git branch -M main
git remote add origin https://github.com/OWNER/REPO.git
git push -u origin main
```

すでにGit管理されている場合、`git init`は不要です。既存の`origin`がある場合は、重複して追加せずURLを確認してください。GitHubへの公開はこのREADMEの手順で行い、このPoCの動作には不要です。

## 注意事項

- 記事は公開情報に基づく情報提供であり、投資助言ではありません。投資判断はご自身で行ってください。
- noteの画面変更、ログイン状態、CAPTCHA、ブラウザ権限などにより操作が失敗することがあります。
- 株価・出来高・PERは時点で変わります。記事に載せる際は基準日、出典、予想／実績の別を確認してください。
