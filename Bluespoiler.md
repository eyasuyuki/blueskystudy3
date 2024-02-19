Blueskyに伏せ字投稿ができるWebアプリの作り方
====
2024-02-21 [Yasuyuki ENDO](https://bsky.app/profile/javaopen.org)

<!-- paginate: true -->

---

# 詳しくはZennに書きました。

https://zenn.dev/eyasuyuki/articles/825b28b0ec0a4c

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
- Bluesky接続部分のテストはどうしたか
- BlueskyへのPostと記事URLの推測

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
- だがfusetterみたいに自前サーバーを持つのは面倒だ
- 画像のALTなら1000文字まで入れられる(邪道)
- 自前バックエンドは持たずにBlueskyだけを使う

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



---

# Bluesky接続部分のテストはどうしたか

---

# BlueskyへのPostと記事URLの推測
