DreamHouse サーモスタット
---------------------

このサンプルアプリを使用すると、DreamHouse アプリから Ecobee3 スマートサーモスタットを制御できるようになります。デモをご覧ください。

[![Demo](http://img.youtube.com/vi/83BdO5WjYh8/0.jpg)](http://www.youtube.com/watch?v=83BdO5WjYh8)

初めに、[Ecobee3 スマートサーモスタット](https://www.amazon.com/gp/product/B00ZIRV39M/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00ZIRV39M&linkCode=as2&tag=jamesward-20&linkId=0708922d14ecfb1007ac2b1c24c80d3a)が必要です。これを実際の空調システム（HVAC）または[モックの HVAC システム](https://www.jamesward.com/2016/05/17/building-a-mock-hvac-for-smart-thermostat-demos)と接続します。サーモスタットをセットアップし、インターネットに接続して、Ecobee アカウントと接続します。

ローカルで実行：

1. [ngrok](https://ngrok.com/download) をインストールします。
1. ngrok を実行します。
`ngrok http 5000`
1. [Ecobee のデベロッパーダッシュボード](https://www.ecobee.com/consumerportal/index.html#/dev)で新しいデベロッパーアプリを作成し、*Authorization Method（認証方式）*を `Authorization Code`（認証コード）に、*Redirect Domain（リダイレクトドメイン）*を、使用する ngrok ドメインに設定します。`API key` をメモしておきます。
1. アプリを起動します。
`ECOBEE_APP_KEY=<Ecobee アプリのキー> npm run dev`
1. 暖房（またはモックの暖房）をオンにします。
`http://<ngrok ID>.ngrok.io/on`
1. アプリの認証が完了すると、サーモスタットの設定温度が 10 度上がり、暖房が入ります。
1. 暖房をオフにします。
`http://<ngrok ID>.ngrok.io/off`

Heroku で実行：

1. [Ecobee のデベロッパーダッシュボード](https://www.ecobee.com/consumerportal/index.html#/dev)で新しいデベロッパーアプリを作成し、*Authorization Method（認証方式）*を `Authorization Code`（認証コード）に、*Redirect Domain（リダイレクトドメイン）*を、仮のドメイン名に設定します。`API key` をメモしておきます。
1. アプリをデプロイします。[![Deploy on Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)
1. 新しく作成した Ecobee デベロッパーアプリの *Redirect Domain（リダイレクトドメイン）*を Heroku アプリのドメイン（例：`foo-bar-123.herokuapp.com`）に変更します。
1. 暖房をオンにします。
`http://<Heroku アプリの名前>.herokuapp.com/on`

### アプリのアーキテクチャ

このアプリケーションでは、Ecobee の OAuth API を使用して、Ecobee REST API で使用するアクセストークンを取得します。ここでは、デモ用にアクセストークン（およびリフレッシュトークン）をメモリにキャッシュします。実際の環境で使用するアプリでは、よりセキュアで、マルチユーザー対応の認証処理方法を使用する必要があります。アクセストークンを使用して現在の温度を読み取り、10 度単位で温度を上げ下げします（`/on` と `/off` のどちらのエンドポイントにアクセスするかに応じます）。これは、REST API 呼び出しから [Ecobee API](https://www.ecobee.com/home/developer/api/introduction/index.shtml) を使用することで実行します。このアプリは、Node.js と Express で作成されており、ソースの全文は `server.js` ファイルにあります。
