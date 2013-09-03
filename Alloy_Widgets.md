Alloy Widget
============

本文書は[Alloy Widgets](http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_Widgets)の日本語訳です。

- [Alloy Widget](#alloy-widget)
  - [はじめに](#はじめに)
  - [Widget を使う](#widget-を使う)
  - [Widget を作る](#widget-を作る)

はじめに
--------

Widget は Alloy で実装された Titanium プロジェクトに簡単に追加できる自己完結型のコンポーネントです。Widget は、複数のアプリケーションでコードを再利用したり、同一アプリケーション内で複数回利用するための方法として、考え出されました。Widget は独自の View や Controller, style, asset を保持しており、 Alloy プロジェクトの app ディレクトリと同じように配置されています。

<!--
Widgets are self-contained components that can be easily dropped into Alloy-powered Titanium projects. They were conceived as a way to reuse code in multiple applications or to be used multiple times in the same application. Widgets have their own views, controllers, styles and assets and are laid out the same as the app directory in the Alloy project.
-->

Widget を使う
-------------

プロジェクトで Widget を使う方法については [Importing Widgets] を参照してください。

<!--
See Importing Widgets for more information on using widgets in your project.
-->

Widget を作る
-------------

Widget は Alloy プロジェクトの app/widgets/ ディレクトリの中の個別のディレクトリに作成される必要があります。Widget はそれぞれ独自の View や Controller, Model, style, asset を保持しており、Alloy プロジェクトの app ディレクトリと同じように配置されています。配置については [設定より規約](Alloy_Concepts.md#設定より規約) を参照してください。また、Widget は自己完結しているので、i18n フォルダに配置される国際化・地域化ファイルを除いて、パスに含まれるコードや asset 以外を参照すべきではありません。Alloy プロジェクトと異なる点や、役立つ知見について、以下の項目で説明します。

<!--
Widgets should be built in their own directory in the Alloy project's app/widgets/ directory. Widgets have their own views, controllers, models, styles and assets and are laid out the same as the app directory in the Alloy project. See Convention over Configuration for the layout. Additionally, since widgets are self-contained, they should not reference any code or assets not within its path, except for internationalization and localization files, which are located in the i18n folder. Some differences from Alloy projects and helpful tricks are noted below.
-->

### Asset とライブラリ

assets フォルダや libs フォルダにあるファイルについては、Widget のルートフォルダからの相対パスを指定するのに、Alloy プロジェクトの app ディレクトリとは違って、 WPATH() マクロを利用してください。たとえば app/widgets/foo/lib/helper.js にライブラリがある場合、それを require するには ```require(WPATH('helper'));``` を使い、また、app/widgets/foo/assets/images/foo.png に画像がある場合、それを参照するには ```WPATH('images/foo.png') を使ってください。

<!--
For any files in the assets or libs folder, use the WPATH() macro to automatically map the path relative from the widget's root folder, as opposed to the Alloy project's app directory. For example, if you have a library located at app/widgets/foo/lib/helper.js, to require it, use require(WPATH('helper')); and if you have an image located at app/widgets/foo/assets/images/foo.png, to reference it, use WPATH('images/foo.png').
-->

### 設定

Widget はそれぞれ、 widget.json と呼ばれる設定ファイル設定ファイルを Widget のルートディレクトリに持っています。このファイルのフォーマットについては [Widget 設定ファイル (widget.json)](Alloy_Reference_Guides/Widget_Configuration_File.md) を参照してください。

<!--
Widgets have their own configuration file called widget.json located in the widget's root directory. Refer to Widget Configuration File (widget.json) for the format of the file.
-->

### Controller

メインの Controller は index.js ではなく widget.js です。

widget.js/widget.xml 以外の View Controller を使う場合には、Controller の新しいインスタンスを生成するのには ```Widget.createController(controller_name, [params])``` メソッドを利用し、Titanium proxy オブジェクトにアクセスするには ```getView()``` メソッドを利用してください。たとえば、 Button View Controller を foo Widget で持っていたとします。これをメインの Widget View に追加するには、以下のようにしてください。

<!--
The main controller is called widget.js instead of index.js.

To use another view-controller besides widget.js/widget.xml, use the Widget.createController(controller_name, [params]) method to create a new instance of the controller and the getView() method to access the Titanium proxy object. For example, suppose you have a button view-controller in your foo widget. To add it to the main widget view:
-->

**app/widgets/foo/controllers/widget.js**

```JavaScript
var button = Widget.createController('button').getView();
$.widget.add(button);
```

Widget Controller にあるメソッドは、 $ の prefix を付けて Alloy プロジェクトや他の Widget からアクセスできるようにしない限り、すべて private です。たとえば、Widget の Controller で以下のコードが定義されていた場合、

<!--
All methods in the widget controller are private unless you prefix the method with $, which makes it accessible to the Alloy project and other widgets. For example, if the following code was defined in a widget controller:
-->

```JavaScript
$.init = function (args) {
    // Button object with id=button
    $.button.title = args.title || 'Si';
    $.button.color = args.color || 'black';
    // global variable
    message = args.message || 'Hola mundo';
}
```

Alloy プロジェクトの中で、 Alloy プロジェクトの View (訳注: Widget の間違い！？)で指定された Widget ID を前につけて、 init を呼び出してください。この例では、その Widget ID は foo となります。

<!--
Then, in the Alloy project, call init prefixed with the widget ID specified in the Alloy project's view--in this example, the ID is foo:
-->

```JavaScript
$.foo.init({title:'Yes', color:'gray', message:'I pity the foo.'});
```

### Models

```Widget.createModel(model_name, [params])``` や ```Widget.createCollection(model_name, [params])``` メソッドを使って Widget の Controller 内で Model や Collection を作成する点を除き、一般の Alloy プロジェクトと同じように Model を利用してください。 Widget の View で *Model* や *Collection* のタグを利用することもできます。

<!--
Use models the same way as with a regular Alloy project except to create a model or collection inside a widget controller, use the Widget.createModel(model_name, [params]) and Widget.createCollection(model_name, [params]) methods, respectively. You can also use the Model and Collection tags in widget views.
-->

### Styles

メインの *TSS* ファイルとして、 *index.tss* の代わりに *widget.tss* を利用します。

<!--
The main TSS file is called widget.tss instead of index.tss.
-->

### Views

メインの View は index.xml の代わりに widget.xml と呼ばれます。

XML markup コンポーネントの中で id 属性を指定することで、 Titanium オブジェクトのプロパティにアクセスしたり、上書きしたりするのが簡単になります。たとえば、Widget View の中に button という id のついた Button オブジェクトがあり、その Alloy プロジェクトで Widget の id が *foo* だった場合、Button プロパティにアクセスするには下記のようにします。

<!--
The main view is called widget.xml instead of index.xml.

Specifying the id attribute in the XML markup components will make it easier to access and override Titanium object properties. For example, suppose you have a Button object in a widget view that has its id assigned as button and in the Alloy project the widget id is foo. To access a Button property:
-->

```JavaScript
Ti.API.info("button state: " + $.foo.button.enabled);
```

複数の View Controller を持つ Widget の場合、 *Widget* タグを使って View Controller から拡張子を外した名前を *name* 属性で指定してください。たとえば、以下の markup は Controller の節での例と似ています。

<!--
For widgets that have multiple view-controllers, use the Widget tag and assign the name attribute with the name of the view-controller minus the file extension. For example, the following markup is analogous to the example in the Controller section:
-->

**app/widgets/foo/views/widget.xml**

```xml
<Alloy>
    <View>
        <Widget src="foo" name="button"/>
    </View>
</Alloy>
```

### Widgets

Widget は他の Widget を含むこともできます。Widget では *config.json* の代わりに *widget.json* を設定ファイルとして利用する点を除き、 [Importing Widgets] の指示に従ってください。また、 Widget の Controller 内で別の Widget を作るには、 ```Widget.createWidget(widget_name, [controller_name], [params])``` メソッドを利用してください。

<!--
Widgets can also contain other widgets. Follow the same directions from Importing Widgets except the widget's configuration file is called widget.json instead of config.json. Additionally, to create a widget inside a widget controller, use the Widget.createWidget(widget_name, [controller_name], [params]) method.
-->