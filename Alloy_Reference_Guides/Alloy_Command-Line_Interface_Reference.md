Alloy CLI リファレンス
======================

- [New](#new)
- [Generate](#generate)
- [インストール](#インストール)
- [Compile](#compile)
- [Run](#run)
- [その他のオプション](#その他のオプション)


Alloy のコマンドラインインタフェース（CLI）は Alloy プロジェクトの管理や構築のためのコマンドラインツールです。

<!--
The Alloy Command-Line Interface (CLI) is a command-line tool for managing and building Alloy projects.
-->

New
---

新しい Alloy プロジェクトを作成する。

<!--
Creates a new Alloy project.
-->

```JavaScript
alloy new [<project_path>] [<project_template>] [--force] [--no-colors]
```

| Options              | Description |
| -------------------- | ----------- |
| \<project_path\>     | Titanium プロジェクトのひな形へのパスです。指定しない場合は現在いるディレクトリが利用されます。 |
| \<project_template\> | テンプレートを選択してください。単一画面のアプリケーションでは **default** を選択し、タブ付きのアプリケーションでは **two_tabbed** を選択してください。デフォルトは **default** です。 |
| -f, --force          | コマンドを強制実行します。 |
| -n, --no-colors      | カラー出力を無効化します。 |

<!--
| Options              | Description |
| -------------------- | ----------- |
| \<project_path\>     | Path to a skeleton Titanium project, otherwise the current working directory is used. |
| \<project_template\> | Selects the template. Choose between default for a single pane application or two_tabbed or a tabbed application. Defaults to default. |
| -f, --force          | Forces the command to execute. |
| -n, --no-colors      | Disables color output. |
-->

Generate
--------

Alloy コンポーネントの骨組みを生成する。

<!--
Creates skeleton Alloy components.
-->

```JavaScript
alloy generate <component> [--outputPath <output_path>] [--platform <platform>] [--force] [--no-colors] 
```

| Options | Description |
| -------------------- | ----------- |
| \<component\> | 書式はコンポーネントによります。 |
| controller \<name\> | Controller, View, Style のひな形を生成します。 |
| jmk | *alloy.jmk* を生成します。 |
| model \<name\> \<adapter\> [schema] | 指定された名前で Model を作成します。以下の [Model フォーマット](#Model フォーマット) を参照してください。 |
| migration \<model_name\> | 指定された Model 用に Migration ファイルのひな形を作成します。 |
| style <\<name\> &#124; --all> | 指定された名前で Style ファイルのひな形を作成します。<br>名前が View Controller に紐付いている場合、 Alloy は Style ファイルを生成するのに、 markup ファイルの関連する id や class 名を利用します。<br> *--all* フラグが指定された場合、Alloy はすべての View Controller 用に style ファイルを生成します。<br>markup ファイルで新しい id や class 属性を追加した場合は、このコマンドを実行することで、 Style ファイルに新しい属性情報を追加することができます。|
| view \<name\> | 指定された名前で View と Style のひな形を作成します。 |
| widget \<name\> | 指定された名前で基本的な widget を作成します。 |
| -o, --outputPath \<outputPath\> | 生成コードの出力パスです。 'app' ディレクトリを指します。 |
| --platform \<platform\> | Platform 特有の View または Controller を生成します。 \<platform\> は android, ios, mobileweb のいずれかです。 |
| -f, --force | コマンドを矯正実行します。 |
| -n, --no-colors | カラー出力を無効化します。 |

<!--
| Options | Description |
| -------------------- | ----------- |
| \<component\> | Format is component specific. |
| controller \<name\> | Create a skeleton controller, view and style. |
| jmk | Creates alloy.jmk. |
| model \<name\> \<adapter\> [schema] | Creates a model with the specified name. See Model Format below. |
| migration \<model_name\> | Creates a skeleton migration file for the specified model. |
| style <\<name\> &#124; --all> | Creates a skeleton style file with the specified name. <br>If the name is associated with a view-controller, Alloy uses the id and class names from the markup file to populate the style file.<br>If --all flag is specified, Alloy generates skeleton style files for all view-controllers.<br>If you add new id or class attributes to the markup file, running this command updates the style file with the new attributes.\
| view \<name\> | Creates a skeleton view and style with the specified name. |
| widget \<name\> | Creates a basic widget with the specified name. |
| -o, --outputPath \<outputPath\> | Output path for generated code. Point to the 'app' directory. |
| --platform \<platform\> | Create a platform-specific view or controller component, where \<platform\> is either android, ios or mobileweb. |
| -f, --force | Forces the command to execute. |
| -n, --no-colors | Disables color output. |
-->

### Model フォーマット

Model を生成するには、はじめに adapter type を選択します。

<!--
To generate a model, first select the adapter type:
-->

- Android や iOS プラットフォーム用の SQLite データベースには 'sql' を指定します。
- Mobile Web プラットフォームで利用する HTML5 のローカルストレージ用には 'localStorage' を指定します。
- モデルを Titanium SDK のコンテキストのローカルに保存するには 'properties' を指定します。

<!--
- 'sql' for the SQLite database for the Android and iOS platform.
- 'localStorage' for HTML5 localStorage for the Mobile Web platform.
- 'properties' for storing models locally in Titanium SDK context.
-->

'sql' や 'properties' アダプタに対しては、テーブルスキーマも指定する必要があります。 'sql' アダプタはモデル用の migration ファイルも生成します。

<!--
For the 'sql' and 'properties' adapters, you will also need to specify a table schema. The 'sql' adapter type will also generate a migration file with the model.
-->

モデルを生成するためのテーブルスキーマの書式は、フィールド名とデータ種別をコロン (':') でつなげたものをスペースでつなげたリストです。たとえば、 'name:string age:number sex:varchar bob:date' は文字列型の 'name' カラムと、数値型の 'age' カラム、 文字型の 'sex' カラム、日付型の 'bob カラムを生成します。

<!--
The table schema format for generating models is a space-delimited list of the field name, followed by a colon (':'), followed by the data type. For example, specifying 'name:string age:number sex:varchar dob:date' will create a table with four columns: 'name' which stores a string, 'age' which stores a number, 'sex' which stores a character, and 'dob' which stores a date.
-->

実際のところ、上記の例において、 Android や iOS のプラットフォームは SQLite を使っているので、これらのデータタイプを利用することができません。代わりに SQLite では以下のようにマッピングされます。

<!--
Actually, in the above example, since both the Android and iOS platforms use SQLite, none of these datatypes are available. Instead, they will be mapped based on the following:
-->

| Datatype                                                 | SQLite Datatype |
| -------------------------------------------------------- | --------------- |
| string <br> varchar <br> text                            | TEXT            |
| int <br> tinyint <br> smallint <br> bigint <br> integer  | INTEGER         |
| double <br> float <br> real                              | REAL            |
| blob                                                     | BLOB            |
| decimal <br> number <br> date <br> datetime <br> boolean | NUMERIC         |
| null                                                     | NULL            |
| unknown datatype                                         | TEXT            |


インストール
------------

Alloy の特別なコンポーネントをインストールする。

<!--
Installs special Alloy project components.
-->

```JavaScript
alloy install <module> [<project_path>]
```

| Options          | Description                                                                 |
| ---------------- | --------------------------------------------------------------------------- |
| \<module\>       | 書式はモジュールによります。                                                |
| plugin           | Alloy プロジェクトを Studio へフックするためのコンパイラプラグインをインストールします。|
| <project_path>   | Alloy プロジェクトへのパスです。指定しない場合は現在のディレクトリです。    |

<!--
| Options          | Description                                                                 |
| ---------------- | --------------------------------------------------------------------------- |
| \<module\>       | Format is module specific.                                                  |
| plugin           | Installs the compiler plugin to hook the Alloy project to Studio.           |
| \<project_path\> | Path to the Alloy project, otherwise the current working directory is used. |
-->

Compile
-------

Alloy のコードを Titanium SDK のコードにコンパイルする。

<!--
Compiles Alloy code to Titanium SDK code.
-->

```JavaScript
alloy compile [<project_path>] [--config <compiler_options>] [--no-colors]
```

| Options                         | Description                                                                 |
| ------------------------------- | --------------------------------------------------------------------------- |
| <project_path>                  | Alloy プロジェクトへのパスです。指定しない場合は現在のディレクトリです。    |
| -c, --config <compiler_options> | コンパイラ設定オプションです。 [コンパイラオプション](#コンパイラオプション) を参照してください。 |
| -n, --no-colors                 | カラー出力を無効化します。                                                  |

<!--
| Options                         | Description                                                                 |
| ------------------------------- | --------------------------------------------------------------------------- |
| <project_path>                  | Path to the Alloy project, otherwise the current working directory is used. |
| -c, --config <compiler_options> | Compiler configuration options to use. See Compiler Options below.          |
| -n, --no-colors                 | Disables color output.                                                      |
-->

### コンパイラオプション

コンパイラオプションは、空白なしのカンマ区切りの代入書式（変数=値）で指定します。たとえば、 "beautify=false,deploytype=test" と指定することでデフォルトのコンパイラの振る舞いを上書きし、生成される JavaScript ファイルの整形機能を無効化し、アプリケーションをテスト環境用にビルドします。

指定可能なコンパイラオプションについては、 [Build 設定ファイル (alloy.jmk)](Alloy_Reference_Guides/Build_Configuration_File.md) の *event.alloyConfig* オブジェクトを参照してください。

<!--
The compiler options are a list of comma-delimited assignment statements (variable=value) without any spaces. For example, specifying "beautify=false,deploytype=test" overrides the default compiler behavior to disable beautifying the generated JavaScript files and building the application for the test environment.

Refer to the event.alloyConfig object in Build Configuration File (alloy.jmk) for a list of settable compiler options.
-->

Run
---

Alloy プロジェクトを動かすには、 Titanium のビルドコマンドを利用してください。詳細は [Titanium Command-Line Interface Reference guide](http://docs.appcelerator.com/titanium/3.0/#!/guide/Titanium_Command-Line_Interface_Reference) を参照してください。

<!--
Use the titanium build command to run Alloy projects. See the Titanium Command-Line Interface Reference guide for more information.
-->

その他のオプション
------------------

| Options       | Description            |
| ------------- | ---------------------- |
| -h, --help    | コマンドの使い方を出力 |
| -v, --version | バージョン番号を出力   |
