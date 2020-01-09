# OGPanicスターター

OGPanicは、OG画像にタイトルとカテゴリー情報をアイキャッチ画像に追加し、ソーシャルメディア共有時に投稿を目立たせるWebサービスです。ヘッドレスクロムを使用してOG画像をレンダリングするため、基本的に画像内に配置できるものに制限はありません。Webフォントを使用したり、CSSで美しいグラデーションを追加したり、最新のCSSやJavascriptで実装できる他の派手な効果を追加したりができます。

## テンプレートの開発

[OGPanicの公式サイト](https://ogpanic.com)にユーザを登録して、APIエンドポイントとトークンを取得します。


このリポジトリをクローンし、`.env.development`を作成するか、`example.env.development`をコピーして作成します。OGPanicのトークン管理のダッシュボードからAPIエンドポイントとトークンをコピーします。

```
OGPANIC_ENDPOINT=https://ogaas-app-1.ogpanic.com
OGPANIC_TOKEN=AabE@G-13
```

次のコマンドを叩けば、テンプレート開発のUIを立ち上げます。ブラウザで`http://localhost:3000/editor`からアクセスできます。

```bash
$ npm install
# npx ogaas develop --help, for more infomation
$ npm run develop
```

![Template Editor](https://github.com/ogpanic/theme-starter/blob/master/editor.png?raw=true)


### ソースコードの階層


テンプレートは `./src`の下にあり、それぞれが異なるディレクトリにあります。ディレクトリ名は、og-imageをレンダリングするためのテンプレートの名前として使用されます。

例えば ./src/blogの下のテンプレートの名前は`blog`になり、このテンプレートによって生成された画像には`https：// <endpoint>/og/blog/<id>.<jpeg|png>`からアクセスできます。

`./src/<template_name>`には、必須ファイルが2つあります：`index.html`と`data.json`。必要に応じて画像やスタイルシートなどのリソースを追加できます。

### メタデータの準備

OGPanicでは、生成されたog-imageにアクセスするためのid（スラッグ）のみを指定できます。そのIDのメタデータをアップロードしたユーザーのみが、画像に表示する内容をコントロールできます。`data.json`は、テンプレートを設計するときにモックアップ用のメタデータを保持するファイルです。

次の例では、`id`が画像のスラッグとして使用され、`data`に、最終的な画像を生成するために必要なものを何でも入れることができます。

```json
{"id": "first-blog", "data": {"name": "A great blog"}}
```

実稼働環境では、ページが作成または更新されたときにメタデータをアップロードすることをお勧めします。[API](#api)または[コマンドライン](#command-line)を使用して、OGPanicにデータをアップロードできます。

### テンプレートとメタデータの結合

これは、ogイメージの生成に使用されるテンプレートhtmlです。 上記のメタデータの例と次のテンプレートを使用します。

```html
<h1>{{name}}</h1>
```

次のものが得られます。CSSを使用してタイプミスと位置を調整し、見栄えを良くしましょう！

```html
<h1>A great blog</h1>
```

## メタデータのアップロード

`npm run developer`でテンプレートを開発する場合、data.jsonへの変更は自動的にアップロードされます。

次のコードを使用して、実稼働環境でデータをアップロードします。`OGPANIC_ENDPOINT`、`OGPANIC_TOKEN`を環境変数として定義するか、`.env`ファイルに置くことができます。

```javascript
import { uploadMeta } from 'ogaas-cli'

await uploadMeta([
    {id: "my-first-blog", data: {"name": "A awesome blog title"}}
])
```

次のコマンドを使用してアップロードすることもできます。

```bash
# npx ogaas upload --help, for more infomation
$ npx ogaas upload <data.json>
```

## テンプレートのビルドとデプロイ

テンプレートをデプロイするには、エディターUIの`DEPLOY TEMPLATES`ボタンを押すか、コマンドラインを使用します。

コマンドラインの場合は、まずこちらのコマンドでテンプレートをビルドする。

```bash
# npx ogaas build --help, for more infomation
$ npm run build
```

次のビルドされたテンプレートをアップロードします。なお、`.env.production`に正しいAPIエンドポイント/トークンのセットアップがあることを確認してください。

```bash
# npx ogaas deploy --help, for more infomation
$ npm run deploy
```
