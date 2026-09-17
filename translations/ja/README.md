# Material Dialogs

<!-- github-global:langs:start -->
[简体中文](../zh-CN/README.md) | [日本語](./README.md)
<!-- github-global:langs:end -->


![スクリーンショット](https://raw.githubusercontent.com/afollestad/material-dialogs/master/art/mdshowcase.png)

# サンプルプロジェクト

最新のサンプル APK は、このリポジトリからこちらからダウンロードできます: https://github.com/afollestad/material-dialogs/blob/master/sample/sample.apk

Google Play にも公開されています:

<a href="https://play.google.com/store/apps/details?id=com.afollestad.materialdialogssample">
  <img alt="Get it on Google Play"
       src="https://developer.android.com/images/brand/en_generic_rgb_wo_60.png" />
</a>

サンプルプロジェクトをインストールしておくと、新リリースの通知を受け取るのに便利です。また、このリポジトリを Watch していれば、私がリリースを公開するたびに GitHub からメールが届きます。

---

# Gradle 依存関係 (jCenter)

モジュールの `build.gradle` ファイルに以下の依存関係を記述するだけで、Android プロジェクトでこのライブラリを簡単に参照できます:

```Gradle
dependencies {
    compile 'com.afollestad:material-dialogs:0.7.1.3'
}
```

[ ![Download](https://api.bintray.com/packages/drummer-aidan/maven/material-dialogs/images/download.svg) ](https://bintray.com/drummer-aidan/maven/material-dialogs/_latestVersion)

---

# 新着情報

バージョンのリストと各変更履歴については、プロジェクトの Releases ページをご覧ください。

### [リリースを見る](https://github.com/afollestad/material-dialogs/releases)

このリポジトリを Watch すると、私がアップデートを公開するたびに GitHub からメールが届きます。

---

# 基本的なダイアログ

まず、`MaterialDialog` は `DialogBase` を継承し、さらに `DialogBase` は `AlertDialog` を継承していることに注意してください。標準メソッドのごく一部は意図的に非推奨化されており動作しませんが、`dismiss()`、`setTitle()`、`setIcon()` などのメソッドを利用できます。代替手段については後述します。

以下は、Google の Material design ガイドラインで見られるダイアログを模した基本的な例です(こちら: http://www.google.com/design/spec/components/dialogs.html#dialogs-usage)。文字列を受け取るメソッドには、リテラル文字列と文字列リソースのどちらでも指定できること、同様にカラーリソース(例: `titleColor` と `titleColorRes`)でも同じことができる点に注意してください。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .negativeText(R.string.disagree)
        .show();
```

Lollipop(API 21+)以降、または AppCompat を使用している場合、Material ダイアログは `positiveColor`(positive アクションボタンに使用される色)を、styles.xml のテーマの `colorAccent` 属性に自動的に合わせます。

コンテンツが十分に長い場合はスクロール可能になり、アクションボタンの上に区切り線が表示されます。

---

# AlertDialogからの移行

古いダイアログを移行する場合は、```AlertDialogWrapper``` を使用できます。importを変更し、```AlertDialog.Builder``` を ```AlertDialogWrapper.Builder``` に置き換える必要があります:

```java
new AlertDialogWrapper.Builder(this)
        .setTitle(R.string.title)
        .setMessage(R.string.message)
        .setNegativeButton(R.string.OK, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                dialog.dismiss();
            }
        }).show();
```

ただし、新規に使用する場合は、オリジナルの ```MaterialDialog``` API を使用することを強く推奨します。

---

# アイコンの表示

MaterialDialog は標準の AlertDialog と同様にアイコンの表示をサポートしています。アイコンはタイトルの左側に表示されます。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .icon(R.drawable.icon)
        .show();
```

`limitIconToDefaultSize()`、`maxIconSize(int size)`、または `maxIconSizeRes(int sizeRes)` の Builder メソッドを使用して、アイコンの最大サイズを制限できます。

---

# 積み重ねアクションボタン

複数のアクションボタンを合わせて1行に収まりきらない場合、ダイアログはボタンを縦方向に積み重ねて表示します。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.longer_positive)
        .negativeText(R.string.negative)
        .show();
```

また、`Builder` の `forceStacking()` メソッドを使用して、ダイアログにボタンを強制的に積み重ねさせることもできます。

---

# ニュートラルアクションボタン

肯定テキストと否定テキストに加えて、ニュートラルテキストを指定できます。ニュートラルアクションは左端に表示されます。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .negativeText(R.string.disagree)
        .neutralText(R.string.more_info)
        .show();
```

---

# コールバック

ユーザーがアクションボタンを選択したタイミングを知るには、コールバックを設定します。そのためには、`ButtonCallback` クラスを使用し、必要に応じて `onPositive()`、`onNegative()`、または `onNeutral()` メソッドをオーバーライドします。この方法の利点は、ボタンの機能を *À la carte*(必要なものだけ)でオーバーライドできるため、空のメソッドをスタブとして用意する必要がないことです。

```java
new MaterialDialog.Builder(this)
        .callback(new MaterialDialog.ButtonCallback() {
            @Override
            public void onPositive(MaterialDialog dialog) {
            }
        });

new MaterialDialog.Builder(this)
        .callback(new MaterialDialog.ButtonCallback() {
            @Override
            public void onPositive(MaterialDialog dialog) {
            }

            @Override
            public void onNegative(MaterialDialog dialog) {
            }
        });

new MaterialDialog.Builder(this)
        .callback(new MaterialDialog.ButtonCallback() {
            @Override
            public void onPositive(MaterialDialog dialog) {
            }

            @Override
            public void onNegative(MaterialDialog dialog) {
            }

            @Override
            public void onNeutral(MaterialDialog dialog) {
            }
        });
```

`autoDismiss` が無効になっている場合は、これらのコールバック内で手動でダイアログを閉じる必要があります。自動的に閉じる機能(auto dismiss)はデフォルトで有効になっています。

---

# リストダイアログ

リストダイアログの作成には、文字列の配列を渡すだけです。コールバック(`itemsCallback`)も非常にシンプルです。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .items(R.array.items)
        .itemsCallback(new MaterialDialog.ListCallback() {
            @Override
            public void onSelection(MaterialDialog dialog, View view, int which, CharSequence text) {
            }
        })
        .show();
```

`autoDismiss` をオフにした場合は、コールバック内で手動でダイアログを閉じる必要があります。自動的に閉じる機能はデフォルトで有効です。
`positiveText()` やその他のアクションボタンをビルダーに渡すことで、リストの下にアクションボタンを強制的に表示させることもできますが、これは一部の特定のケースでのみ有用です。

---

# 単一選択リストダイアログ

単一選択リストダイアログは、通常のリストダイアログとほぼ同じです。唯一の違いは、`itemsCallback` の代わりに `itemsCallbackSingleChoice` を使用してコールバックを設定することです。これにより、ダイアログはリスト項目の横にラジオボタンを表示するようになります。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .items(R.array.items)
        .itemsCallbackSingleChoice(-1, new MaterialDialog.ListCallbackSingleChoice() {
            @Override
            public boolean onSelection(MaterialDialog dialog, View view, int which, CharSequence text) {
                /**
                 * 後述の alwaysCallSingleChoiceCallback() を使用している場合、
                 * ここで false を返すと、新しく選択されたラジオボタンが実際には選択されなくなります。
                 **/
                return true;
            }
        })
        .positiveText(R.string.choose)
        .show();
```

項目を事前に選択しておきたい場合は、`itemsCallbackSingleChoice()` の -1 の代わりに 0 以上のインデックスを渡してください。カスタムアダプターを使用していない場合、後から `MaterialDialog` インスタンスの `setSelectedIndex(int)` を使って選択インデックスを更新できます。

`positiveText()` を使用して正のアクションボタンを設定しなかった場合、ユーザーが正のアクションボタンを押すと、ダイアログは自動的に単一選択コールバックを呼び出します。また、自動解除が無効になっていない限り、ダイアログは自動的に閉じられます。

`alwaysCallSingleChoiceCallback()` を呼び出すと、ユーザーが項目を選択するたびに単一選択コールバックが呼び出されます。

## ラジオボタンの色設定

アクションボタンや Material ダイアログの他の多くの要素と同様に、ダイアログのラジオボタンの色をカスタマイズできます。`Builder` クラスには `widgetColor()`、`widgetColorRes()`、`widgetColorAttr()` メソッドが用意されています。これらの名前とパラメーターのアノテーションは、名前から自明です。デフォルトでは、ラジオボタンは Activity のテーマ内の `colorAccent` (AppCompat の場合) または `android:colorAccent` (Material テーマの場合) に設定された色で着色されることに注意してください。

また、この README の Global Theming セクションで示されているように、グローバルテーマ属性 `md_widget_color` もあります。

---

# 複数選択リストダイアログ

複数選択リストダイアログは、通常のリストダイアログとほぼ同一です。唯一の違いは、`itemsCallback` の代わりに `itemsCallbackMultiChoice` を使用してコールバックを設定することです。これにより、ダイアログはリスト項目の横にチェックボックスを表示するようになり、コールバックは複数の選択を返すことができます。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .items(R.array.items)
        .itemsCallbackMultiChoice(null, new MaterialDialog.ListCallbackMultiChoice() {
            @Override
            public boolean onSelection(MaterialDialog dialog, Integer[] which, CharSequence[] text) {
                /**
                 * If you use alwaysCallMultiChoiceCallback(), which is discussed below,
                 * returning false here won't allow the newly selected check box to actually be selected.
                 * See the limited multi choice dialog example in the sample project for details.
                 **/
                 return true;
            }
        })
        .positiveText(R.string.choose)
        .show();
```

項目を事前に選択状態にしておきたい場合は、`itemsCallbackMultiChoice()` の null の代わりにインデックスの配列(リソースまたはリテラル)を渡してください。カスタムアダプターを使用していない場合、後から `MaterialDialog` インスタンスの `setSelectedIndices(Integer[])` を使って選択されたインデックスを更新できます。

`positiveText()` を使用して肯定アクションボタンを設定しなかった場合、ユーザーが肯定アクションボタンを押すと、ダイアログは自動的に複数選択コールバックを呼び出します。自動解除(auto dismiss)が無効化されていない限り、ダイアログは自身を閉じます。

`alwaysCallMultiChoiceCallback()` を呼び出した場合、ユーザーが項目を選択するたびに複数選択コールバックが呼び出されます。

## チェックボックスの色設定

アクションボタンや Material ダイアログの他の多くの要素と同様に、ダイアログのチェックボックスの色をカスタマイズできます。`Builder` クラスには `widgetColor()`、`widgetColorRes()`、`widgetColorAttr()` メソッドが用意されています。名前とパラメーターのアノテーションを見れば、使い方は自明です。デフォルトでは、チェックボックスは Activity のテーマ内の `colorAccent`（AppCompat の場合）または `android:colorAccent`（Material テーマの場合）に設定された色で着色されることに注意してください。

この README の Global Theming セクションで示されているように、グローバルテーマ属性もあります: `md_widget_color`。

---

# カスタムリストダイアログ

Android のネイティブダイアログと同様に、`.adapter()` を使って独自のアダプターを渡すことで、リストの動作を自由にカスタマイズできます。

```java
new MaterialDialog.Builder(this)
        .title(R.string.socialNetworks)
        .adapter(new ButtonItemAdapter(this, R.array.socialNetworks),
                new MaterialDialog.ListCallback() {
                    @Override
                    public void onSelection(MaterialDialog dialog, View itemView, int which, CharSequence text) {
                        Toast.makeText(MainActivity.this, "Clicked item " + which, Toast.LENGTH_SHORT).show();
                    }
                })
        .show();
```

`ListView` にアクセスする必要がある場合は、`MaterialDialog` のインスタンスを使用できます:

```java
MaterialDialog dialog = new MaterialDialog.Builder(this)
        ...
        .build();

ListView list = dialog.getListView();
// Do something with it

dialog.show();
```

`ListView` にアクセスするためにカスタムアダプターを使う必要はありません。単一選択/複数選択ダイアログや通常のリストダイアログなどでも利用できます。

---

# カスタムビュー

カスタムビューは非常に簡単に実装できます。

```java
boolean wrapInScrollView = true;
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .customView(R.layout.custom_view, wrapInScrollView)
        .positiveText(R.string.positive)
        .show();
```

`wrapInScrollView` が true の場合、ライブラリが自動的にカスタムビューを ScrollView の中に配置します。
これにより、必要に応じて(小さい画面、長いコンテンツなど)ユーザーがカスタムビューをスクロールできるようになります。ただし、この動作を望まない場合もあります。これは主に、カスタムレイアウト内に ScrollView を含める場合、具体的には ListView、RecyclerView、WebView、GridView などを使用する場合に該当します。サンプルプロジェクトには、このパラメーターに true と false の両方を使用した例が含まれています。

`wrapInScrollView` が true の場合、カスタムビューの周囲には自動的にパディングが設定されます。それ以外の場合は、コンテンツに見合った適切なパディングの値を自分で設定する必要があります。

## 後からのアクセス

ダイアログが構築された後にカスタムビュー内の View にアクセスする必要がある場合は、`MaterialDialog` の `getCustomView()` を使用できます。`Builder` にレイアウトリソースを渡した場合に特に便利で、ダイアログがビューのインフレーションを代わりに処理してくれます。

```java
MaterialDialog dialog = //... initialization via the builder ...
View view = dialog.getCustomView();
```

---

# 書体（Typefaces）

デフォルトでは、Material Dialogs はダイアログのタイトルとアクションボタンに `Roboto Medium` フォントを、コンテンツやリスト項目などに `Roboto Regular` を使用します。これはこのライブラリに含まれるフォントアセットを使用して行われるため、デフォルトで奇妙な手書き風書体を使用する Samsung 端末でも、これらのフォントが使用されます。

このデフォルトの動作を避けたい場合は、`Builder` を使用する際に `disableDefaultFonts()` を呼び出すことができます。これにより、ライブラリは Roboto および Roboto Medium フォントを適用せず、すべてが通常のシステムフォントで表示されます。

カスタムフォントを明示的に使用したい場合は、`Builder` を使用する際に `typeface(String, String)` を呼び出すことができます。これにより、プロジェクトの `assets` フォルダ内の TTF ファイルからフォントが読み込まれます。例えば、`/src/main/assets/fonts` に `Roboto.ttf` と `Roboto-Light.ttf` がある場合、`typeface("Roboto", "Roboto-Light")` を呼び出します。名前に拡張子は使用しないことに注意してください。このメソッドは、`TypefaceHelper` を介した Typeface の再利用も処理します。この `TypefaceHelper` を自分のプロジェクトで使用すれば、重複したアロケーションを避けることができます。ttf ファイル以外の Typeface ファイルを読み込みたい場合は、`typeface(Typeface, Typeface)` Builder メソッドを使用できます。

---

# アクションボタンの取得と設定

ダイアログが構築されて表示された後に、ダイアログのアクションボタンへの参照を取得したい場合(例: ボタンの有効化・無効化など):

```java
MaterialDialog dialog = //... initialization via the builder ...
View negative = dialog.getActionButton(DialogAction.NEGATIVE);
View neutral = dialog.getActionButton(DialogAction.NEUTRAL);
View positive = dialog.getActionButton(DialogAction.POSITIVE);
```

ダイアログのアクションボタンのタイトルを更新したい場合(リテラル文字列の代わりに文字列リソース ID を渡すこともできます):

```java
MaterialDialog dialog = //... initialization via the builder ...
dialog.setActionButton(DialogAction.NEGATIVE, "New Title");
```

---

# テーマ設定

Lollipop 以前は、リフレクションやカスタムドローアブルを使わずに AlertDialog のテーマ設定を行うことはほぼ不可能でした。KitKat 以降、Android はよりカラーニュートラルになりましたが、AlertDialog はタイトルとタイトルの区切り線に Holo Blue を使い続けていました。Lollipop ではさらに改善され、デフォルトではアクションボタン以外にダイアログ内に色が使われなくなりました。このライブラリはテーマ設定をさらに簡単にします。

## 基本事項

デフォルトでは、Material Dialogs はダイアログを生成するコンテキストから取得した `?android:textColorPrimary` 属性に基づいて、ライトテーマまたはダークテーマを適用します。色が明るい場合(例:白に近い場合)、その Activity はダークテーマを使用していると推測し、ダイアログのダークテーマを使用します。ライトテーマの場合はその逆です。テーマは `Builder#theme()` メソッドから手動で設定できます:

```java
new MaterialDialog.Builder(this)
        .content("Hi")
        .theme(Theme.DARK)
        .show();
```

また、次のセクションで説明するグローバルテーマ属性を使用することもできます。グローバルテーマを使用すれば、表示するすべてのダイアログに対してテーマセッターを毎回呼び出す必要がなくなります。

## 色

このライブラリで作成したダイアログのほぼすべての要素に色を付けることができます:

```java
new MaterialDialog.Builder(this)
        .titleColorRes(R.color.material_red_500)
        .contentColor(Color.WHITE) // notice no 'res' postfix for literal color
        .dividerColorRes(R.color.material_pink_500)
        .backgroundColorRes(R.color.material_blue_grey_800)
        .positiveColorRes(R.color.material_red_500)
        .neutralColorRes(R.color.material_red_500)
        .negativeColorRes(R.color.material_red_500)
        .widgetColorRes(R.color.material_red_500)
        .show();
```

名前はほとんどの場合、見れば意味がわかるようになっています。このチュートリアルの他のいくつかのセクションで説明した `widgetColor` メソッドは、プログレスバー、チェックボックス、ラジオボタンに適用されます。また、これらの各メソッドには、色を直接指定する方法、カラーリソースを使用する方法、カラー属性を使用する方法の3つのバリエーションがあることにも注意してください。

## セレクター

テーマのセレクターを使用すると、押下可能な要素の色を変更できます:

```java
new MaterialDialog.Builder(this)
        .btnSelector(R.drawable.custom_btn_selector)
        .btnSelector(R.drawable.custom_btn_selector_primary, DialogAction.POSITIVE)
        .btnSelectorStacked(R.drawable.custom_btn_selector_stacked)
        .listSelector(R.drawable.custom_list_and_stackedbtn_selector)
        .show();
```

最初の `btnSelector` の行は、すべてのアクションボタンに使用されるセレクタードローアブルを設定します。2番目の `btnSelector` の行は、ポジティブボタンにのみ使用されるドローアブルを上書きします。その結果、ポジティブボタンはニュートラルボタンやネガティブボタンとは異なるセレクターを持つことになります。`btnSelectorStacked` は、ボタンがすべて1行に収まるだけのスペースがない場合や、`Builder` で `forceStacked(true)` を使用した場合にボタンが積み重ねられるときに使用されるセレクタードローアブルを設定します。`listSelector` は、カスタムアダプターを使用していない場合のリスト項目に使用されます。

***カスタムアクションボタンセレクターを使用する際の重要な注意点***: デフォルトのものと同様に、セレクタードローアブルがインセットドローアブルを参照していることを確認してください。これは正しいアクションボタンのパディングのために重要です。

## Gravity

ダイアログ内の要素の gravity を変更したいケースはおそらくあまりないと思いますが、変更は可能です。

```java
new MaterialDialog.Builder(this)
        .titleGravity(GravityEnum.CENTER_HORIZONTAL)
        .contentGravity(GravityEnum.CENTER_HORIZONTAL)
        .btnStackedGravity(GravityEnum.START)
        .itemsGravity(GravityEnum.END)
        .buttonsGravity(GravityEnum.END)
        .show();
```

これらはほぼ自明です。`titleGravity` はダイアログタイトルの gravity を設定し、`contentGravity` はダイアログコンテンツの gravity を設定し、`btnStackedGravity` は縦積みされたアクションボタンの gravity を設定し、`itemsGravity` はリスト項目の gravity を設定します(カスタムアダプターを使用していない場合)。

`buttonsGravity` については以下を参照してください:

<table>
<tr>
<td><b>START (デフォルト)</b></td>
<td>Neutral</td>
<td>Negative</td>
<td>Positive</td>
</tr>
<tr>
<td><b>CENTER</b></td>
<td>Negative</td>
<td>Neutral</td>
<td>Positive</td>
</tr>
<tr>
<td><b>END</b></td>
<td>Positive</td>
<td>Negative</td>
<td>Neutral</td>
</tr>
</table>

Positive ボタンがない場合、CENTER を除いて Negative ボタンがその位置を担当します。

## Material パレット

Material デザインのパレットに合う色を確認するには、次のページを参照してください: http://www.google.com/design/spec/style/color.html#color-color-palette

---

# グローバルテーマ設定

上記のセクションで説明したテーマの多くの側面は、これらの属性のいずれかを含むテーマを持つ Activity から表示するすべてのダイアログに自動的に適用できます:

```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">

    <!--
        これを true に設定すると、すべてのダイアログがデフォルトで Theme.DARK になります。
    -->
    <item name="md_dark_theme">true</item>

    <!--
        これはデフォルトのダークまたはライトのダイアログ背景色を上書きします。
        ここで暗い色を使用する場合は、テキストやセレクターが見えるように
        md_dark_theme を true に設定する必要があることに注意してください。
    -->
    <item name="md_background_color">#37474F</item>

    <!--
        すべてのダイアログのタイトルの横にアイコンを適用します。
    -->
    <item name="md_icon">@drawable/ic_launcher</item>
  
    <!--
        アイコンの最大サイズを制限します。
    -->
    <attr name="md_icon_max_size" format="dimension" />
    
    <!--
        アイコンをデフォルトの最大サイズ (48dp) に制限します。
    -->
    <attr name="md_icon_limit_icon_to_default_size" format="boolean" />

    <!--
        デフォルトでは、タイトルのテキスト色は
        ?android:textColorPrimary システム属性から取得されます。
    -->
    <item name="md_title_color">#E91E63</item>


    <!--
        デフォルトでは、コンテンツのテキスト色は
        ?android:textColorSecondary システム属性から取得されます。
    -->
    <item name="md_content_color">#9C27B0</item>


    <!--
        デフォルトでは、肯定アクションのテキスト色は AppCompat の colorAccent 属性、
        または Material テーマの android:colorAccent 属性から取得されます。
    -->
    <item name="md_positive_color">#673AB7</item>

    <!--
        デフォルトでは、肯定アクションのテキスト色は AppCompat の colorAccent 属性、
        または Material テーマの android:colorAccent 属性から取得されます。
    -->
    <item name="md_neutral_color">#673AB7</item>

    <!--
        デフォルトでは、肯定アクションのテキスト色は AppCompat の colorAccent 属性、
        または Material テーマの android:colorAccent 属性から取得されます。
    -->
    <item name="md_negative_color">#673AB7</item>

    <!--
        デフォルトでは、プログレスダイアログのプログレスバー、チェックボックス、
        ラジオボタンの色は AppCompat の colorAccent 属性、または
        Material テーマの android:colorAccent 属性から取得されます。
    -->
    <item name="md_widget_color">#673AB7</item>

    <!--
        デフォルトでは、リスト項目のテキスト色はライトテーマでは黒、
        ダークテーマでは白になります。
    -->
    <item name="md_item_color">#9C27B0</item>

    <!--
        これは、コンテンツがスクロール可能な場合に使用される上下の
        区切り線の色を上書きします。
    -->
    <item name="md_divider_color">#E91E63</item>

    <!--
        これはリスト項目に使用されるセレクターを上書きします。
    -->
    <item name="md_list_selector">@drawable/selector</item>

    <!--
        これは積み重ねられたアクションボタンに使用されるセレクターを上書きします。
    -->
    <item name="md_btn_stacked_selector">@drawable/selector</item>

    <!--
        これは肯定アクションボタンに使用される背景セレクターを上書きします。
    -->
    <item name="md_btn_positive_selector">@drawable/selector</item>

    <!--
        これは中立アクションボタンに使用される背景セレクターを上書きします。
    -->
    <item name="md_btn_neutral_selector">@drawable/selector</item>

    <!--
        これは否定アクションボタンに使用される背景セレクターを上書きします。
    -->
    <item name="md_btn_negative_selector">@drawable/selector</item>
    
    <!-- 
        ダイアログタイトルの表示に使用される gravity を設定します。デフォルトは start です。
        start、center、end のいずれかを指定できます。
    -->
    <item name="md_title_gravity">start</item>
    
    <!-- 
        ダイアログコンテンツの表示に使用される gravity を設定します。デフォルトは start です。
        start、center、end のいずれかを指定できます。
    -->
    <item name="md_content_gravity">start</item>
    
    <!--
        リスト項目の表示に使用される gravity を設定します (カスタムアダプターは除く)。デフォルトは start です。
        start、center、end のいずれかを指定できます。
    -->
    <item name="md_items_gravity">start</item>
    
    <!--
        ダイアログのアクションボタンの表示に使用される gravity を設定します。デフォルトは start です。
        
        START (デフォルト)    Neutral     Negative    Positive
        CENTER:               Negative    Neutral     Positive
        END:	              Positive    Negative    Neutral
    -->
    <item name="md_buttons_gravity">start</item>
    
    <!--
        積み重ねられたアクションボタンの表示に使用される gravity を設定します。デフォルトは end です。
        start、center、end のいずれかを指定できます。
    -->
    <item name="md_btnstacked_gravity">end</item>

</style>
```

アクションボタンの色も、サンプルプロジェクトで見られるように、Material テーマの `android:colorAccent` 属性、または AppCompat Material テーマの `colorAccent` 属性から取得されます。手動で色を設定すると、その動作は上書きされます。

---

# 表示・キャンセル・終了コールバック

生成される `MaterialDialog` インスタンス上ではなく、`Builder` から直接 show/cancel/dismiss リスナーを設定できます:

```java
new MaterialDialog.Builder(this)
    .title("Use Google's Location Services?")
    .content("Let Google help apps determine location. This means sending anonymous location data to Google, even when no apps are running.")
    .positiveText("Agree")
    .showListener(new DialogInterface.OnShowListener() {
        @Override
        public void onShow(DialogInterface dialog) {
        }
    })
    .cancelListener(new DialogInterface.OnCancelListener() {
        @Override
        public void onCancel(DialogInterface dialog) {
        }
    })
    .dismissListener(new DialogInterface.OnDismissListener() {
        @Override
        public void onDismiss(DialogInterface dialog) {
        }
    })
    .show();
```

---

# 入力ダイアログ

入力ダイアログはその名の通り、入力フィールド(EditText)を使ってアプリケーションのユーザーから入力を受け取るものです。必要に応じて、EditText の上にコンテンツを表示することもできます。

```java
new MaterialDialog.Builder(this)
        .title(R.string.input)
        .content(R.string.input_content)
        .input(R.string.input_hint, R.string.input_prefill, new MaterialDialog.InputCallback() {
            @Override
            public void onInput(MaterialDialog dialog, CharSequence input) {
                // Do something
            }
        }).show();
```

このダイアログでは、肯定アクションボタンが強制的に表示されることに注意してください。ボタンが押されると、入力内容がコールバックに送信されます。

入力ダイアログは、EditText へのフォーカス設定とキーボードの表示を自動的に処理するため、ユーザーはすぐに入力を始めることができます。ダイアログが閉じられると、キーボードも自動的に非表示になります。

## EditText の色設定

アクションボタンや Material ダイアログの他の多くの要素と同様に、入力ダイアログの `EditText` の色をカスタマイズできます。`Builder` クラスには `widgetColor()`、`widgetColorRes()`、`widgetColorAttr()` メソッドが用意されています。名前とパラメーターのアノテーションから、その役割は自明です。デフォルトでは、EditText は Activity のテーマにある `colorAccent`(AppCompat の場合)または `android:colorAccent`(Material テーマの場合)の色で着色されることに注意してください。

また、この README の Global Theming セクションで示されているグローバルテーマ属性 `md_widget_color` もあります。

---

# プログレスダイアログ

このライブラリを使用すると、Materialデザインのプログレスダイアログを表示できます。さらに、アプリのアクセントカラーを使ってプログレスバーを色付けすることもできます(AppCompat でアプリをテーマ化している場合、または Lollipop の Material テーマを使用している場合)。

## 不定進捗ダイアログ

回転する円が表示されるクラシックな進捗ダイアログを表示します。動作の様子はサンプルプロジェクトをご覧ください:

```java
new MaterialDialog.Builder(this)
    .title(R.string.progress_dialog)
    .content(R.string.please_wait)
    .progress(true, 0)
    .show();
```

## 確定型(シークバー)プログレスダイアログ

ダイアログが不確定(indeterminate)でない場合、最大値まで増加する水平のプログレスバーが表示されます。
コード内のコメントで、この処理の内容を説明しています。

```java
// Create and show a non-indeterminate dialog with a max value of 150
// If the showMinMax parameter is true, a min/max ratio will be shown to the left of the seek bar.
boolean showMinMax = true;
MaterialDialog dialog = new MaterialDialog.Builder(this)
    .title(R.string.progress_dialog)
    .content(R.string.please_wait)
    .progress(false, 150, showMinMax)
    .show();

// Loop until the dialog's progress value reaches the max (150)
while (dialog.getCurrentProgress() != dialog.getMaxProgress()) {
    // If the progress dialog is cancelled (the user closes it before it's done), break the loop
    if (dialog.isCancelled()) break;
    // Wait 50 milliseconds to simulate doing work that requires progress
    try {
        Thread.sleep(50);
    } catch (InterruptedException e) {
        break;
    }
    // Increment the dialog's progress by 1 after sleeping for 50ms
    dialog.incrementProgress(1);
}

// When the loop exits, set the dialog content to a string that equals "Done"
dialog.setContent(getString(R.string.done));
```

このダイアログの動作については、スレッド処理の追加も含めてサンプルプロジェクトを参照してください。

## プログレスバーの色設定

アクションボタンや Material ダイアログの他の多くの要素と同様に、プログレスダイアログのプログレスバーの色をカスタマイズできます。`Builder` クラスには `widgetColor()`、`widgetColorRes()`、`widgetColorAttr()` メソッドが用意されています。これらの名前とパラメーターのアノテーションを見れば、使い方は自明です。デフォルトでは、プログレスバーは Activity のテーマ内の `colorAccent`（AppCompat の場合）または `android:colorAccent`（Material テーマの場合）に設定された色で着色されることに注意してください。

また、この README の Global Theming セクションで示されているように、グローバルテーマ属性 `md_widget_color` もあります。

---

# 設定ダイアログ

Android の `EditTextPreference`、`ListPreference`、`MultiSelectListPreference` を使うと、設定アクティビティの設定項目を、ユーザーが入力や選択によって行う操作と関連付けることができます。Material Dialogs には `MaterialEditTextPreference`、`MaterialListPreference`、`MaterialMultiSelectListPreference` クラスが含まれており、これらを preferences XML で使用することで、自動的に Material テーマのダイアログを利用できます。詳細はサンプルプロジェクトを参照してください。

---

# Tint Helper

`MDTintHelper` クラスを使用すると、チェックボックス、ラジオボタン、エディットテキスト、プログレスバーを動的に色付けできます（実行時に `styles.xml` を変更できない問題を回避するため）。このライブラリでは、設定した `widgetColor` に合わせて UI 要素を動的に色付けするために使用されています。

---

# その他

アクションボタンが押されたときやユーザーがリスト項目を選択したときに、ダイアログを自動的に閉じたくない場合：

```java
MaterialDialog dialog new MaterialDialog.Builder(this)
        // ... other initialization
        .autoDismiss(false)
        .show();
```