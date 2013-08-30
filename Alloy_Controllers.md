Alloy Controller
================

本文書は[Alloy Controllers](http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_Controllers)の日本語訳です。

- [概要](#概要)
- [Controller](#controller)
  - [継承](#継承)
  - [環境依存のコード](#環境依存のコード)
  - [引数を渡す](#引数を渡す)
  - [Global Namespace](#global-namespace)
- [初期化ファイル (alloy.js)](#初期化ファイル-alloyjs)
- [ライブラリと CommonJS モジュール](#ライブラリと-commonjs-モジュール)
  - [Alloy, Underscore.js, Backbone.js を拡張する](#alloy-underscorejs-backbonejs-を拡張する)



概要
----

本トピックでは、 Controller 及び、 Model 以外の JavaScript のコードをどのように書くかについて説明します。Alloy の Controller は UI オブジェクトを操作するために Titanium SDK API を直接呼び出し、 UI 以外の API も呼び出すので、ある程度、従来の Titanium 開発に関する知識が必要になってきます。詳細は [Titanium API Guides](http://docs.appcelerator.com/titanium/3.0/#!/api) を参照してください。

<!--
This topic covers how to write controller code as well as other JavaScript files excluding models. Some traditional Titanium development is required, since Alloy controllers make direct calls to the Titanium SDK API to manipulate UI objects and access non-UI APIs. Refer to the Titanium API Guides for more information.
-->

Controller
----------

Alloy では、 Controller は UI を操作したり Model とやり取りするアプリケーションロジックを含みます。以下のコードは view (index.xml) に紐付いたプレゼンテーション層のロジック (index.js) です。

<!--
In Alloy, controllers contain the application logic used to control the UI and communicate with the model. The following code contains the presentation logic (index.js) associated with the view (index.xml).
-->

**app/controllers/index.js**

```JavaScript
function doClick(e) {
    alert($.label.text);
}

$.index.open();
```

**app/views/index.xml**

```xml
<Alloy>
    <Window class="container">
        <Label id="label" onClick="doClick">Hello, World</Label>
    </Window>
</Alloy>
```

View 内の ID 属性を持つ UI 要素は、全て自動的に定義され、 $ という特別な変数が前についたプロパティで Controller から取得することができます。$ は Controller への参照です。たとえば、 Controller 内の $.label という prefix は、View の Ti.UI.Label インスタンスにアクセスするために使われます。この参照は、オブジェクトのプロパティやメソッドに直接アクセスするために使われます。たとえば $.label.hide() を呼ぶことで、 View から label を隠したり、 $.label.text を使うことで label の text を変えることができます。

外部の Controller や View にアクセスするには、それぞれ Alloy.createController と Controller.getView メソッドを利用してください。詳細は [Alloy API documentation](http://docs.appcelerator.com/titanium/3.0/#!/api/Alloy) を参照してください。

最上位の UI オブジェクトに ID が定義されていない場合、$. に Controller の名前をつけて参照してください。View の Window オブジェクトは ID を持っていないので、Controller は View から最上位の UI オブジェクトを取得するのに $.index を使います。しかし、たとえば ```<Window id='window'>``` というように id 属性が定義されていた場合、Controller は Window オブジェクトにアクセスするのに $.window を利用する必要があります。$.index は undefined となり、 $.index.open() を呼ぼうとするとアプリケーションがエラーを出します。

<!--
All UI elements which have an id attribute in a view are automatically defined and available as a property prefixed by the special variable $ in the controller. The $ is a reference to the controller. For example, the $.label prefix in the controller is used to access the Ti.UI.Label object instance in the view. This reference is used to directly access properties or methods of this object. For example, calling $.label.hide() hides the label from the view or you can change the label text with $.label.text.

To access external controllers and views, use the Alloy.createController and Controller.getView methods, respectively. See the Alloy API documentation for more details.

If the top-level UI object does not have an ID defined, reference it using the name of the controller prefixed by the $. Since the Window object in the view does not contain an ID, the controller uses $.index to grab the top-level UI object from the view. However, if an id attribute was defined, for example, <Window id='window'>, the controller needs to use $.window to gain access to the Window object; $.index will be undefined and the application will throw an error when calling $.index.open().
-->

### 継承

Controller は base (parent) Controller をひもづけることで、 exports.baseController = 'baseControllerName' というように、他の Controller を継承することができます。 CommonJS model と同様、 Controller は継承元の Controller の exported な関数をすべて継承します。これらの関数は上書きすることも可能です。

たとえば、 animal の View と Controller は speak メソッドつきで label オブジェクトを定義しています。

<!--
Controllers can inherit from other controllers by assigning it a base (parent) controller: exports.baseController = 'baseControllerName'. As in the CommonJS model, the controller inherits any exported functions from the base controller. These functions can also be overwritten.

For example, the animal view-controller defines a label object with a speak method:
-->

**app/controllers/animal.js**

```JavaScript
exports.speak = function() {
    alert("Yelp!");
};
```

**app/views/animal.xml**

```xml
<Alloy>
    <Label id="animalLabel">Animal</Label>
</Alloy>
```

そして、下記のコードは animal の View, Controller を継承して speak メソッドと label のテキスト属性を上書きし、 dog Controller 用にカスタマイズしています。

<!--
Then, the following code inherits from the animal view-controller and override the speak method and label text property to customize it for a dog controller.
-->

**app/controllers/dog.js**

```JavaScript
exports.baseController = "animal";
$.animalLabel.text = "Dog";
exports.speak = function() {
    alert("Bark!");
};
```

### 環境依存のコード

Alloy はコンパイラへの指示命令のように振る舞う特別な変数のセットを持っています。これらのコンパイラ定数はコードの生成とコンパイル時の最適化に利用され、使われないコードは全て削除されます。

<!--
Alloy introduces a set of special variables that act like compiler directives. Using these compiler constants optimizes the code at generation/compilation and any non-reachable code is removed.
-->

Controller で利用するために Alloy で定義された定数の一覧は下記のとおりです。

* OS_ANDROID : 現在のコンパイラのターゲットが Android の場合 true です。
* OS_BLACKBERRY: 現在のコンパイラのターゲットが BlackBerry の場合 true です。
* OS_IOS : 現在のコンパイラのターゲットが iOS の場合 true です。
* OS_MOBILEWEB : 現在のコンパイラのターゲットが Mobile Web の場合 true です。
* OS_TIZEN : 現在のコンパイラのターゲットが Tizen の場合 true です。
* ENV_DEV : 現在のコンパイラのターゲットが(シミュレータまたはエミュレータで動作する)開発用ビルドの場合 true です。
* ENV_TEST : 現在のコンパイラのターゲットがデバイスのテスト用のビルドの場合 true です。
* ENV_PRODUCTION : 現在のコンパイラのターゲットが(パッケージのインストール後に動作する)本番用のビルドの場合 true です。

<!--
The following are the constants defined by Alloy for use in the controller code:

* OS_ANDROID : true if the current compiler target is Android
* OS_BLACKBERRY: true if the current compiler target is BlackBerry
* OS_IOS : true if the current compiler target is iOS
* OS_MOBILEWEB : true if the current compiler target is Mobile Web
* OS_TIZEN : true if the current compiler target is Tizen
* ENV_DEV : true if the current compiler target is built for development (running in the simulator or emulator)
* ENV_TEST : true if the current compiler target is built for testing on a device
* ENV_PRODUCTION : true if the current compiler target is built for production (running after a packaged installation)
-->

たとえば iOS のデバイスはバックボタンを持たないため、アプリケーションは window Controller に対して条件付きのコードを追加する場合があります。

<!--
For example, since iOS devices do not include a back button, the application can conditionally add one to a window controller:
-->

```JavaScript
if (OS_IOS)
{
    var closeButton = Ti.UI.createButton({
        title: 'Close',
        style: Ti.UI.iPhone.SystemButtonStyle.PLAIN
    });

    closeButton.addEventListener('click', function(){
        $.window.close();
    });

    $.window.leftNavButton = closeButton;
}
```

### 引数を渡す

外部の Controller を初期化する際にカスタマイズ用の引数を渡すことができます。たとえば、以下のようになります。

```JavaScript
var controller = Alloy.createController('controller', {args1: 'foo'})
```

外部の Controller において、 *arguments[0]* は引数を受けとるための特別な変数です。たとえば、 TalbeView オブジェクトに複数の TableViewRow オブジェクトを追加したい場合を考えてみてください。

TableViewRow オブジェクト('row')において、 View はそのオブジェクトだけを保持し、 Controller は引数を処理するためのった数行のコードからなります。

<!--
When initializing an external controller, you can pass arguments to customize it, for instance, var controller = Alloy.createController('controller', {args1: 'foo'}). In the external controller, the special variable arguments[0] is used to receive the arguments. For example, suppose you want to add multiple TableViewRow objects to a TableView object.

For the TableViewRow object, called 'row', the view contains only the object, and the controller contains only a few lines of code to parse the arguments:
-->

**app/views/row.xml**

```xml
<Alloy>
    <TableViewRow id="rowView"/>
</Alloy>
```

**app/controllers/row.js**

```JavaScript
var args = arguments[0] || {};
$.rowView.title = args.title || '';
$.rowView.url = args.url || '';
```

別の Controller では TableView オブジェクト('tableView')を保持し、データ配列を繰り返し処理で、新しい 'row' オブジェクトを生成して 'tableView' に追加する処理が書かれています。

<!--
In a separate controller containing the TableView object, called 'tableView', the code is cycling through an array of data and creating new instances of 'row' to supply it to 'tableView.'
-->

**app/controllers/index.js**

```JavaScript
var data[];
for (var i=0; i<source.length; i++) {    
    var arg = {
        title: source[i].postTitle,
        url: source[i].postLink
    };
    var row = Alloy.createController('row', arg).getView();
    data.push(row);
}
$.tableView.setData(data);
```

上記の例に見られるように、 Controller は 'row' Controller に異なる引数を渡し、 'row' の unique なインスタンスを生成しています。

<!--
As seen in the example above, the controller is passing different arguments to the 'row' controller, creating unique instances of 'row'.
-->

### Global Namespace

Controller は Alloy.Globals namespace を利用してグローバルな変数を格納し、アクセスすることができます。たとえば親 Window のインスタンスを Globals に格納しておいて、別の Window からその Window にアクセスすることができます。

親 Window を格納する:

<!--
Controllers can store and access global variables using the Alloy.Globals namespace. For example, you can store an instance of a parent window in Globals and access it in another window.

Store the parent window:
-->

```JavaScript
$.index.open();
Alloy.Globals.parent = $.index;
```

別の Controller からその親 Window にアクセスする:
<!--
Access the parent window in another controller:
-->

```JavaScript
var parent = Alloy.Globals.parent;
parent.close();
```

その他の Controller でない JavaScript のコードもグローバル変数にアクセスすることができますが、 Alloy module を require する必要があります。詳しくは下記の [Alloy を拡張する](#alloy-underscorejs-backbonejs-を拡張する) を参照してください。

<!--
Other non-controller JavaScript code can access the globals variable but need to require the Alloy module. See Extending Alloy below.
-->

初期化ファイル (alloy.js)
-------------------------

初期化ファイル app/alloy.js はアプリケーションのライフサイクルの始めの方でコードを実行するのに使います。このファイルの内容は始めに起動される index.js Controller が実行される直前に実行されるので、UI コンポーネントが読み込まれる前にコードを実行したり、実行される前に Alloy の組み込み関数を上書きすることができます。このファイルのコードは Alloy namespace にアクセスすることもできます。

たとえば、デフォルトの isTablet メソッドは iPad や、large または extra large グループの Android デバイス、縦横いずれかが 400 dp 以上の Mobile Web Application に対して true を返しますが、この振る舞いを上書きして、 alloy.js に以下のようなコードを追加することができます。

<!--
The initializer file app/alloy.js can be used to execute some code near the beginning of the application's lifecycle. The contents of this file will be executed right before the initial index.js controller is loaded, allowing you to execute code before any UI components are loaded and to override builtin Alloy functions before they are executed. The code in this file also has access to the Alloy namespace.

For instance, the default isTablet method returns true if it is identified as an iPad, an Android device in the large or extra large group, or if either dimension exceeds 400 dp for Mobile Web application. To override that behavior, you can add the following code to alloy.js.
-->

```JavaScript
Alloy.isTablet = function(){
    return !(Math.min(Ti.Platform.displayCaps.platformHeight, Ti.Platform.displayCaps.platformWidth) < 600);
}
```

ライブラリと CommonJS モジュール
--------------------------------

Controller のコードとしては適さない JavaScript のコードがあるかもしれません。関連する View がなかったり、再利用性を高めるために MVC フレームワークから分離したかったり。その場合、 Alloy プロジェクトの app ディレクトリに lib というフォルダを作成してください。そして CommonJS モジュールや、 CommonJS の書式に従った JavaScript のコードを lib フォルダに入れてください。これらのファイルは Alloy プロジェクトのコンパイル時に Resources フォルダにコピーされます。

このライブラリや CommonJS モジュールを利用するには、ライブラリ名やモジュール名を 'app/lib' のパスと '.js' 拡張子抜きで require してください。

<!--
Some JavaScript code might not be suitable as controller code, since it does not have an associated view, or you want to separate it from the MVC framework for easier reusability. Create a folder called lib in the app directory of your Alloy project. Add your CommonJS modules or JavaScript code using the CommonJS format into the lib folder. These files are copied to the Resources folder, when compiling your Alloy project.

To use the library or CommonJS module, require it with the library name or module name without the 'app/lib' path and '.js' extension:
-->

```JavaScript
var lib = require('library_name');
lib.foo();
```

> Titanium と Alloy は Node.js のコンセプトである "folders as modules" をサポートしていないので、フォルダ名を require しても自動的にフォルダ内の index.js や index.json を読み込まないし、メインの entry point を指定するために package.json ファイルを使ったりしません。ライブラリへのメインの entry point となるファイルを明示的に require する必要があります。

<!--
> Titanium and Alloy do not support the Node.js concept of "folders as modules," that is, requiring a folder name does not automatically load the index.js or index.json file inside the folder or use the package.json file to locate the main entry point. You need to explicitly require the file that serves as the main entry point to the library.
-->

### Alloy, Underscore.js, Backbone.js を拡張する

createController や createModel といった Alloy API のメソッドや、CommonJS modules にある Underscore.js や Backbone.js 、 app/lib にある JavaScript のファイルにアクセスするには、ライブラリの中のそれらの module を読み込む必要があります。

<!--
To access the Alloy API methods, such as createController and createModel, as well as Underscore.js and Backbone.js in CommonJS modules and JavaScript files in app/lib, you need to load those modules in to the library:
-->

```JavaScript
var Alloy = require('alloy'), _ = require("alloy/underscore")._, Backbone = require("alloy/backbone");

// Alloy extended
var foo = Alloy.createController('foo').getView();
foo.open();

// Underscore extended
var even = _.find([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0; });
Ti.API.info(even);

// Backbone extended
var Book = Backbone.Model.extend();
var book = new Book({title: 'Ulysses', author: 'James Joyce'});
Ti.API.info(JSON.stringify(book));
```

現在、これらのモジュールは自動的に Global スコープで使えるようになっていて、モジュールを読み込まなくてもこれらの API を使えますが、 require メソッドでこれらのモジュールを読み込む前にモジュールを参照するのは非推奨で、この振る舞いは将来廃止される可能性があります。将来のリリースと互換性を持たせるには、常に require メソッドを用いてこれらのモジュールを読み込んでから、利用するようにしてください。

<!--
Currently, these modules are automatically available in the global scope and these APIs can be used without loading the modules. Referencing these modules without loading them first with the require method is discouraged and this behavior may be deprecated in the future. To ensure compatibility with future releases, always use the require method to load and use these modules.
-->
