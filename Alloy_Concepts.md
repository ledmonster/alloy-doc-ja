Alloy コンセプト
================

本文書は[Alloy Concepts](http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_Concepts)の日本語訳です。

- [Alloy コンセプト](#alloy-コンセプト)
	- [概要](#概要)
	- [Model View Controller](#model-view-controller)
		- [Alloy: Backbone による MVC](#alloy-backbone-による-mvc)
	- [Alloy と Titanium SDK](#alloy-と-titanium-sdk)
	- [設定より規約](#設定より規約)
		- [プラットフォーム依存の Resource](#プラットフォーム依存の-resource)
	- [Widgets](#widgets)
	- [Builtins](#builtins)
	- [コンパイルプロセス](#コンパイルプロセス)


概要
----

本ガイドでは Alloy フレームワークの重要なコンセプトについて説明します。Model View Controller フレームワークや、Convention over Configuration (設定より規約)設計、widgets、そして Backbone.js と Underscore.js のビルトイン対応について説明します。

<!--
 This guide covers the important concepts related to the Alloy framework, including the model-view-controller framework, convention-over-configuration design, widgets, and built-in support from Backbone.js and Underscore.js.
-->

Model View Controller
---------------------

Alloy は Model View Controller (MVC) のパラダイムを利用して、アプリケーションを3つの異なるコンポーネントに分割します。

- **Model** は、ルールやデータ、アプリケーションの状態を含んだビジネスロジックを提供します。
- **View** は、データを表示やモデルデータ操作のための GUI コンポーネントをユーザに提供します。
- **Controller** は、 Model と View のコンポーネント間をつなぐアプリケーションロジックを提供します。

たとえばカレンダーアプリにおいて、Model にはイベントやリマインダ、招待、連絡先情報が含まれます。View はユーザに対してカレンダーのデータやリマインダを表示したり、ユーザがイベントを登録できるようにしたりします。リマインダにおいて、 Controller は Model のデータをチェックし、 'reminder' View をユーザに対して起動します。イベント追加では、 Controller は 'add event' View を開いて、ユーザがデータを入力したら model にイベント情報を追加します。

MVC の利点は、機能を分離することでコードの再利用ができるようになることえす。たとえば、Controller のコードをほとんど同じにして、 Model のデータを変えないまま、異なるデバイスに対して特別な view を用意することができます。

<!--
Alloy utilizes the model-view-controller (MVC) paradigm, which separates the application into three different components:

- **Models** provide the business logic, containing the rules, data and state of the application.
- **Views** provide the GUI components to the user, either presenting data or allowing the user to interact with the model data.
- **Controllers** provide the glue between the model and view components in the form of application logic.

For example, in a calendar application, the models include events, reminders, invitations and contacts. The views present the calendar data and reminders to the user or allow the user to add events. For reminders, the controller checks the model data and launches a 'reminder' view to the user. For adding events, the controller opens an 'add event' view, then adds the event into the model data once the user entered the data.

An advantage of MVC is the ability to reuse code by separating the functionality. For example, you can have specific views for different devices, while keeping the controller code relatively the same and the model data unchanged.
-->

### Alloy: Backbone による MVC

Backbone.js は元々は WEB アプリケーションのために設計された、軽量な MVC フレームワークです。Alloy の Model は Backbone.js の上に構築されているので、Backbone のリッチな Model とコレクションの API を利用することができます。特別な JSON オブジェクトを吐き出す JavaScript ファイルを使って Model を定義しますが、それは、Model とコレクションをカスタマイズするための Backbone の拡張機能を利用しています。Model の作成方法の詳細は、 [Alloy Model](Alloy_Models.md) を参照してください。また、 Backbone.js の詳細は http://backbonejs.org/ を参照してください。

Alloy の View は Titanium UI コンポーネントをもとに作られています。View は XML で記述され、Titanium Style Sheets (.tss) を用いて装飾します。これらはコンポーネントの作成を抽象化しているので、 Titanium API を直接呼び出す必要がありません。Alloy は View を作成するためのコードを生成します。View の作成方法の詳細は、 [Alloy View](Alloy_Views.md) を参照してください。

Alloy の Controller は一般に、 Alloy の View と1対1に対応しています。Controller は抽象レイヤーを介さずに直接 Titanium SDK API を利用します。Controller はすべての View コンポーネントにアクセスできます。Controller の作成方法の詳細は、 [Alloy Controller](Alloy_Controllers.md) を参照してください。

また、Alloy は Underscore.js をビルトインでサポートしています。 Underscore.js は、配列・イテレーター用のヘルパーなど、一連の有用な機能を提供します。詳細については http://underscorejs.org/ を参照してください。

<!--
Backbone.js is a lightweight MVC framework, originally designed for web applications. Alloy models are built on top of Backbone.js, taking advantage of Backbone's rich Model and Collection APIs. You define models using a Javascript file that exports a special JSON object, which uses Backbone's extend functionality to customize models and collections. See Alloy Models for more information on creating model objects and http://backbonejs.org/ for more information on Backbone.js.

Alloy views are built from Titanium UI components. You define views using XML markup and style them using Alloy Titanium Style Sheets (.tss), which abstracts the creation of these components without using Titanium API calls. Alloy generates the code to create your views. See Alloy Views for more information on creating views.

Alloy controllers generally have a one-to-one relationship with Alloy views. Controllers directly use the Titanium SDK API without an abstraction layer. The controller has access to all of the view components. See Alloy Controllers for more information on creating controllers.

Additionally, Alloy provides built-in support for Underscore.js, which provides a set of utility functions, such as array and iterative helpers. Refer to http://underscorejs.org/ for more information.
-->

Alloy と Titanium SDK
---------------------

Alloy は、Titanium SDK を用いて、XML とスタイルシートを通した UI コンポーネント作成を抽象化しています。Alloy が Titanium SDK の機能を実装していなかったり、機能が UI に関連しなかった場合、あなたは常に Alloy Controller のコードの中で Titanium SDK API を利用したり、機能を実装するために CommonJS モジュールを作成したりすることができます。以下の項目では、Alloy が titanium SDK の何を抽象化しているのかを示します。

画像のような Asset を Alloy で利用する場合、Titanium SDK のマニュアルにある Resources フォルダへの参照は全て、 app/assets フォルダへの参照と置き換えてください。たとえば、 [Icons and Splash Screens](http://docs.appcelerator.com/titanium/3.0/#!/guide/Icons_and_Splash_Screens) のマニュアルではファイルを Resources/android か Resources/iphone フォルダに配置するように書かれています。Alloy では、これらのファイルを app/asset/android か app/asset/iphone フォルダに配置してください。

<!--
Alloy uses Titanium SDK to abstracts the creation of UI components through the use of XML markup and style sheets. If Alloy has not implemented a feature of the Titanium SDK or if the feature is not UI related, you can always use the Titanium SDK API in the Alloy controller code or create a CommonJS module to implement a feature. The table below lists what Alloy directly abstracts from the Titanium SDK.

For assets, such as images, any references to the Resources folder in the Titanium SDK documentation should be replaced with the app/assets folder to use it for Alloy. For example, the Icons and Splash Screens guide tells you to place files in either the Resources/android or Resources/iphone folder. For Alloy, the files should be placed in either the app/asset/android or app/asset/iphone folder.
-->

### 各種パッケージ

#### Titanium SDK

- Titanium.UI.* Objects
- Titanium.Android.Menu
- Titanium.Facebook.LoginButton
- Titanium.Map
- Titanium.Media.VideoPlayer

```JavaScript
Titanium.UI.createButton();
```

#### Alloy

- XML 要素です。名前空間は書かないでください。いくつかの要素については、 ns 属性をつける必要があるかもしれません。詳細は [Alloy_XML_Markup.md#Namespace] を参照してください。

<!--
- XML element. Remove the namespace. For some elements, you may need to assign the ns attribute. For more information, refer to Alloy XML Markup: Namespace.
-->

```xml
<!-- Creates a button -->
<Button />
```

- TSS 要素です。名前空間は書かないでください。オブジェクトが Titanium.UI の他の名前空間に所属していて、 Alloy で名前空間が暗黙に指定されていない場合、要素の名前でスタイルを指定することはできません。代わりに、セレクタ (XML のクラス属性) か id (XML の id 属性を) を使ってスタイルを作成してください。ALloy が暗黙的に名前空間を指定しているオブジェクトの一覧は、[Alloy_XML_Markup.md#Namespace] を参照してください。

<!--
- TSS element. Remove the namespace. If the object belongs to another namespace besides Titanium.UI and is not implicitly assigned a namespace by Alloy, you cannot create a style based on the element name. Instead, create a style based on a selector (XML class attribute) or id (XML id attribute). For a list of objects that Alloy implicitly assigns a namespace, refer to Alloy XML Markup: Namespace.
-->

```JavaScript
// Does not create a button.  Used to style all Button objects in the associated view.
"Button":{
    // Button attributes
}
```

### プロパティ

#### Titanium SDK

Titanium Object properties

```JavaScript
Titanium.UI.createButton({
    text: "Foobar",
    top: 0,
    width: Ti.UI.SIZE
});
```

#### Alloy

- プロパティが文字列や数字、Titanium SDK の定数で表現される場合、XML 要素になります。Titanium のオブジェクトを保持するプロパティの一部は、XML タグを用いてインラインで表現することができます。詳細は [Alloy_XML_Markup.md#Property_Mapping] を参照してください。

<!--
- XML attribute if the property can be expressed as a string, number or Titanium SDK constant. Some properties that take Titanium objects can be expressed inline using XML tags. For more information, refer to Alloy XML Markup: Property Mapping.
-->

```xml
<Button title="Foobar" top="0" width="Ti.UI.SIZE"/> 
```

プロパティが文字列や数字、Titanium SDK の定数、ディレクトリ、配列で表現される場合は TSS の属性になります。ALloy Globals や CFG 名前空間にメソッドや値を定義して、それらに TSS ファイルから間接的に参照することもできます。詳細は [Alloy_Styles_and_Themes] を参照してください。

<!--
- TSS attribute if the property can be directly expressed as a string, number, Titanium SDK constant, dictionary or array. Indirectly, you can assign a method or value to the Alloy Globals or CFG namespace and reference it in the TSS file. For more information, refer to Alloy Styles and Themes.
-->

```JavaScript
"Button":{
    title: "Foobar",
    top: 0,
    width: Ti.UI.SIZE
}
```

### メソッド

#### Titanium SDK

Titanium Object methods

```JavaScript
var button = Titanium.UI.createButton();
button.setTitle('Push Me!');
```

#### Alloy

Controller のコードの中で使います。Controller でオブジェクトを参照できるように XML でオブジェクトの id 属性を定義する必要があります。

<!--
Use in the controller code. You need to define the id attribute of the object in the XML markup, so the object can be referenced in the controller.
-->

```JavaScript
// Need to give the object an ID, for example, <Button id="button" />
$.button.setTitle('Push Me!');
```

### イベント

#### Titanium SDK

Titanium Object events

```JavaScript
var button = Titanium.UI.createButton();
button.addEventListener('click', doClick);
```

#### Alloy

関連する Controller に callback を紐付けるための XML 属性で表現します。イベント名の先頭を大文字にして、 'on' を名前の頭につけてください。

<!--
XML attribute to bind a callback in the associated controller. Capitalize the first character of the event name and append 'on' to the beginning of the name.
-->

```xml
<!-- doClick needs to be declared in the associated controller -->
<Button onClick="doClick"/>
```


設定より規約
------------

開発をシンプルにするために、Alloy は設定ファイルを利用する代わりに、アプリケーションを体系化するためのディレクトリ構造と名付けの規約を利用しています。ALloy は各ファイルが特定の場所に存在する前提で動作します。以下の命名規則に従わないファイルやフォルダはすべて Alloy に無視されます。たとえば生成時に、Alloy は必須の app/views/index.xml と app/controllers/index.js を見に行き、それから、任意の関連ファイル app/styles/index.tss を参照します。Alloy がはじめの画面で使われる View と Controller を Resources/alloy/controllers/index.js に生成するのに、これらのファイルが必要です。

Alloy project で使われるディレクトリとファイルの一覧を下記に示します。

<!--
To simplify development, Alloy uses a directory structure and naming conventions to organize the application rather than configuration files. Alloy expects to find files in specific locations. Any folder or file not adhering to the below naming conventions is ignored by Alloy. For example, at generation time, Alloy will look for the mandatory files app/views/index.xml and app/controllers/index.js, then the optional corresponding file app/styles/index.tss. Alloy requires these files to create the initial view-controller Resources/alloy/controllers/index.js.

The following is a list of directories and files that can be found in an Alloy project:
-->

| パス            | 説明 |
| --------------- | ------------- |
| app             | アプリケーションの Model, View, Controller と assets を格納します。すべての作業はここで行われます。下記の各フォルダの説明を参照してください。 |
| app/alloy.jmk   | ビルド設定のファイルです。 [Build_Configuation_File_(Alloy.jmk).md] を参照。 |
| app/alloy.js    | コンポーネントを事前設定したり、メインの Controller が実行される前に Alloy のメソッドを上書きする際に使われる初期化ファイルです。[Initializer_File_(alloy.js)]を参照。 |
| app/config.json | プロジェクト設定ファイルです。詳細は [Project_Configuration_File_(config.json)] 参照。 |
| app/assets      | 画像の assets や、 Resources ディレクトリにコピーされる必要のあるファイルを格納します。コードからファイルを参照するのに 'app/assets' のパスは不要です。 |
| app/controllers | app/views/filename.xml という view に対応した filename.js というフォーマットで Controller を格納します。詳細は [Alloy_Controllers] を参照。 |
| app/lib         | アプリケーション特有のライブラリを格納します。普通は CommonJS のフォーマットを利用します。詳細は [Library_Code] を参照。 |
| app/migrations  | <DATETIME>_filename.js 形式でデータベースの migration ファイルを格納します。詳細は [Migrations] を参照。 |
| app/models      | filename.js 形式で Model を格納します。詳細は [Alloy_Models.md] 参照 |
| app/styles      | filename.tss 形式で View のスタイルを格納します。このスタイルは app/views/filename.xml に対応した View に適用されます。詳細は [Titanium_Style_Sheets.md] を参照。|
| app/themes      | 全体の GUI の assets やスタイルをカスタマイズするためのテーマを格納します。詳細は [Themes] を参照 |
| app/views       | filename.xml 形式で View を格納します。オプションで app/controllers/filename.js と app/styles/filename.tss 関係します。詳細は [Alloy_XML_Markup.md] 参照。 |
| app/widgets     | Widget ファイルを格納します。各 Widget は app のようなディレクトリ構造を保持します。詳細は [Widgets] 参照。 |
| i18n            | 国際化と地域化のファイルを格納します。Titanium アプリと使い方は同じです。詳細は [Internationalization] 参照。 |
| Resources       | app ディレクトリをもとに Alloy によって生成された Titanium のファイルが格納されます。アプリケーションをビルドすると、すべてのファイルは上書きされます。詳細は [Compilation_Process.md] を参照。|

<!--
| app             | Contains the models, views, controllers and assets of the application. All work should be done here. See folder descriptions below. |
| app/alloy.jmk   | Build configuration file. See Build Configuration File (alloy.jmk). |
| app/alloy.js    | Initializer file used to preconfigure components or override Alloy methods before the main controller is executed. See Initializer File (alloy.js) for more information. |
| app/config.json | Project configuration file. See Project Configuration File (config.json). |
| app/assets      | Contains image assets and other files that need to be copied into the Resources directory. Reference these files in the code without the 'app/assets' path. |
| app/controllers | Contains controllers in the format filename.js to a corresponding view file app/views/filename.xml. See Alloy Controllers for more information. |
| app/lib         | Contains application-specific library code, typically in the CommonJS format. See Library Code for more information. |
| app/migrations  | Contains database migration files in the format <DATETIME>_filename.js. See Migrations for more information. |
| app/models      | Contains model files in the format filename.js. See Alloy Models for more information. |
| app/styles      | Contains view styling in the format filename.tss, which is applied to a corresponding view file app/views/filename.xml. See Titanium Style Sheets for more information. |
| app/themes      | Contains themes to customize the assets and styles of the entire GUI. See Themes for more information. |
| app/views       | Contains views in the format filename.xml with the optional corresponding files app/controllers/filename.js and app/styles/filename.tss. See Alloy XML Markup for more information. |
| app/widgets     | Contains widget files. Each widget will have its own app-like directory structure. See Widgets for more information. |
| i18n            | Contains internationalization and localization files. Same usage as with a Titanium application. See Internationalization for more information. |
| Resources       | Contains the Titanium files generated by the Alloy interface from the app directory. All files will be overwritten each time the application is built. See Compilation Process for more information. |
-->

**Notes:** lib, migrations, themes, widgets フォルダは、新しいプロジェクトを作成した際に自動的には作られません。 migrations と widgets フォルダは、これらのいずれかのコンポーネントが生成された際に Alloy の CLI で作成されます。lib と themes のフォルダは手動で作成する必要があります。

alloy.jmk ファイルは、新しいプロジェクトを作成した際に自動的には作成されません。このファイルを作成するには CLI を利用してください。

<!--
**Notes:** the lib, migrations, themes and widgets folders are not automatically generated when creating a new project. The migrations and widgets folder will be generated by the Alloy command-line interface if any of those components are generated. The lib and themes folders will need to be manually created.

The alloy.jmk file is not automatically generated when creating a new project. Use the command-line interface to generate this file.
-->

### プラットフォーム依存の Resource

Controller や View、スタイルはプラットフォーム依存の Resource を持っているかもしれません。その場合、 android, blackberry, ios, mobileweb または tizen という名前のフォルダをコンポーネントのフォルダの下に作成し、 Android や iOS, モバイルウェブなどのプラットフォーム依存のファイルをそれぞれ追加してください。

たとえば、iOS 特有の view と Andorid 特有の Controller は以下のように配置されます。

<!--
ファイル, views and styles can have platform-specific resources. Just add a folder named android, blackberry, ios, mobileweb, or tizen under the component folder and add your platform specific files for Android, iOS or Mobile Web into the folder, respectively.

For example, an iOS-specific view and an Android-specific controller would be look like this:
-->

- app
  - controllers
    - android
      - index.js
    - index.js
  - views
    - ios
      - index.xml
    - index.xml

代わりに Controller や View やスタイルの中で特別な書式のコードや属性を書いて、プラットフォーム特有のコードやコンポーネントを適用することもできます。詳細は [Controller_Conditional_Code.md] と [View_Conditional_Code.md] を参照してください。

また、assets フォルダは Titanium プロジェクトにある 'Resources' フォルダと同じように、プラットフォーム特有のファイルや解像度依存の画像を配置することができます。詳細は [Platform-Specific Resources] を参照してください。

<!--
Alternatively, you can use special conditional code and attributes inside the controllers, views and styles to apply platform-specific code and components. Refer to Controller Conditional Code and View Conditional Code for more details.

Additionally, the assets folder is laid out the same way as the 'Resources' folder in a Titanium project for platform-specific files and density-specific images. Refer to Platform-Specific Resources for more information.
-->

Widgets
-------

Widgets は Alloy 駆動の Titanium プロジェクトに簡単に組み込める、自己完結したコンポーネントです。コードを複数のアプリケーションで再利用したり、同じアプリケーションの中で何度も利用するために考えだされました。Widgets は専用の Model や View や Controller, スタイル、assets を保持し、app ディレクトリと同じ構成で Alloyo プロジェクト内に配置されます。

プロジェクトで Widgets を利用する方法の詳細は [Importing Widgets] を参照してください。

Widgets の作成方法の詳細は [Creating Widgets] を参照してください.

利用可能な Widgets の詳細は [Alloy API documentation](http://docs.appcelerator.com/titanium/3.0/#!/api/Alloy.widgets) と [Alloy github repository](https://github.com/appcelerator/alloy/tree/master/widgets) を参照してください。

<!--
Widgets are self-contained components that can be easily dropped into Alloy-powered Titanium projects. They were conceived as a way to reuse code in multiple applications or to be used multiple times in the same application. Widgets have their own models, views, controllers, styles and assets and are laid out the same as the app directory in the Alloy project.

For information on using widgets in a project, see Importing Widgets.

For information on creating widgets, see Creating Widgets.

For information on available widgets, see the Alloy API documentation and Alloy github repository.
-->

Builtins
--------

Alloy には、アニメーションや文字列操作、表示単位の変換といった単純な特定の機能のための追加のユーティリティがあります。これらのユーティリティを 'builtins' を呼びます。これらのユーティリティを利用するには、 Controller で 'alloy' を root ディレクトリに指定して require する必要があります。たとえば、'shake' ボタンをクリックすることで現在の View を振動させるのにアニメーションの機能を利用する場合、以下のコードを Controller に追加します。

<!--
Alloy comes with additional utilities used to simplify certain functions, such as animations, string manipulation, and display unit conversion. These utilities are referred to as 'builtins.' To use these utilities, the controller needs to call require with 'alloy' as the root directory. For example, to use an animation function to shake the current view by pressing the 'shake' button, add this code to a controller:
-->

```JavaScript
var animation = require('alloy/animation');
$.shake.addEventListener('click', function(e) {
	animation.shake($.view);
});
```

Builtin の機能や利用方法の詳細は [Alloy builtins API documentation] を参照してください。

<!--
See Alloy builtins API documentation for the functionality and usage of builtins.
-->

コンパイルプロセス
------------------

本項では、 Alloy の CLI がどのように app フォルダのファイルを Titanium プロジェクトの Resources フォルダにコンパイルするかについて、簡単な概要を説明します。

<!--
This section provides a brief overview of how the Alloy command-line interface converts the files in the app folder to a Titanium project in the Resources folder.
-->

#### Cleanup

前回ビルドされた Resources フォルダの内容は cleanup されます。プラットフォーム特有のフォルダが Resources フォルダに存在して、 app/assets フォルダには存在しない場合、そのディレクトリと中身は削除されません。

<!--
The Resources folder is cleaned of any previous built files. If a platform-specific folder exists in the Resources folder, but not in the app/assets folder, it and its contents are not removed.
-->

#### Alloy フレームワーク, Assets, Lib

Backbone.js, Underscore.js のようなライブラリや同期アダプタ、基底クラスといった Alloy フレームワークのファイルは Resources/alloy フォルダにコピーされます。Alloy ベースのライブラリである alloy.js は Resources フォルダにコピーされます。これらのファイルはすべての Alloy プロジェクトで必要です。

プロジェクトの設定ファイルである config.json は処理されて Resources/alloy/CFG.js にコピーされます。

assets フォルダと lib フォルダのファイルは、theme の assets フォルダに存在するものも含め、 Resources フォルダにコピーされます。

<!--
Alloy framework files, which include the Backbone.js and Underscore.js libraries, sync adapters and base controller class, are copied to the Resources/alloy folder. The Alloy base library, alloy.js, is copied to the Resources folder. These files are necessary to run any Alloy project.

The project configuration file, config.json, is processed and copied to Resources/alloy/CFG.js.

The files in the assets and lib folders, as well as the files in the theme's assets folder, are copied to the Resources folder.
-->

#### ビルド設定

ビルド設定ファイル alloy.jmk が存在する場合、ロードされます。'pre:compile' タスクが定義されていた場合、ここで実行されます。

<!--
If the build configuration file, alloy.jmk, exists, it is loaded. The 'pre:compile' task executes at this point if it is defined.
-->

#### Model View Controller と Widget の生成

Model のファイルが処理されます。コンパイラは Model ごとに JavaScript のファイルを生成し、それらを Resources/alloy/models フォルダにコピーします。

Widget のファイルが処理されます。コンパイラは Widget ごとにフォルダを生成し、View と Controller に応じた JavaScript のファイルを生成して、Resources/alloy/widgets フォルダにコピーします。

スタイル、 View, Controller のファイルが、Theme のスタイルフォルダにあるものや app.tss のグローバルスタイルファイルを含め、処理されます。コンパイラは View, Controller ごとに JavaScript のファイルを生成し、それらを Resources/alloy/controllers フォルダにコピーします。

<!--
The model files are processed. The compiler creates a JavaScript file per model and copies them to the Resources/alloy/models folder.

The widget files are processed. The compiler creates a folder per widget that contains JavaScript files per view-controller and copies them to the Resources/alloy/widgets folder.

The style, view and controller files, as well as the files in the theme's style folder and the app.tss global style file, are processed. The compiler creates a JavaScript file per view-controller and copies them to the Resources/alloy/controllers folder.
-->

#### メインアプリケーション

Alloy はテンプレートから app.js のひな形を作成します。このファイルの中身はいくつかの Alloy モジュールを要求し、メインの View Controller の index.js を呼び出します。初期化ファイル alloy.js が存在する場合、appl.js ファイルのメインの View Controller の初期化処理の呼び出しの直前に、ファイルの中身全体がコピーされます。

ビルド設定ファイルに 'compile:app.js' 設定が定義されていた場合、ファイルが Resources ディレクトリに書きだされる前に、そのタスクが実行されます。

<!--
Alloy creates a skeleton app.js file from a template. The contents of this file require some Alloy modules and calls the main view-controller index.js. If an initializer file, alloy.js, exists, the entire contents of the file are copied into the app.js file right before the call to initiate the main view-controller.

Before the file is written to the Resources directory, the 'compile:app.js' task executes if one is defined in the build configuration file.
-->

#### コードの最適化

高速・コンパクトに最適化するために、生成されたコードは UglifyJS で処理されます。オプションで、コードを美しくすることもできます。

コードが特定のプラットフォーム用にコンパイルされた場合、そのプラットフォームで実行されない環境依存のコードはすべての削除されます。たとえば、アプリケーションが iOS 特有のコードを含んでいて、アプリケーションを Android 用にコンパイルする場合、 iOS 特有のコードはすべて削除されます。

Require された Alloy の builtin ライブラリは Resources/alloy フォルダにコピーされ、上記と同じプロセスで最適化されます。

それから、ビルド設定ファイルで定義されていた場合、 'post:compile' タスクが実行されます。

<!--
The generated code is processed through UglifyJS to optimize the code for speed and compactness. The code is optionally beautified.

If the code is compiled for a specific platform, all conditional code that should not be executed for that platform is removed. For example, if the application contains code sections specifically for iOS but the application is compiled for an Android platform, all of the iOS conditional code is removed.

Required Alloy builtin libraries are copied to the Resources/alloy folder and optimized in the same process as described before.

Then, the 'post:compile' task executes if one is defined in the build configuration file.
-->