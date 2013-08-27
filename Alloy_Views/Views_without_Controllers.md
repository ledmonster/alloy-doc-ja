Controller なしの View
======================

オプションで style sheet と組み合わせて、View は Controller なしで作成することができます。これらの  View は GUI を作成するための組み立てブロックとして利用することができます。たとえば、button view は View Controller の中でダイアログボックスを作成するために繰り返し利用することが可能で、下記の app/views/foo.xml View は Controller なしで定義されます。

<!--
Views can be created without controllers with an option style sheet. These views can be used as building blocks to create a GUI. For instance, a button view could be reused repeatedly in a view-controller to construct a dialog box. For example, the following view app/views/foo.xml can be defined without a controller:
-->

```xml
<Alloy>
    <Button id='fooButton'>I am a foo button!</Button>
</Alloy>
```

この View は、ユニークな ID のついた Require タグを使うことで、他の View の中で繰り返し呼び出すことができます。

<!--
This view can be inserted into another view multiple times by using the Require tag and assigning it an unique ID. For example,
-->

```xml
<Alloy>
    <Window layout="vertical">
        <Require id="button1" src="foo" type="view"/>
        <Require id="button2" src="foo" type="view"/>
        <Require id="button3" src="foo" type="view"/>
    </Window>
</Alloy>
```

そして、 Controller は $.button1, $.button2, $.button3 を使って foo View の各インスタンスにアクセスすることができます。

<!--
Then, the controller can use $.button1, $.button2 and $.button3 to access each instance of the foo view.
-->

Controller で View を動的に作成するには、 Alloy.createController(view_name).getView() メソッドを使って新しい View のインスタンスを作成してください。

<!--
To dynamically generate a view with a controller, create a new instance of the view using the Alloy.createController(view_name).getView() method. For example,
-->

```JavaScript
// Get an instance of the view
var button = Alloy.createController('foo').getView();
// Modify the view
button.bottom = 0;
button.title = 'I am not a foo button anymore.';
button.width = Ti.UI.SIZE;
button.addEventListener('click', doClick);
// Add the view
$.index.add(button);
```
