# Portfolio

X のポストを集約した作品まとめページ。GitHub Pages で公開する静的1枚もの。

## 作品を追加する

[index.html](index.html) の `WORKS` 配列にオブジェクトを1つ足すだけ。

```js
{
  year: "2026",
  title: "作品タイトル",
  role: "担当したこと",
  tags: ["音響"],              // フィルタ用。省略可
  note: "補足メモ",            // 省略可
  tweetId: "1234567890123456789",
  links: [{ label: "公式サイト", url: "https://..." }],
},
```

`tweetId` は X のポストURL末尾の数字。

```
https://x.com/foo/status/1234567890123456789
                         ^^^^^^^^^^^^^^^^^^^
```

編集したら commit → push。1分ほどで反映される。

## デプロイ

初回のみ:

1. GitHub で `lactoice251.github.io` という名前のリポジトリを **Public** で作成
2. このディレクトリで:

```sh
git remote add origin https://github.com/lactoice251/lactoice251.github.io.git
git push -u origin main
```

3. リポジトリの Settings → Pages で Source が `Deploy from a branch / main / (root)` になっているか確認
4. `https://lactoice251.github.io/` で公開される

リポジトリ名がユーザー名と完全一致したときだけ、ドメイン直下（ルート）で公開される。
名前が違うと `https://lactoice251.github.io/<リポジトリ名>/` になる。
ユーザー名を変えたときは、リポジトリ名も追従させないとルート公開が外れる。

2回目以降は `git add . && git commit -m "..." && git push` だけ。

## 設計メモ

X の埋め込みは X 側のスクリプト（`platform.twitter.com/widgets.js`）に依存していて、
仕様変更やスクリプト障害で表示が壊れることがある。そのため:

- 作品名・クレジット・年は**埋め込みと独立したテキスト**として持つ
- 各項目に「X のポストを開く」素のリンクを必ず出す
- 埋め込みの読み込みに失敗したらフォールバック表示に差し替える
- 埋め込みは画面に入ってから読む（IntersectionObserver）ので、件数が増えても初回表示は重くならない

結果、X 側が完全に死んでもページとしては成立する。
