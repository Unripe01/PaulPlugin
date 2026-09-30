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

## Macでの導入手順

配布元は **[Unripe01/PaulPlugin](https://github.com/Unripe01/PaulPlugin)**、導入するプラグインはリポジトリ内の `plugins/paul-kun/` です。以下は、別の人が自分のMacのターミナル（zsh）で実行する手順です。`$HOME/PaulPlugin` はその人のホームフォルダ内の保存先で、ユーザー名の書き換えは不要です。

1. [Codex](https://developers.openai.com/codex)にサインインし、Macで `git` と `codex` コマンドが使えることを確認します。足りない場合は、それぞれの公式手順で準備します。追加の株価APIやnote用MCPは不要です。
2. このGitHubリポジトリへアクセスできるGitHubアカウントで、以下を実行します。

```bash
command -v git
command -v codex
git clone https://github.com/Unripe01/PaulPlugin.git "$HOME/PaulPlugin"
test -f "$HOME/PaulPlugin/.agents/plugins/marketplace.json"
test -f "$HOME/PaulPlugin/plugins/paul-kun/plugin.json"
test -f "$HOME/PaulPlugin/plugins/paul-kun/skills/stock-to-note/SKILL.md"
codex plugin marketplace add "$HOME/PaulPlugin"
codex plugin marketplace list
codex plugin add paul-kun@paul-kun-demo
codex plugin list
```

`git clone` に失敗した場合は、まずブラウザで上記のGitHubページを、その人自身のアカウントで開けるか確認してください。非公開リポジトリなら、所有者からアクセス権をもらい、招待を承諾してからcloneします。HTTPSのGit認証も必要です。ページが見えない状態でCodexの操作だけ進めても導入できません。

Codexデスクトップアプリを使う場合は、cloneした `~/PaulPlugin` をプロジェクトとして開き、プラグイン一覧で「Paul Kun Demo」から「ポールくんプラグイン」をインストール・有効化します。CLIで追加した場合も、**新しいチャット**で試してください。プラグインが表示されなければアプリを再起動します。

## 利用前準備

- CodexからWeb検索とブラウザ操作が利用できること。
- 操作するブラウザで[note](https://note.com/)へログインすること。未ログインならプラグインがログインを促し、完了の連絡を受けてから保存を再開します。同じPCでも別のブラウザやプロファイルのログインは引き継がれない場合があります。
- 記事は通常、下書き保存です。公開したい場合は依頼文に「公開して」と明記してください。

## 使い方

> 今日noteに書けそうな日本株を探して、記事にしてnoteに下書きして

銘柄を指定するなら「カバー（5253）を調べてnoteに下書きして」のように頼めます。結果には銘柄・証券コード、下書き／公開の状態、確認できたURLまたは画面上の保存状態が返ります。

## PoC資料

実際の業務利用を想定したPoCの概要・ユースケース・評価項目をまとめています。

👉 [PoC資料を見る](./docs/poc.md)

## 実際のテスト方法

1. CodexでWeb検索とブラウザ操作を使える状態にし、**そのブラウザで**noteにログインする。
2. 新しいチャットで上記の「今日noteに…」をそのまま依頼する。
3. 銘柄選定、出典つき調査、記事作成が行われたことを確認する。
4. ブラウザのnote新規記事画面でタイトルと本文が入力され、「下書き保存」の完了表示か、下書き一覧で記事が残っていることを確認する。
5. 公開状態になっていないことを確認する。公開のテストは別途「公開して」と明示した場合だけ行う。

ログイン画面が出たら、プラグインが同じブラウザでのログインを促します。ユーザーがこのチャットで完了を知らせた後、保存を続けます。CAPTCHAが出た場合もユーザーの操作を待ちます。保存に失敗した場合は、下書き一覧を先に見て重複作成を避けます。

## GitHubへ変更を反映する（管理者向け）

GitHubへの書き込み権限があり、上の手順で `~/PaulPlugin` にcloneした場合のコマンドです。`origin` は `https://github.com/Unripe01/PaulPlugin.git` を指します。

```bash
git -C "$HOME/PaulPlugin" status
git -C "$HOME/PaulPlugin" add .
git -C "$HOME/PaulPlugin" commit -m "Update Paul Kun plugin"
git -C "$HOME/PaulPlugin" push origin main
```

## 注意事項

- 記事は公開情報に基づく情報提供であり、投資助言ではありません。投資判断はご自身で行ってください。
- noteの画面変更、ログイン状態、CAPTCHA、ブラウザ権限などにより操作が失敗することがあります。
- 株価・出来高・PERは時点で変わります。記事に載せる際は基準日、出典、予想／実績の別を確認してください。
