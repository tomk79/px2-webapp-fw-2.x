# Paprika Framework for Pickles 2
Publishing Web Application with "Pickles 2".


## Setup - セットアップ手順

### [Pickles 2 プロジェクト](https://pickles2.pxt.jp/) をセットアップ

### 1. `composer.json` に、パッケージ情報を追加

```json
{
    "require": {
        "pickles2/px2-paprika": "^0.1"
    }
}
```

### 2. `composer update` を実行

更新したパッケージ情報を反映します。

```
$ composer update
```


### 3. `.htaccess` を開き、 `RewriteCond` の条件に 拡張子 `.php` を追加

```
#-------------------------
#  for pickles2
<IfModule mod_rewrite.c>

	# ...中略...

	RewriteCond %{REQUEST_URI} /(.*?\.(?:html|htm|css|js|php(?:/.*)?))?$
	RewriteRule ^(.*)$ \.px_execute\.php/$1 [L]

	# ...中略...

</IfModule>
```

### 4. `px-files/config.php` を開き、プラグインを設定

#### `paths_proc_type` を設定

`*.php` を追加する。

```php
<?php
/**
 * config.php template
 */
return call_user_func( function(){

	// ...中略...

	// 拡張子 `*.php` を、php用プロセッサに関連付け
	$conf->paths_proc_type = array(
		// ...中略...

		'*.php' => 'php', // <- for Paprika Framework

		// ...中略...
	);
```

#### `paths_enable_sitemap` を設定

`*.php` を追加する。

```php
	// 拡張子 `*.php` で、サイトマップを有効化
	$conf->paths_enable_sitemap = array(
		// ...中略...

		'*.php', // <- for Paprika Framework

		// ...中略...
	);
```

#### `funcs->before_content` を設定

```php
	/**
	 * funcs: Before content
	 */
	$conf->funcs->before_content = array(

		// Paprika - PHPアプリケーションフレームワーク
		// before_content の先頭に設定してください。
		'tomk79\pickles2\paprikaFramework2\main::before_content('.json_encode( array(
			// アプリケーションが動的に生成したコンテンツエリアの名称
			'bowls'=>array('custom_area_1', 'custom_area_2', ),

			// Paprika を適用する拡張子の一覧
			'exts' => array('php'),
		) ).')' ,

		// ...中略...
	);
```

#### `funcs->processor->php` 設定を追加

```php
	/**
	 * processor
	 */
	$conf->funcs->processor->php = array(
		// Paprika - PHPアプリケーションフレームワーク
		'tomk79\pickles2\paprikaFramework2\main::processor' ,

		// html のデフォルトの処理を追加
		$conf->funcs->processor->html ,
	);
```

### 5. アプリケーションを初期化する。

このコマンドははじめに一度だけ実行します。
データベースなどが初期構築されます。

```
$ php path/to/.px_execute.php /?PX=paprika.init
```


## Paprika を `.html` 拡張子のページにも適用するには

1. `exts` オプションに `html` を追加します。
2. `$conf->funcs->processor->html` の先頭にも `tomk79\pickles2\paprikaFramework2\main::processor` を追加します。
3. パブリッシュ先のディレクトリに、 `.html` 拡張子でも PHPが実行されるよう設定します。 `.htaccess` で 設定する場合、 `AddHandler application/x-httpd-php .php .html` のように書きます。


## PXコマンド - PX Commands

- `paprika.init` - データベースを初期化する。
- `paprika.publish_template` - アプリケーションのためのテンプレートファイルを生成する。(フレームワークの内部で暗黙的にコールされます)


## 変更履歴 - Change Log

### pickles2/px2-paprika v0.2.0 (リリース日未定)

- `paprika_prepend.php` の仕組みを廃止。
- Paprika環境変数から、 `realpath_controot_preview`、`realpath_files_private_cache` を削除。
- Paprika環境変数 `realpath_homedir` は、Pickles 2 のホームディレクトリではなく、 Paprika のホームディレクトリを返すようになった。

### pickles2/px2-paprika v0.1.1 (2019年11月17日)

- Windows + PHP7 の環境で、CSV ファイルを正しく読み込めない問題に対応した。

### pickles2/px2-paprika v0.1.0 (2018年9月25日)

- Pickles 2 グループへ移管した。

### tomk79/px2-paprika v0.0.1 (2018年9月19日)

- Initial Release.

## ライセンス - License

MIT License


## 作者 - Author

- (C)Tomoya Koyanagi <tomk79@gmail.com>
- website: <https://www.pxt.jp/>
- Twitter: @tomk79 <https://twitter.com/tomk79/>
