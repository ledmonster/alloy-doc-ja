Alloy クイックスタート
======================

本文書は[Alloy Quick Start](http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_Quick_Start)の日本語訳です。

- [概要](#概要)
- [CLI のインストール](#cli-のインストール)
- [プロジェクトを作成する](#プロジェクトを作成する)
- [シンプルな例](#シンプルな例)
	- [View](#view)
	- [Style](#style)
	- [Controller](#controller)
	- [Asset](#asset)
	- [コンパイルと実行](#コンパイルと実行)
- [さらなる例](#さらなる例)
- [次に](#次に)


概要
----

本文書では Alloy のコマンドライン・インタフェース (CLI) のインストール方法と、クイックプロジェクトの作成方法について説明します。

<!-- This guide provides basic instructions on installing the Alloy command-line interface (CLI) and creating a quick project. -->

CLI のインストール
------------------

Alloy の CLI とプラグインは Alloy Studio によって自動的にインストールされます。[Titanium Studio Quick Start](http://docs.appcelerator.com/titanium/latest/#!/guide/Quick_Start)を確認して下さい。もしインストールで問題があったり、Alloy CLI を独立にインストールしたい場合は、下記の手動インストールの説明を参照してください。

<!--
The Alloy command-line interface and plugin should be automatically installed by the studio updater. See Titanium Studio Quick Start for instructions.

If there were installation problems or if you wish to install the Alloy CLI independent of the Studio installation, refer to the manual installation instructions below.
-->

#### 手動インストール

Alloy CLI をインストールする前に Titanium SDK 3.0.x 以上、および Titanium CLI がインストール・設定されている必要があります。[Setting up the Titanium CLI](http://docs.appcelerator.com/titanium/3.0/#!/guide/Setting_up_the_Titanium_CLI) を参照して下さい。

http://nodejs.org/#download から Node.js をダウンロードし、インストールして下さい。Node.js には Alloy をインストールするのに必要な npm パッケージマネージャが含まれています。コンソール画面で、以下のコマンドを実行して Alloy をインストールして下さい。

```bash
$ sudo npm install -g alloy
```

デフォルトでは最新の Alloy がインストールされます。特定のバージョンの Alloy をインストールするには、 `alloy` の後ろに @ マークとバージョン番号をつけてください。たとえば `sudo npm install -g alloy@1.0.0` を実行することで Alloy 1.0.0 をインストールすることができます。

<!--
> Titanium SDK 3.0.x or later, and the Titanium CLI need to be installed and configured before installing the Alloy CLI. See Setting up the Titanium CLI for instructions.

Download and install Node.js from http://nodejs.org/#download, which includes the npm package manager needed to install Alloy.
From a console window, run the following command to install Alloy:

```bash
$ sudo npm install -g alloy
```

By default, these installation directions will install the latest Alloy release. To install a specific released version, use the same directions, except after 'alloy', add the at symbol ('@') with the version number. For instance, executing sudo npm install -g alloy@1.0.0 will install version 1.0.0.
-->

プロジェクトを作成する
----------------------

#### Titanium Studio を利用する

新しい Alloy プロジェクトを作成するには、Titanium Stadio を起動して、

1. メニューから **File > New > Mobile Project** を選択してください。新しく **プロジェクトウィザード** が立ち上がります。
2. **Available Templates** で **Alloy** を選択し、テンプレートを選び、 **Next** ボタンをクリックしてください。
3. 全てのフィールドを埋めて、 **Finish** ボアンをクリックしてください。

新しい Alloy プロジェクトのひな形が生成されます。もしあなたがクラシックな Titanium 開発に慣れているなら、 Resources フォルダが App および Project Explorer から隠されているので注意してください。Alloy プロジェクトに対する全ての操作は app ディレクトリで行います。

<!--
To create a new Alloy project, start Titanium Studio, then

1. From the menu, select File > New > Mobile Project. The New Mobile Project wizard appears.
2. Select Alloy in the Available Templates box, choose a template, then click the Next button.
3. Complete all of the fields, then click the Finish button.

A new skeleton Alloy project will be generated. If you are familiar with classic Titanium development, the Resources folder is hidden from the App and Project Explorer. All work for an Alloy project is done in the app directory.
-->

#### CLI を利用する

新しい Alloy プロジェクトを作成するには、Titanium CLI を用いて Titanium プロジェクトを作成し、 Alloy CLI を用いて Alloy フレームワークコンポーネントを生成してください。

```bash
$ titanium create --name=appname --id=com.domain.appname --platforms=android,ipad,iphone,mobileweb
$ cd appname
$ alloy new
```

Alloy プロジェクトの新しいひな形がアプリ名のディレクトリに生成されます。

<!--
To create a new Alloy project, use the Titanium CLI to create a new Titanium project, then use the Alloy CLI to generate the Alloy framework components:

```bash
$ titanium create --name=appname --id=com.domain.appname --platforms=android,ipad,iphone,mobileweb
$ cd appname
$ alloy new
```

A new skeleton Alloy project will be generated in the appname directory.
-->

シンプルな例
------------

以下の例では Titanium KitchenSink のサンプルアプリの image_view_file.js を Alloy プロジェクトで実装します。 KitchenSink での実装例を見たい場合は、Base UI タブをクリックして、 **Views > Images Views > Image File** をたどってください。

<--
The following example converts the image_view_file.js file from the Titanium KitchenSink sample application to an Alloy project.

To see the example in the KitchenSink application, click on the Base UI tab, then navigate to Views > Images Views > Image File.
-->

### View

View ファイルでは GUI の構造を宣言します。たとえば下記のファイルの場合、画像とラベルの配置されたウィンドウを定義します。
app/views/index.xml の中身を下記の内容で置き換えてください。

<!--
The view file declares the structure of the GUI. For example, the file below defines a window with an image and label.

Replace the contents of app/views/index.xml with the following:
-->

```xml
<Alloy>
	<Window>
		<ImageView id="imageView" onClick="clickImage"/>
		<Label id="l">Click Image of Apple Logo</Label>
	</Window>
</Alloy>
```


### Style

style ファイルでは view のコンポーネントのフォーマットとスタイルを定義します。たとえば下記の style の場合、ウィンドウの背景色と位置、ラベルの色と大きさ、画像の色と位置と大きさを定義しています。
app/styles/index.tss を以下の内容で置き換えてください。

<!--
The style file formats and styles the components in the view file. For example, the style below defines the background color of the window; position, dimensions and color of the label; and position, dimensions and location of the image.

Replace the contents of app/styles/index.tss with the following:
-->

```javascript
"Window": {
	backgroundColor:"white"
},
"#l":{
	bottom:20,
	width: Ti.UI.SIZE,
	height: Ti.UI.SIZE,
	color:'#999'
},
"#imageView":{
	image:"/images/apple_logo.jpg",
	width:24,
	height:24,
	top:100
}
```

### Controller

controller ファイルには、ユーザ入力に応答するプレゼンテーション層のロジックを記述します。たとえば以下のコードの場合、ユーザが画像をクリックした時にアラートダイアログを表示し、アプリケーション開始時にウィンドウを開きます。
app/controllers/index.js を以下の内容で置き換えてください。

<!--
The controller file contains the presentation logic, which responds to input from the user. For example, the controller code below creates an alert dialog when the user clicks on the image and opens the window when the application starts.

Replace the contents of app/controllers/index.js with the following:
-->

```javascript
function clickImage(e) {
	Titanium.UI.createAlertDialog({title:'Image View', message:'You clicked me!'}).show();
}

$.index.open();
```

### Asset

app/assets/images フォルダを作成し、 [apple_logo.jpg](https://raw.github.com/appcelerator-developer-relations/KitchenSink/master/Resources/images/apple_logo.jpg) ファイルを [KitchenSink プロジェクト](https://github.com/appcelerator-developer-relations/KitchenSink) からコピーしてください。このファイルはビルド時に Alloy によって Resources/images/apple_logo.jpg にコピーされます。

<!--
Create a folder called app/assets/images and copy the apple_logo.jpg file from the KitchenSink project. This file will be copied to Resources/images/apple_logo.jpg by Alloy during the build process.
-->

### コンパイルと実行

Alloy CLI は view, style, controller を Titanium プロジェクトに変換し、プロジェクトは Titanium Studio または Titanium CLI によってビルドできるようになります。

<!--
The Alloy CLI converts the view, style and controller in to a Titanium project, which can be built and ran by Studio or the Titanium CLI.
-->

#### Titanium Studio を利用する

**App Explorer** で **Run** ボタンをクリックし、デバイスを選択して、アプリケーションを起動してください。Alloy が Alloy プロジェクトのファイルから Titanium のファイルを生成し、Titanium Stadio はそれをコンパイルしてデバイスシミュレーターで起動します。

> モバイルウェブプラットフォームの場合、 CLI を使ってアプリケーションをコンパイルし、それを Titanium Stadio で実行してください。アプリケーションのコンパイルには、以下のコマンドをコンソールで実行します。
> 
> ```bash
> $ alloy compile --config platform=mobileweb
> ```

<!--
In the App Explorer view, click the Run button and select the device to launch the application. Alloy will generate the Titanium files from the Alloy project files, which will then be compiled by Studio and launched on the device simulator.

> For the Mobile Web platform, compile the application using the CLI, then run it with Studio. Run the following command from a console to compile the application:
> 
> ```bash
> $ alloy compile --config platform=mobileweb
> ```
-->

#### CLI を利用する

コンソールウィンドウでプロジェクトのルートに行って、以下のコマンドを実行してください。

<!--
From a console window, go to the root directory of the project, then
-->

```bash
$ titanium build [-p platform]
```

Titanium CLI には Alloy CLI のフックがあるので、 Alloy コマンドを実行する必要がありません。
Titanium のビルドコマンドの詳細については [Titanium Command-Line Interface Reference](http://docs.appcelerator.com/titanium/3.0/#!/guide/Titanium_Command-Line_Interface_Reference) を参照してください。

> モバイルプラットフォームの場合は、コンパイル後に Titanium Studio でアプリケーションを実行してください。

<!--
The Titanium CLI contains hooks to the Alloy CLI, so you do not need to run any Alloy commands.

Refer to the Titanium Command-Line Interface Reference for more information about using the titanium build command.

> For the Mobile Web platform, run the application from Studio after compiling it.
-->

さらなる例
----------

Alloy のシンプルな利用例をもっと見たい場合は [Alloy の GitHub プロジェクトの alloy/test/app ディレクトリ](https://github.com/appcelerator/alloy/tree/master/test/apps) を参照してください。GitHub プロジェクトにあるサンプルアプリを使うには、まずはじめにプロジェクトを clone します。

<!--
For more simple usage examples of Alloy, go to the alloy/test/app directory on the Alloy GitHub project.

To use the sample applications from the GitHub project, first clone the project:
-->

```bash
$ git clone https://github.com/appcelerator/alloy
```

#### Titanium Studio を利用する

1. 新しい Alloy プロジェクトを作成します。
2. **File > Import** を選択します。
3. **General > File System** を選択して、 **Next** ボタンをクリックすると、 **Import** ダイアログが表示されます。
4. 一番目のブラウズボタン (From directory) をクリックして、clone した Alloy プロジェクトのテストアプリケーションまでいき、 **Open** ボタンをクリックします。たとえば Master-Detail アプリケーションをインポートしたい場合、 alloy/test/apps/advanced/master_detail/ を選択します。
5. 全てのフォルダとファイルを選択します。すべてにチェックマークが付いていることを確認して下さい。
6. 二番目のブラウズボタン (Into folder) をクリックして、あなたのプロジェクトの app ディレクトリまでいき、 **OK** ボタンをクリックしてください。
7. **Finish** ボタンをクリックしてください。サンプルアプリがあなたのプロジェクトにインポートされ、実行できるようになります。

<!--
1. Create a new Alloy project.
2. Select File > Import.
3. Select General > File System and click the Next button. The Import dialog appears.
4. Click the first Browse button (From directory), then navigate to a test application in the cloned Alloy project and click the Open button. For example, if you want to import the Master-Detail application, select alloy/test/apps/advanced/master_detail/.
5. Select all folder and files, that is, make sure they are all checkmarked.
6. Click the second Browse button (Into folder), then navigate to your project's app directory and click the OK button.
7. Click the Finish button. The sample application is imported in your project and you can run it.
-->

#### CLI を利用する

以下のコマンドを実行します。

<!--
Run the following commands:
-->

```bash
$ ti create --name ProjectName
$ cd ProjectName
$ alloy new
$ cp -r <path_to_alloy_git_repo>/test/apps/<path_to_app>/* apps/.
$ ti build
```

次に
----

[Alloy コンセプト](Alloy_Concepts.md)を読んで、 Alloy および、どのようにプロジェクトを構成するかについてより深く学んでください。そこから [Alloy View](Alloy_Views.md), [Alloy Controller](Alloy_Controllers.md), [Alloy Model](Alloy_Models.md) へのリンクに飛んで、 view, controller, model をそれぞれどのように書くかを学びます。

<!--
Review Alloy Concepts to learn more about Alloy and how to structure your project. From there, visit the links on Alloy Views, Alloy Controllers, and Alloy Models to learn how to write views, controllers and models, respectively.
-->