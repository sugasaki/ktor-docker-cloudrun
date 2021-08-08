Cloud RunへGithubから継続的デプロイ


↓を参考に進めました

[Cloud Run へのデプロイ  \|  Cloud Build のドキュメント  \|  Google Cloud](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run?hl=ja#cloud-run_1)


# プロジェクト作成


アプリを作成・構築・メンテするための`GCPのプロジェクト`を作成します。

![スクリーンショット 2021-08-08 15.20.11.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/181c32cc-0055-40b0-0274-4a280feadf05.png "スクリーンショット 2021-08-08 15.20.11.png")



# Cloud Run

Cloud Runのページから、サービスの作成を選択

![スクリーンショット 2021-08-08 15.29.18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/50fce047-adbe-9772-ebf7-8fa7df59a628.png "スクリーンショット 2021-08-08 15.29.18.png")


## 1. サービスの設定

サービス名を入力、リージョンを選択して、次へ

![スクリーンショット 2021-08-08 15.31.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/11f66cbe-705b-a70f-5c57-0a0a71ab9b33.png "スクリーンショット 2021-08-08 15.31.12.png")


## 2. サービスの最初のリビジョンの構成


ここで、`ソース リポジトリから新しいリビジョンを継続的にデプロイする` を選択し、
`SET UP WITH CLOUD BUILD` を押す

![スクリーンショット 2021-08-08 15.33.02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/4f1f8003-0e46-304d-7288-6012f895a1cc.png "スクリーンショット 2021-08-08 15.33.02.png")


## 3. Cloud Build の設定

Cloud Build の設定が開く

### 3.1 リポジトリの選択

![スクリーンショット 2021-08-08 15.34.52.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/868ea619-2f4a-2204-23db-397d732c9205.png "スクリーンショット 2021-08-08 15.34.52.png")


プロジェクトを新規作成した後であれば以下のようなメッセージが表示さえる。各々のAPIの設定を行う

- イメージを作成するには、Cloud Build API が必要です。
- （省略可）Container Analysis（無料）を使用してソース情報を取得して表示します

ボタンをぽちっと押すとAPI連携が始まる
![スクリーンショット 2021-08-08 15.37.24.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/9e772271-206b-a6f7-7091-f1e463c09020.png "スクリーンショット 2021-08-08 15.37.24.png")


ここでは、[以前作った](https://qiita.com/sugasaki/items/d5800aedafc7dd3f528c)Server Side Kotlinnの[リポジトリ](https://github.com/sugasaki/ktor-docker-hello)を指定しました。

https://qiita.com/sugasaki/items/d5800aedafc7dd3f528c


![スクリーンショット 2021-08-08 15.39.55.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/c7bcf3ce-ecbb-afe1-5b49-7f5c8c09484d.png "スクリーンショット 2021-08-08 15.39.55.png")

リポジトリのミラーリングは未選択で進みました。

### 3.2 ブランチの選択

#### ブランチ選択

継続的デプロイに使用するブランチを選択します。

![スクリーンショット 2021-08-08 15.44.48.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/ab2df42f-b52f-bf16-9425-3485a26debca.png "スクリーンショット 2021-08-08 15.44.48.png")

#### Build Typeの選択

Githubのブランチ内にDockerfileをPUSH済なので、それを使います。
![スクリーンショット 2021-08-08 15.47.43.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/30228d43-6a70-1be9-8349-db6f93a143a3.png "スクリーンショット 2021-08-08 15.47.43.png")

入力終わったら保存を押します。

### 3.3 詳細設定

ポート番号や最大リクエスト数などの詳細設定ができます。
必要におうじて変更をおこない、必要がなければ、次へボタンを押下

![スクリーンショット 2021-08-08 15.51.18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/3148654b-c2a8-8bbc-6d40-cda54ec25858.png "スクリーンショット 2021-08-08 15.51.18.png")


### 3.4 サービスをトリガーする方法の構成

![スクリーンショット 2021-08-08 15.52.54.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/47b7694a-9861-7435-a2a1-8a4dac28733b.png "スクリーンショット 2021-08-08 15.52.54.png")

以下を選択して、作成ボタンを押す

- すべてのトラフィックを許可する
- 未認証の呼び出しを許可


注）未認証の呼び出しを許可を選択しないと、外部から作成したサービスにアクセスできないので選択すること。

作成ボタンを押すと「継続的デプロイを設定しています」の画面に切り替わります。

![スクリーンショット 2021-08-08 15.56.48.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/33fc0f27-9d8e-221c-f69c-343ddce2e3ee.png "スクリーンショット 2021-08-08 15.56.48.png")


## Cloud Run管理画面へ

「継続的デプロイを設定しています」の画面で、しばらく待つとジョブの実行が完了します。


ジョブの実行が完了すると、URLが表示されます。

![スクリーンショット 2021-08-08 16.02.40.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/8e5489fd-3d6c-9d36-78ac-651a863d99e7.png "スクリーンショット 2021-08-08 16.02.40.png")

URLにアクセスすると、作成したアプリが表示されます。

![スクリーンショット 2021-08-08 16.11.47.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/6d5b9f6d-3461-7a51-6e59-a40a6ebe3574.png "スクリーンショット 2021-08-08 16.11.47.png")



# 継続的デプロイ

GithubへPUSHを検知して、Cloud Buildが起動します。

![スクリーンショット 2021-08-08 16.07.46.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/0e0abc02-7f97-5bb1-02c3-d1581a4077d2.png "スクリーンショット 2021-08-08 16.07.46.png")


ビルドが完了すると、デプロイされ、アプリが変更されます。

![スクリーンショット 2021-08-08 16.13.43.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/49910/1056a53e-a446-10eb-8036-84c80ea87710.png "スクリーンショット 2021-08-08 16.13.43.png")





以上です


