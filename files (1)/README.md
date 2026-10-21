# Lucid Pair

ローカル優先のデスクトップ AI 秘書(Windows / x64 / ポータブル zip 配布)。運営者: yama9108(個人)。

- ホームページ: [index.html](./index.html)
- 個人情報対策 Privacy policy: [privacy-policy.html](./privacy-policy.html)

## 概要

取得したデータは原則として自分の端末内で管理します。外部に送るのは
「利用者が自分で OAuth で接続したサービス」「利用者が AI を有効にした場合の AI 処理先」
「利用者が有効にした場合の SMTP 送信先」だけで、開発者のサーバーには送信しません
(OAuth 連携も利用者の端末上で動作します)。

主な機能:

- **作業の「後回し」** — 押した瞬間の画面・作業場所・予定を記録してタスク化し、期限の 5 分前にリマインド
- **画面分析オーバーレイ** — 作業中のウィンドウの文字や操作画面を分析し、その位置にマーキングと吹き出しを重ねる
- **予定のリマインド** — 予定は 10 分前、予定のリマインドは設定時刻どおり
- **外部サービスの監視** — Google カレンダー / Microsoft 365(Outlook・OneDrive・Teams)/ Discord など
- **作業員モード(サンドボックス)** — 実行は許可リストと信頼ディレクトリ内に限定、外部操作は承認ゲート経由

## Google API Services User Data Policy への準拠

> Lucid Pair's use and transfer to any other app of information received from Google APIs will adhere to the Google API Services User Data Policy, including the Limited Use requirements.

参照: [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy) /
[OAuth 2.0 Policy](https://developers.google.com/identity/protocols/oauth2/policies)

## 申請するスコープ(すべて read-only)

| スコープ | 区分 | 取得するデータ | 用途 | 状態 |
| --- | --- | --- | --- | --- |
| `openid` / `email` / `profile` | 非機密 | 表示名・メールアドレス | 接続アカウントの表示 | 要求済み(現在付与済み) |
| `calendar.readonly` | 機密 | 予定の件名・日時・説明 | 予定のリマインド生成 | 申請対象 |
| `gmail.readonly` | 制限付き | 件名・本文(最大 2,000 文字)・送信元・日時 | 優先度判定・要約・約束の抽出 | 検証完了後に申請 |

- 書き込み系スコープは要求しません。
- **段階的な申請**: 制限付き(restricted)の `gmail.readonly` は Google による追加の検証と
  セキュリティ評価(CASA)を伴うため、検証が完了するまで同意画面には含めません。
  まず機密スコープ(カレンダー)だけで公開し、Gmail 対応は評価の完了後に追加する予定です。
- 実装していない機能(ドライブ・ドキュメント・スライド・YouTube など)のスコープは宣言していません。
  機能を追加する時点で、この表と同意画面の宣言を同時に更新します。
- Google API のレスポンスをディスクにキャッシュせず、保存するのは、マスク処理を通した件名・本文・予定情報と、そこから作成した要約だけです。

## 外部 AI 処理について

Google のメール・予定は**外部 AI へ送りません**(端末内のルールベース処理のみで処理します)。
外部 AI へ送信するのは、利用者の画面から取得した画像と Google 以外のサービス由来テキストだけです。

理由は、AI 処理先の無料枠では送信された入力と出力が当該提供元のサービス改善や人によるレビューに
利用され得るためで、
[Gemini API Additional Terms of Service](https://ai.google.dev/gemini-api/terms) に照らして
Google ユーザーデータを扱わない構成にしています。

AI 処理先でのデータの扱いは、利用者が使用する API キーのプラン(無料枠/有料枠)に応じた提供元の規約に従います。
開発者が AI 学習等の目的でデータを送ることはありません。

## Google Cloud Console での入力値

OAuth 同意画面(Google Auth Platform > Branding)に次の URL を入力します。

| 欄 | 入力値 |
| --- | --- |
| Application home page | `https://yama9108.github.io/Lucidpair/` |
| Application privacy policy link | `https://yama9108.github.io/Lucidpair/privacy-policy.html` |

公開後、Google Auth Platform > Audience の **「Publish app」** を押すと公開ステータスが
`In production` になります。

### 公開前の注意

- **未検証のまま `In production` にした場合**、Google の未検証アプリ警告が同意画面に表示され、
  同意できるユーザー数は**プロジェクト生涯で 100 人**まで(月初の更新ではなく累積)です。
  検証を完了するとこの制限と警告は外れます。
- 検証の申請は **Google Auth Platform の「Verification Center」** から行います
  (コンソールの左メニューから遷移できます)。
- `gmail.readonly` は制限付きスコープのため、検証に加えてセキュリティ評価(CASA)が求められる可能性があります。
- **独自ドメインの検討**: 本ページの公開先は GitHub Pages(`github.io`)ですが、これは共有ドメインのため
  ドメイン所有を検証できません。OAuth 2.0 Policy は「ホームページが、自己が所有する
  検証済みのドメイン上にあること」を求めています。検証を申請する前に
  独自ドメインを取得し、そのドメインへ本ページと個人情報対策ページを移す予定です。
  (移行後も `github.io` のページは残し、独自ドメインへリダイレクトする形です)

## HTML ファイルの役割

| ファイル | 内容 |
| --- | --- |
| `index.html` | ホームページ(Branding の home page 欄) |
| `privacy-policy.html` | 個人情報対策 Privacy policy(Branding の privacy policy 欄) |

## お問い合わせ

(個人連絡先は公開していません)

<https://github.com/yama9108/Lucidpair/issues>

Issues は公開されるため、個人情報(氏名・メールアドレス・所在地など)は記載しないでください。
当アプリの開発者サーバーや API へ送信・保存されることはなく、GitHub が直接受け取るだけです。

最終更新日: 2026-10-21
