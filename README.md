# Next.jsのチュートリアルをやってみる
触ってみたけどめちゃいいぞこれ...

# DEMO
できたのはこれ（マークダウンで記事を管理するブログ）
https://nextjs-tutorial-blog-flax.now.sh/

# セットアップ
```bash
npm init next-app [アプリ名]
cd [アプリ名]
npm run dev
```

# ディレクトリ構造
## 元からあるのはこれだけ
* pages
  * ページそのものを置いておくところ
  * index.jsがトップページ
  * あとは好きにディレクトリを切って配置していけば良い
  * `_app.js`は全コンポーネント共通のあれこれをかけるみたい
    * layout的なやつとは違う
* public
  * 画像などのアセット類を置いておくところ

## チュートリアルで追加したディレクトリ
自分たちでちゃんとルールを作って守りさえすれば好きに作って良さそう

* components
  * 使いまわせるコンポーネントとそれに関連するcssを置いておくところ
  * 全画面共通の`layout.js`と`layout.module.css`を置いた
* lib
  * その他の処理をするファイルを置いておくところ
  * データをフェッチしてくるための処理を作っておいたりした
    * `posts.js`
* posts
  * チュートリアルではブログを作ったのでその記事のMarkdownファイルを配置した
* styles
  * グローバルで使いたいcssやコンポーネントに依存せずあちこちで使えるcssなどを配置した
    * `global.css`, `utils.module.css`
* pages/api
  * APIルートとして使える


# レンダリング方式
* Static Generation
  * 静的に事前にページを生成しておいて、最低限のjsのみクライアント側で動かす
  * パフォーマンスももっとも良く、SEOにもいいのでこれでいけそうな奴は基本これがよい
  * `getStaticProps`を使う
* Server-side Rendering
  * リクエストごとに動的に変わるようなページや、変更頻度が多いページはこれが良い
  * リクエストがきたらサーバー側でレンダリングした結果を返すので静的生成よりも負荷が高いし遅い
  * `getServerSideProps`を使う
* Client-side Rendering
  * データのフェッチが必要じゃない部分だけ先にページを生成して置いて、動的に変えたいところはクライアント側でフェッチしてから書き換える
  * 多分これが一番遅いし、SEOなども弱い
  * 管理画面やダッシュボードなどはこれが有効
    * 値の変更は頻繁だしSEOは気にしなくていいし
  * `SWR`というNext.js独自のReactHooksを使う


# デプロイ
Vercelというものを使うと良いらしい。これが最高だった。

## 手順
* githubでサインインする
* githubリポジトリへのアクセス許可する
* デプロイしたいリポジトリをimportする
  * 特に設定は変えずにOKしていってよい
* これであとは１分くらい待てば完了

## Next.jsがVercelにデプロイされるとやってくれること
* 静的生成とアセット（js、css、画像、フォントなど）
* サーバーサイドレンダリングとAPIルートを使う場合は自動的にサーバーレス関数になり分離される

## 更新手順
これがすごい！
* githubリポジトリにプルリクを送る
  * なんとプルリクを送るとその修正を反映した状態のプレビューURLが閲覧できるようになる
  * 確認して問題なければマージする。そうすると自動で本番もデプロイされる