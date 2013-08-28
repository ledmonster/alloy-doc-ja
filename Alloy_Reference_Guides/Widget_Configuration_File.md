Widget 設定ファイル (widget.json)
=================================

すべての widget は、widget のルートディレクトリに widget.json と呼ばれる設定ファイルを持っています。この設定ファイルは、少しの例外を除いて [npm package.json format](https://npmjs.org/doc/json.html) の仕様を踏襲しています。

widget の設定ファイルは、以下の key, value ペアも受け付けます。

<!--
All widgets have a configuration file called widget.json located in the root directory of the widget. The configuration file follows the npm package.json format with a few exceptions.

The widget configuration file also accepts the following key-value pairs:
-->

| Key                   | Value  | 説明 |
| --------------------- | ------ | ---- |
| id                    | String | 必須。widget のルートフォルダと同じ名前である必要があります。 |
| copyright             | String | Widget のコピーライト情報 |
| license               | String | Widget のライセンス情報 |
| min-alloy-version*    | Float  | Widget を動かすのに必要な Alloy の最小バージョン |
| min-titanium-version* | Float  | Widget を動かすのに必要な Titanium の最小バージョン |
| tags                  | String | キーワードの一覧 |
| platforms*            | 任意のプラットフォームのリスト(カンマ区切り) | どのプラットフォームで動くか |

(*) min-alloy-version, min-titanium-version, platforms は現在必須ではありませんが、将来、Alloy はこれらの値を使ってチェックします。

<!--
| Key                   | Value  | Description |
| --------------------- | ------ | ----------- |
| id                    | String | Mandatory. Needs to be the same name as the widget root folder. |
| copyright             | String | Widget copyright information. |
| license               | String | Widget license information. |
| min-alloy-version*    | Float  | Minimum required Alloy version to run the widget. |
| min-titanium-version* | Float  | Minimum required Titanium version to run the widget. |
| tags                  | String | List of keywords. |
| platforms*            | Any combination of platforms (comma delimited) | Which platforms the widget will run on. |

(*) Currently, the min-alloy-version, min-titanium-version and platforms keys are not required, but in the future, Alloy will perform a check against these values.
-->
