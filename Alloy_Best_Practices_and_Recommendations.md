Alloy ベストプラクティス・推奨手法
==================================

本文書は[Alloy Best Practices and Recommendations](http://docs.appcelerator.com/titanium/3.0/#!/guide/Alloy_Best_Practices_and_Recommendations)の日本語訳です。

- [Titanium から Alloy への移行ガイド](#titanium-から-alloy-への移行ガイド)
	- [Titanium アプリを作る際には起動時にライブラリを読み込んでいました。私のパターンを MVC に当てはめたとき、アプリケーションの機能は深く Controller の名前空間の中に配置した方が良いですか？](#titanium-アプリを作る際には起動時にライブラリを読み込んでいました。私のパターンを-mvc-に当てはめたとき、アプリケーションの機能は深く-controller-の名前空間の中に配置した方が良いですか？)
	- [Controller のためのパフォーマンス上のベストプラクティスってありますか？](#controller-のためのパフォーマンス上のベストプラクティスってありますか？)
	- [view の紐付いたたくさんの callback を書いた Controller 達を単に読みこむだけにして、 Alloy に繋がりの管理を任せても構いませんか？](#view-の紐付いたたくさんの-callback-を書いた-controller-達を単に読みこむだけにして、-alloy-に繋がりの管理を任せても構いませんか？)
	- [標準的な Titanium アプリケーションに対してやったように、私の organization で Alloy を支援するのに採用すべきベストプラクティスはありますか？](#標準的な-titanium-アプリケーションに対してやったように、私の-organization-で-alloy-を支援するのに採用すべきベストプラクティスはありますか？)
- [コーディング規約のベストプラクティス](#コーディング規約のベストプラクティス)
	- [命名規則](#命名規則)
	- [グローバル変数](#グローバル変数)



本ガイドでは、Alloy アプリケーションを書く上での推奨事項を述べます。本ガイドは、既にある [Titanium SDK ベストプラクティス・推奨手法](http://docs.appcelerator.com/titanium/3.0/#!/guide/Best_Practices_and_Recommendations) を補完するもので、特に [Coding Best Practices]() と [Style and Conventions]() に一番の焦点を当てています。

<!--
This guide provides recommendations for writing Alloy applications. This guide supplements the existing Titanium SDK Best Practices and Recommendations guide with a primary focus on the Coding Best Practices and Style and Conventions pages.
-->

Titanium から Alloy への移行ガイド
----------------------------------

<!--
### In my Titanium application, I previously loaded libraries upon startup. In organizing my patterns with MVC, do I need to organize application functionality further under namespaces within controllers?

Not unless you want to. If you have a pre-existing set of functionality that you want to make available globally throughout your app, there is no reason to further sub-categorize it unless it serves you in terms of organization or scalability. There is nothing preventing you from requiring your pre-existing modules in all your Alloy controllers, or even making a single global reference to your module that can be referenced throughout your app. For example:
-->

### Titanium アプリを作る際には起動時にライブラリを読み込んでいました。私のパターンを MVC に当てはめたとき、アプリケーションの機能は深く Controller の名前空間の中に配置した方が良いですか？

特にそうしたい場合を除いて、そうする必要はありません。アプリ全体で使うグローバルな機能セットを既に持っているなら、構造化やスケーラビリティの面でメリットがない限り、それをわざわざ細かくカテゴライズする理由はありません。すべての Alloy コントローラーで既存のモジュールを呼んで構いません、また、アプリ全体から参照可能な単一のグローバルな参照を作成しても構いません。たとえば、以下のようにします。

**apps/alloy.js**
```JavaScript
// Alloy.Globals.refToYourModule will be available in all controllers
Alloy.Globals.refToYourModule = require('yourModule');
```

### Controller のためのパフォーマンス上のベストプラクティスってありますか？

これまでの Titanium 開発でのベストプラクティスは、そのまま Alloy の世界でも適用できます。実際、従来の Titanium 開発でできることは全て Alloy の Controller でもできます。従来の Titanium 開発で学んだ [Coding Best Practices](http://docs.appcelerator.com/titanium/3.0/#!/guide/Coding_Best_Practices) を Alloy で使ってください。つまり、あなたが今できる最善のことは、ランタイムのパフォーマンスを上げるように有効にコンパイラに指示することです。[Alloy Controllers: Conditional Code](Alloy_Controllers.md#Conditional-Code) を参照してください。

<!--
### Are there best practices to employ within controllers for performance?

The same best practices that apply to traditional Titanium development still apply in the world of Alloy. In fact, everything you can do in traditional Titanium development you can do in Alloy's controllers. Use the Coding Best Practices you learned with traditional Titanium development in Alloy. That said, the best trick you can use now is taking advantage of the compiler directives to speed up runtime performance. Refer to Alloy Controllers: Conditional Code
-->

### view の紐付いたたくさんの callback を書いた Controller 達を単に読みこむだけにして、 Alloy に繋がりの管理を任せても構いませんか？

はい、できますし、このコミュニケーションを処理するために Controller 間イベントを使うことも出来ます。Controller 間の疎結合を促すので後者の方が好ましいです。Controller 間での通信用に Controller イベントを使う基本的な例は、master-detail の Alloy テストアプリケーションを参照してください: : https://github.com/appcelerator/alloy/tree/master/test/apps/advanced/master_detail

<!--
### Do I simply load controllers with a bunch of callbacks that relate to the view and let Alloy handle the linkage?

You can do this, or you can use eventing between your controllers to handle this communication. The latter is preferred as it encourages loose coupling between you controllers. For a basic example of using controller eventing to communicate between controllers, refer to the master-detail Alloy test application: https://github.com/appcelerator/alloy/tree/master/test/apps/advanced/master_detail
-->

### 標準的な Titanium アプリケーションに対してやったように、私の organization で Alloy を支援するのに採用すべきベストプラクティスはありますか？

現行の organization のサイズや深さによります。organization に合わせて Alloy を曲げる方が良いか、Alloy に合わせて organization を曲げるほうが良いかを判断する必要があります。Alloy の背後にある原動力の1つは、たくさんの異なる性質の Titanium のコーディング方法を標準化する必要性でした。判断の過程で、あなたのプロジェクトはより綺麗に、よりベストプラクティスに沿った形になるでしょう。またそれだけでなく、他の開発者が支援・貢献しやすいような、プロジェクトの親近感や構造が生まれるでしょう。

<!--
### Is there a best practice that I should use to help out Alloy for my own organization here, as I did in the standard Titanium applications?

It depends on the size and depth of your existing organization. You need to determine if it makes sense to bend Alloy around your existing organization, or bend your existing organization around Alloy. One of the driving forces behind Alloy was a need to standardize the many disparate Titanium coding methodologies out there. In doing so, your projects will be cleaner and more in line with best practices, but also give it a familiar feel and structure that makes it easier for other developers to help and contribute.
-->

コーディング規約のベストプラクティス
------------------------------------

### 命名規則

- 変数名やプロパティ、関数の名前の前に二重の _ を付けてはいけません(例: __foo)。これらの名前は Alloy で予約されています。これらの名前を利用すると、名前が衝突して意図しない動作をする可能性があります。
- ID に JavaScript の予約語を使ってはいけません。現在予約されている語の一覧は[こちら](https://github.com/appcelerator/alloy/blob/master/Alloy/common/constants.js#L101-L112)です。

<!--
Do not use double underscore prefixes on variables, properties, or function names (e.g., __foo). They are reserved for use in Alloy. If you use them, there is potential for conflicts and unexpected behavior.
Do not use JavaScript reserved words as IDs. List of currently reserved words.
-->

### グローバル変数

app.js でグローバル変数を宣言しないで、他のファイルでグローバル変数を利用するようにしてください。今はそういう使い方もできますが、推奨されておらず、将来廃止される予定です。Alloy アプリケーションでグローバル変数を使いたい人は、JS ファイルで以下のように宣言することができます。

<!--
Do not declare global variables in app.js and use them in other files. Such usage is currently allowed but not recommended, and it will be deprecated in the future. Users who wish to use globals in Alloy applications can declare the following in their JS files:
-->

```JavaScript
var Alloy = require('alloy'), Backbone = require('alloy/backbone'), _ = require('alloy/underscore')._;
```

<!--
As an example:
-->

たとえば、このようにします。

```JavaScript
if (typeof Alloy === 'undefined') {
    var Alloy = require('alloy');
}
if (typeof Backbone === 'undefined') {
    var Backbone = require('alloy/backbone');
}
if (typeof _ === 'undefined') {
    var _ = require('alloy/underscore')._;
}

var loading = Alloy.createWidget("com.appcelerator.loading");
```
