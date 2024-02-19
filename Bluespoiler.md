Blueskyに伏せ字投稿ができるWebアプリの作り方
====
2024-02-21 [Yasuyuki ENDO](https://bsky.app/profile/javaopen.org)

<!-- paginate: true -->

---

# GitHubリポジトリ

https://github.com/eyasuyuki/bluespoiler/

---

# 説明すること

- どうやって伏せ字投稿するのか
- なぜ作ろうと思ったのか
- なぜWebサービスやモバイルアプリとしてリリースしなかったのか
- なぜFlutterで書いたのか
- 画像サイズチェック
- Bluesky接続部分とテスト
- BlueskyへのPostと記事URLの推測

---

# 説明しないこと

- fusetterの伏せ字アルゴリズムを勝手に推測する
- Flutterのライブラリの使い方
  - Riverpod
  - Freezed
  - ```go_router```
  - ```flutter_hooks```
  - ```image_picker_web```
- Rich TextでURLをリンクさせる

---

# 詳しくはZennに書きました。

https://zenn.dev/eyasuyuki/articles/825b28b0ec0a4c

![width:600px](https://github.com/eyasuyuki/blueskystudy3/blob/main/images/zenn.png?raw=true)

---

# どうやって伏せ字投稿するのか

- https://eyasuyuki.github.io/bluespoiler/
- [と]の間の文が○になる(fusetterの再発明)
- ネタバレの本文は画像のALTに入る

---

| 伏せ字投稿                                                                                          | ネタバレALT                                                                                           |
------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------
| ![width:300px](https://raw.githubusercontent.com/eyasuyuki/blueskystudy3/main/images/post.jpg) | ![width:300px](https://raw.githubusercontent.com/eyasuyuki/blueskystudy3/main/images/spoiler.jpg) |

---

# なぜ作ろうと思ったのか

- Blueskyにはfusetterがないので
- だがfusetterみたいに自前サーバーを持つのは面倒
- 画像のALTなら1000文字まで入れられる(邪道)
- 自前バックエンドは持たずにBlueskyとだけ通信すればいい

---

# なぜWebサービスやモバイルアプリとしてリリースしなかったのか

- Webサービスは維持費がかかるし無料枠だとしても維持するのが大変
- 当初はモバイルアプリ版も作ろうと思った
- Webアプリとして公開してiPhoneのホーム画面に追加してみた。アプリ必要ないじゃん
- 失敗したのは何もログを取っていないこと(ユーザーにとってはメリット)

---

# なぜFlutterで書いたのか

- 当初はモバイルアプリ版も出そうと思っていた
- ReactやVueなどWebクライアントサイドの流行を追うのは大変
- FlutterならモバイルもWebもデスクトップも同時開発できる
- Flutterは2年ぐらい触ってないのでキャッチアップしたかった
- Bluesky.dartの情報がすぐに見つかった

---

# 画像サイズチェック

- 976.56KBを超える画像をアップロードするとエラーになる
- 976.56KB以下に圧縮してみたがWebクライアントサイドでは無理そう
- 単に警告するだけにした

---

## 画像選択部分のコード

![width:800px](https://github.com/eyasuyuki/blueskystudy3/blob/main/images/pickImage.png?raw=true)

---

# Bluesky接続部分とテスト

- とりあえず本当に接続してテストする
- 本物のID/パスワードを隠蔽するために```flutter_dotenv```を使う
- ```.gitignore```に```.env```を追加する
- プロジェクトルートに```.env```ファイルを作成

![width:600px](https://github.com/eyasuyuki/blueskystudy3/blob/main/images/dot_env.png?raw=true)

---

## Articleクラス

![width:800px](https://github.com/eyasuyuki/blueskystudy3/blob/main/images/Article.png?raw=true)

---

## Blueskyへの投稿

![width:800px](https://github.com/eyasuyuki/blueskystudy3/blob/main/images/postArticle.png?raw=true)

---

## テストmain

![width:800px](https://github.com/eyasuyuki/blueskystudy3/blob/main/images/test_main.png?raw=true)

---

## 画像投稿のテスト

![width:600px](https://github.com/eyasuyuki/blueskystudy3/blob/main/images/testPostArticle.png?raw=true)

※ 本当に投稿されます。

# BlueskyへのPostと記事URLの推測

BlueskyにPostすると例えばこんな結果が帰ってきます。

```json
{"uri":"at://did:plc:dptps7rgxju4nrg6qskop2wz/app.bsky.feed.post/3kjmvo7ocl62e",
 "cid":"bafyreihxcb3n6hxucw3hdeb3kylhzkoumitkx4hygoy24tyxt4hemajdde"}
```

Blueskyの記事URLはこんな感じです。

https://bsky.app/profile/javaopen.org/post/3kjmvo7ocl62e

```3kjmvo7ocl62e```が記事のID、```javaopen.org```は```session.data.handle```で取得できます。

```dart
final session = await bsky.createSession(identifier: article.id, password: article.password);
```

---

## 記事URLの生成



