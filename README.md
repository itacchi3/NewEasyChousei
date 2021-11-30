# チョーセイ
## 概要

LINE上で動く，スマホ入力に特化した，時間ごとの調整アプリケーションです．

日程調整の際の負担をなるべく少なくするように努力しています．

↓チョーセイの公式アカウント

![M](https://user-images.githubusercontent.com/72689870/144077950-46a1bc9e-fb2d-4ac0-9371-4a8f409542fd.png)

↓PC用URL(基本はLINE(ios/android)で使うことを想定しています)

https://chousei.vercel.app/






## なぜ作ったの？
所属していたアカペラサークルでは，グループごとに練習や会議などをよく行うため，時間ごとの日程調整をよく行います．しかし，既存のサービスではスマホでかなり使いづらく，サークル員の不満が募っていました．  

そこでスマホでも入力しやすい時間ごとの日程調整アプリを開発することにしました．

## どんな人を対象にしている？
サークルの人達だけでなく，会議や練習などの日程調整や，バイトのシフト調整をLINEのグループでよく行う全ての人達に使ってほしいです．

## 使った技術は？？
|  技術  |  理由  |
| ---- | ---- |
|  Next.js  |  SSRを行うため，簡易的なAPIを構築するため  |
|Vercel| Next.jsとの相性が良いため|
|  TypeScript  |  静的型付けの恩恵を受けるため  |
|LIFF|LINE上で動くようにするため，ログインを自動的に行うため|
|ChakraUI|ある程度デザインを任せることができ，カスタマイズもかなり可能なため|
|PlanetScale|他のDBaaSと比べて，無料枠がかなり広いため|
|Prisma|型安全にDBを操作できるため|

## ユーザー認証
LINEログインを用いています．LINE（ios/android）でURLを開いた場合は自動的にログインされます．

## セキュリティ
ユーザーのプロフィール情報用いて，データの追加や更新を行う際には，クライアント側で発行したIDトークンをサーバーに送信し，サーバーでIDトークンを検証することで，安全に取得したプロフィール情報を用いるようにしています．

参考URL: https://developers.line.biz/ja/docs/liff/using-user-profile/#use-user-info-on-server

## こだわりのポイントは？
### 日程調整の際の負担を低減
LIFFを用いることで，会員登録やインストールといった手間をなくし使い始めのハードルをなくすことができます．
また，LINE上ですべてが完結するので，他のアプリに移動して調整を行うといった心理的負担も低減しています．

### ユーザー体験の向上
既存のサービスではイベントの作成をテキストベースで行っているなど，かなり入力に時間がかかっていました．そこで，直感的な操作で日程表の作成や予定の入力をできるようにすることでスムーズに日程調整ができるようにしました．

また，LIFFとNext.jsの組み合わせにおいて，LIFFではwindowが使われているので普通にimportしてしまうと，Nextjsのサーバサイド処理でエラーが生じてしまいます．そのため，コンテンツをレンダリングする前に毎回クライアント側でLIFFをimportする必要があるので，コンテンツが見ることができるようになるまで少し時間がかかります．

この時間中にユーザーが不安にならないようにするために，LIFFをimportしている時間はloading中だとわかるようにスピナーを表示するようにしました．

また，loading後にレイアウトシフトを起こしてしまうとユーザー体験が悪くなってしまいます．そのため，SSRを行うことによってloading後にコンテンツが完全に揃っているようにしました．SSRを行うとユーザーがコンテンツを見ることができるようになるまでの時間がかかってしまうのですが，この時間をなるべく短くするために，ホスティングサーバーとDBサーバーのリージョンを近い距離にし，DBへのリクエストを並列に処理するようにしました．

