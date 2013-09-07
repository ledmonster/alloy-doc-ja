Alloy デバッグ・トラブルシューティング
======================================

本文書は[Alloy Debugging and Troubleshooting](http://docs.appcelerator.com/titanium/3.0/#!/guide/Alloy_Debugging_and_Troubleshooting)の日本語訳です。


- [概要](#概要)
- [デバッグ](#デバッグ)
  - [Studio Debugger](#studio-debugger)
  - [コンパイラのエラーメッセージ](#コンパイラのエラーメッセージ)
- [トラブルシューティング](#トラブルシューティング)
  - [\[ERROR\] No app.js found. Ensure the app.js file exists in your project's Resources directory.](#error-no-appjs-found-ensure-the-appjs-file-exists-in-your-projects-resources-directory)
  - [Android: Images, HTML pages and other assets do not display](#android-images-html-pages-and-other-assets-do-not-display)
  - [Android Runtime Error: Uncaught TypeError: Cannot call method xxx of undefined](#android-runtime-error-uncaught-typeerror-cannot-call-method-xxx-of-undefined)
  - [Android Runtime Error: Uncaught ReferenceError: Alloy is not defined](#android-runtime-error-uncaught-referenceerror-alloy-is-not-defined)
  - [iOS Application Error: invalid method (xxx) passed to UIModule (unknown file).](#ios-application-error-invalid-method-xxx-passed-to-uimodule-unknown-file)
  - [iOS Application Error: undefined is not an object (evaluating $.xxx.open) (unknown file).](#ios-application-error-undefined-is-not-an-object-evaluating-$xxxopen-unknown-file)
  - [Mobile Web: Changes to code do not take effect](#mobile-web-changes-to-code-do-not-take-effect)
  - [Mobile Web: [ERROR] alloy run not supported by mobileweb](#mobile-web-error-alloy-run-not-supported-by-mobileweb)
  - [Studio: Unable to find alloy binary](#studio-unable-to-find-alloy-binary)
- [助けを求める](#助けを求める)
- [バグレポートを送る](#バグレポートを送る)


概要
----

本ガイドでは Alloy アプリケーションのデバッグとトラブルシューティングについて取り扱います。

<!--
This guide covers debugging and troubleshooting Alloy applications.
-->

デバッグ
--------

### Studio Debugger

Alloy アプリケーションのデバッグには Studio Debugger が使えます。Studio Debugger を使うことで、コードの中にブレイクポイントを設定して、ある特定の行を実行する直前でアプリケーションを一時停止し、変数やその値を調べることができます。Titanium Studio でデバッガーを使うためのより詳細な情報については[Debugging in Studio](http://docs.appcelerator.com/titanium/3.0/#!/guide/Debugging_in_Studio)を参照してください。

<!--
The Studio debugger can be utilized to debug your Alloy application. The Studio debugger allows you to set breakpoints in your code to pause your application before it executes a line and to examine variables and their values. Review Debugging in Studio for detailed information about using the debugger in Studio.
-->

Alloy Controller や alloy.js でブレイクポイントを追加・削除するには、左側の行番号の左の空間をダブルクリックしてください。ブレイクポイントを設定すると、青いドットが表示されます。これらのブレイクポイントは、Resources ディレクトリに生成される Titanium ファイルのコードにひも付きます。

<!--
Create breakpoints in your Alloy Controllers or alloy.js file by double-clicking in the left margin on the left side of a line number to add or remove a breakpoint. A blue dot appears for your breakpoint. These breakpoints map to the code in the generated Titanium file located in the Resources directory.
-->

app ディレクトリにある CommonJS や Alloy のモデルファイルに設定されたブレイクポイントは、デバッグ時に無視されます。これらの要素にブレイクポイントを設定するには、Alloy CLI で生成された Titanium のファイルにブレイクポイントを設定する必要があります。CommonJS のモジュールは Resources ディレクトリにコピーされ、 Alloy Model の生成されたファイルは Resources/alloy/models ディレクトリに配置されます。

<!--
Breakpoints set in the CommonJS modules and Alloy Model files in the app directory are ignored during debugging. To add breakpoints for these items, you need to add the breakpoints to the generated Titanium files created by the Alloy CLI. The CommonJS modules are copied to the Resources directory and the generated Alloy Model files are located in the Resources/alloy/models directory.
-->

*App Explorer* ビューで Resources フォルダが隠れている場合は、 **View Menu** ボタン(白い下向きの三角形)をクリックして、 **Customize Views** を選択して ... *Available Customization* ダイアログを表示してください。 *Filters* タブで *Titanium Resources Folder* のチェックボックスを外し、 **OK** ボタンをクリックしてください。 *App Exploer* ビューで Resources フォルダが表示されるはずです。

<!--
If your Resources folder is hidden, in the App Explorer view, click the View Menu button (white triangle pointing down) and select Customize Views..., then the Available Customizations dialog appears. In the Filters tab, uncheck the Titanium Resources Folder checkbox, then click the OK button. The Resources folder should appear in the App Explorer view.
-->

Resources ディレクトリ内のファイルに対して設定されたブレイクポイントは、app ディレクトリ内のファイルを編集した場合、削除されたり、適切な行に紐付かなくなる場合があります。コードをコンパイルした後に、 Resources ディレクトリ内のファイルに設定したブレイクポイントがそのまま設定されていて、正しい場所（行番号）にあることを確認して下さい。正しく設定されていなかった場合は、ブレイクポイントを再設定して、デバッグモードでプロジェクトに戻ってください。

<!--
Breakpoints set in the files in the Resources directory may be cleared or not mapped to the correct line number if you modify any of the files in the app directory. After your code has been compiled, confirm that the breakpoints set in the files in the Resources directory are still set and in the correct location (line number). If not, clear and reset your breakpoints, then rerun the project in debug mode.
-->

*App Explorer* ビューで Application をデバッグする準備が整ったら、 **Debug** ボタン（フォルダに緑の虫がついたボタン）をクリックしてデバッガを起動し、プラットフォームを選択してください。コードのコンパイルが開始される前に、 Perspective の切替の確認画面が表示されるので、 **Yes** をクリックしてください。

<!--
When you are ready to debug your application, in the App Explorer view, run the debugger by clicking on the Debug button (green bug with a folder) and select a platform. Before the code starts to compile, a dialog appears asking to switch the perspective. Click the Yes button.
-->

*Debug* Perspective では、コードに対してステップイン、停止、再開するための *Debug* ビューを利用することができます。code の実行が停止されている間、 *Variables* ビューで現在の変数とその値を見ることができます。

<!--
In the Debug perspective, you can use the Debug view to step into, pause or resume your code. While your code is paused, in the Variables view, you can view your current variables and their values.
-->

> Alloy Release 1.1.0, Studio 3.1.0 以前のバージョンでは、ブレイクポイントを Controller のコード (app/controllers) に作成すると、ブレイクポイントが無視されます。代わりに Resources/alloy/controllers ディレクトリにある、生成された Titanium のコードに対してブレイクポイントを設定してください。

<!--
> Prior to Alloy Release 1.1.0 and Studio 3.1.0, if you create breakpoints in your controller code (app/controllers), your breakpoints are ignored. Instead, place your breakpoints in the generated Titanium code located in the Resources/alloy/controllers directory.
-->

### コンパイラのエラーメッセージ

Alloy のコンパイラは JavaScript, JSON, TSS, XML ファイルの syntax error に対してエラーメッセージを生成します。エラーメッセージには、エラーの発生したファイルと行、文字の場所と、説明が含まれます。

Titanium Stadio では、これらのエラーメッセージが Console ビューに出力されます。

<!--
The Alloy compiler generates error messages for syntax errors in the JavaScript, JSON, TSS and XML files. The error messages report the file, line and character position, and a description of the error.

In Studio, these error messages output to the Console View.
-->

トラブルシューティング
----------------------

### [ERROR] No app.js found. Ensure the app.js file exists in your project's Resources directory.

Resources フォルダの一部コンテンツが削除されている場合は、 ```alloy compile --config platform=<platform>``` を実行してファイルを再生成してください。

<!--
If part of the contents of your Resources folder were deleted, run ```alloy compile --config platform=<platform>``` to regenerate the missing files.
-->

### Android: Images, HTML pages and other assets do not display

iOS や Mobile Web アプリケーションで asset が表示されていて、 Android では表示されていない場合は、 asset へのパスがスラッシュ ('/') で始まるようにする必要があります。iOS, Mobile Web プラットフォームは相対パスと絶対パスをサポートしていますが、Android プラットフォームは絶対パスしかサポートしていません。

<!--
If assets are being displayed for iOS and Mobile Web applications and not Android applications, the path to the asset needs to be preceded by a slash ('/'). The iOS and Mobile Web platform can accept relative and absolute paths, but the Android platform requires an absolute path.
-->

### Android Runtime Error: Uncaught TypeError: Cannot call method xxx of undefined

1. iOS 専用の Titanium オブジェクトを作成しようとしているのかもしれません。View で platform 属性を利用して、platform 特有のオブジェクトを使ってください。
2. View の top-level UI コンポーネントに ID が割り当てられている場合、つまり、XML のそのコンポーネントの ID 属性が設定されている場合、Controller はそのオブジェクトに参照するのに ```$.<controller_name>``` を利用することができません。割り当てた ID を利用してください。

<!--
1. You might be trying to create an iOS-only Titanium object. Use the platform attribute in the view to enforce platform-specific objects.
2. If the top-level UI component in your view has an assigned ID, that is, the id attribute in the XML markup is defined for that component, the controller cannot use $.<controller_name> to reference it. It needs to use the assigned ID.
-->

### Android Runtime Error: Uncaught ReferenceError: Alloy is not defined

Controller でない JavaScript のファイルは Alloy が自動読み込みされません。'alloy' モジュールを require する必要があります。詳細は [Library Code](http://docs.appcelerator.com/titanium/3.0/#!/guide/Alloy_Controllers-section-34636384_AlloyControllers-LibraryCodeandCommonJSModules) を参照してください。

<!--
Non-controller JavaScript files are not automatically wrapped by Alloy. You need to require the 'alloy' module. See Library Code for more information.
-->

### iOS Application Error: invalid method (xxx) passed to UIModule (unknown file).

Android 専用の Titanium オブジェクトを作成しようとしているのかもしれません。View の中で platform 属性を利用して、platform 特有のオブジェクトを使ってください。

<!--
You might be trying to create an Android-only Titanium object. Use the platform attribute in the view to enforce platform-specific objects.
-->

### iOS Application Error: undefined is not an object (evaluating $.xxx.open) (unknown file).

View の top-level UI コンポーネントに ID が割り当てられている場合、つまり、XML のそのコンポーネントの ID 属性が設定されている場合、Controller はそのオブジェクトに参照するのに ```$.<controller_name>``` を利用することができません。割り当てた ID を利用してください。

<!--
If the top-level UI component in your view has an assigned ID, that is, the id attribute in the XML markup is defined for that component, the controller cannot use $.<controller_name> to reference it. It needs to use the assigned ID.
-->

### Mobile Web: Changes to code do not take effect

Mobile Web はコンパイラプラグインをサポートしていないので、Mobile Web アプリケーションは Titanium Stadio でビルドすることができません。Alloy を使って Mobile Web アプリケーションをビルドする場合は、 Alloy の CLI でコードをコンパイルする必要があります。

<!--
Mobile Web applications cannot be built in Studio, since Mobile Web does not support compiler plugins. If you are building a Mobile Web application using Alloy, you need to compile the code using the Alloy command-line interface:
-->

```bash
$ alloy compile --config platform=mobileweb 
```

### Mobile Web: [ERROR] alloy run not supported by mobileweb

Mobile Web アプリケーションは CLI で実行できません。Titanium Stadio でプログラムを実行してください。

<!--
Mobile Web applications cannot run from the command-line interface. Run the program from Studio.
-->

### Studio: Unable to find alloy binary

おそらく Titanium Stadio への Alloy のインストールで問題が発生しているか、 alloy のバイナリに PATH が通っていません。Alloy がインストールされていて、 PATH (通常 '/usr/local/bin' にインストールされます) に存在することを確認して下さい。きちんとインストールされていない場合は、 [Manual Installation instructions](http://docs.appcelerator.com/titanium/3.0/#!/guide/Alloy_Quick_Start-section-34636229_AlloyQuickStart-Command-LineInterfaceInstallation) の指示にしたがってインストールしてください。

<!--
There was probably an issue installing Alloy with Studio or the alloy binary is not in your PATH. Check to see if alloy is installed and is in your PATH (usually in '/usr/local/bin') or follow the Manual Installation instructions to install it.
-->

助けを求める
------------

Titanium コミュニティのメンバーの支援を求めたり、過去に回答された質問から答えを探すには、 [Titanium Community Questions and Answers Forum](http://developer.appcelerator.com/questions/newest) を利用してください。質問をする際は、タグに 'alloy' を入力し、プラットフォーム情報に Alloy のバージョンを入れてください。Alloy のバージョンはコンソールで ```alloy --version``` を実行すれば確認できます。

詳細は [Using Questions and Answers](http://docs.appcelerator.com/titanium/3.0/#!/guide/Using_Questions_and_Answers) と [Getting Help](http://docs.appcelerator.com/titanium/3.0/#!/guide/Getting_Help) を参照してください。

<!--
Use the Titanium Community Questions and Answers Forum to receive assistance from Titanium Community members or find an answer to a previously answered question. Enter 'alloy' as a tag and include the Alloy version as part of the platform information included in the question. To get the Alloy version, run the alloy --version command in a console.

Refer to Using Questions and Answers and Getting Help for more information.
-->

バグレポートを送る
------------------

既存の課題を探したり、バグレポートを送る場合は [JIRA](https://jira.appcelerator.org/secure/Dashboard.jspa) を利用してください。Titanium (Community) プロジェクトで、コンポーネントとして 'Alloy' を選択し、 environment 情報に Alloy のバージョンを入力して JIRA のチケットを作成してください。Alloy のバージョンはコンソールで ```alloy --version``` を実行すれば確認できます。

詳細は [How to Submit a Bug Report](http://docs.appcelerator.com/titanium/3.0/#!/guide/How_to_Submit_a_Bug_Report) を参照してください。

<!--
Use JIRA to search for existing issues or submit a bug report. Select 'Alloy' as the component when submitting a Titanium Community JIRA ticket and include the Alloy version as part of the environment information. To get the Alloy version, run the alloy --version command in a console.

Refer to How to Submit a Bug Report for more information.
-->