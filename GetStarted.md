#TypeScript覚え書き
#プロジェクトディレクトリの作成
```zsh
npm init -y
#こいつでプロジェクトディレクトリの初期化
npm install typescript ts-loader webpack webpack-cli webpack-dev-server --save-dev
#これで必要なツールをインストール(詳細は下表)
```
|ツール名|内容|
|---|---|
|typescript|TypeScriptコンパイラ|
|ts-loader |Webpack上でTypeScriptを使うためのローダー|
|webpack   |JavaScript用ビルドツール(モジュールバンドラーっていうらしい)|
|webpack-cli|CLIからwebpackを使うためのツール|
|webpack-dev-server|webpackの機能を広く補助するサーバー|

##package.jsonをいじる
package.jsonのscriptsに  

- `"build": "webpack --mode=development"`
- `"start": "webpack-dev-server --mode=development"`  

を追加  

これにより
- `npm start`でwebpack-dev-serverを起動
- `npm run build`でwebpackのビルド処理

をできるようになった

##webpack用の設定
webpack.config.jsにwebpack用の設定を書く  
今回は下記の通り
```JavaScript
const path = require('path');
module.exports = {
	// モジュールバンドルを行う起点となるファイルの指定
	// ファイル名、それの配列やオブジェクトが指定できる
	// 今回はオブジェクトとして指定した場合
	entry: {
		bundle : './src/app.ts'
	},
	output: {
		// モジュールバンドルを行った結果
		// "__dirname"はファイルが存在するディレクトリを表すもの
		path: path.join(__dirname, 'dist'),
		filename: '[name].js' // [name]はentryで記述した名前(この場合bundle)
	},
	// モジュールとして扱うファイルの拡張子を指定する
	// 例えばimport Foo from './foo'とした場合には"foo.ts"を参照する
	// デフォは['.js', '.json']
	resolve: {
		extension:['.ts', '.js']
	},
	devServer: {
		// webpack-dev-serverの公開フォルダ(HTMLとか置いとくとこ)
		contentBase: path.join(__dirname, 'dist'),
	},
	// モジュールに適用するルールの設定(大体の場合はローダーの設定をする)
	modules: {
		rules: [
			{
				// 拡張子が.tsで終わるファイルに対して、TypeScriptコンパイラを適用する
				test:/\.ts$/,loader:'ts-loader'
			}
		]
	}
}
```
##TypeScriptコンパイラの設定

```zsh
tsc --init
```
はい、これで終わり  
これで`tsconfig.json`が生成される  
これの中には初期設定+指定できるコンパイルオブション全てが書かれている  
初期設定は以下の通り  
```json
{
	"compilerOptions": {
		"target":          "es5",
		"module":          commonjs,
		"strict":          true,
		"esModuleInterpo": true
	}
}
```

|オプション名|内容|
|---|---|
|target|コンパイル後のJSのバージョンの指定|
|module|コンパイル時にモジュール関連のコードをどの方式として扱うかを指定|
|strict|暗黙的なany型を禁止するなどコンパイル時のチェックを厳格にする|
|esModuleInterpo|CommonJS形式の外部ライブラリのモジュールを妥当に扱えるようにする|
#参考にした記事
[TypeScriptチュートリアル① -環境構築編-](https://qiita.com/drafts/cd9d4cc31da9c9969639/edit)``
