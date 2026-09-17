# Material Dialogs

<!-- github-global:langs:start -->
[简体中文](../zh-CN/README.md) | [日本語](./README.md) | [繁體中文](../zh-TW/README.md) | [Español](../es/README.md) | [한국어](../ko/README.md)
<!-- github-global:langs:end -->


![スクリーンショット](https://raw.githubusercontent.com/afollestad/material-dialogs/master/art/mdshowcase.png)

# サンプルプロジェクト

最新のサンプル APK は、このリポジトリからこちらでダウンロードできます: https://github.com/afollestad/material-dialogs/blob/master/sample/sample.apk

Google Play でも公開されています:

<a href="https://play.google.com/store/apps/details?id=com.afollestad.materialdialogssample">
  <img alt="Get it on Google Play"
       src="https://developer.android.com/images/brand/en_generic_rgb_wo_60.png" />
</a>

サンプルプロジェクトをインストールしておくと、新規リリースの通知を受け取るのに便利です。また、このリポジトリを Watch していれば、私がリリースを公開するたびに GitHub からメールが届きます。

---

# Gradle 依存関係 (jCenter)

モジュールの `build.gradle` ファイルに以下の依存関係を追加するだけで、Android プロジェクトでこのライブラリを簡単に参照できます:

```Gradle
dependencies {
    compile 'com.afollestad:material-dialogs:0.7.1.3'
}
```

[ ![Download](https://api.bintray.com/packages/drummer-aidan/maven/material-dialogs/images/download.svg) ](https://bintray.com/drummer-aidan/maven/material-dialogs/_latestVersion)

---

# 新着情報

バージョンのリストと変更履歴については、プロジェクトの Releases ページをご覧ください。

### [リリースを見る](https://github.com/afollestad/material-dialogs/releases)

このリポジトリを Watch すると、私がアップデートを公開するたびに GitHub からメールが届きます。

---

# 基本的なダイアログ

まず、`MaterialDialog` は `DialogBase` を継承し、`DialogBase` は `AlertDialog` を継承していることに注意してください。標準メソッドのうちごく一部は意図的に非推奨化されており動作しませんが、`dismiss()`、`setTitle()`、`setIcon()` などのメソッドは利用可能です。代替手段については後述します。

以下は、Google の Material design ガイドライン(こちら: http://www.google.com/design/spec/components/dialogs.html#dialogs-usage)で見られるダイアログを模した基本的な例です。文字列を受け取るメソッドには、リテラル文字列と文字列リソースのどちらでも指定できること、同様にカラーリソース(例: `titleColor` と `titleColorRes`)でも同じことが可能であることに注意してください。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .negativeText(R.string.disagree)
        .show();
```

Lollipop(API 21+)以降、または AppCompat を使用している場合、Material ダイアログは `positiveColor`(positive アクションボタンに使用されます)を、styles.xml のテーマの `colorAccent` 属性に自動的に合わせます。

コンテンツが十分に長い場合はスクロール可能になり、アクションボタンの上に区切り線が表示されます。

# AlertDialog からの移行

古いダイアログを移行する場合は、```AlertDialogWrapper``` を使用できます。インポートを変更し、```AlertDialog.Builder``` を ```AlertDialogWrapper.Builder``` に置き換える必要があります:

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

ただし、新規に使用する場合は、元の ```MaterialDialog``` API を使用することを強く推奨します。

---

# アイコンの表示

MaterialDialog は、標準の AlertDialog と同様にアイコンの表示をサポートしています。アイコンはタイトルの左側に表示されます。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .icon(R.drawable.icon)
        .show();
```

Builder の `limitIconToDefaultSize()`、`maxIconSize(int size)`、または `maxIconSizeRes(int sizeRes)` メソッドを使用して、アイコンの最大サイズを制限できます。

---

# 積み重ねられたアクションボタン

複数のアクションボタンがあり、それらを合わせると1行に収まらないほど幅が広くなる場合、ダイアログはボタンを縦方向に積み重ねて表示します。

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

ユーザーがアクションボタンを選択したタイミングを知るには、コールバックを設定します。そのためには、`ButtonCallback` クラスを使用し、必要に応じて `onPositive()`、`onNegative()`、`onNeutral()` メソッドをオーバーライドします。この方法の利点は、ボタンの機能を *À la carte*（必要なものだけ）でオーバーライドできるため、空のメソッドをスタブとして用意する必要がないことです。

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

`autoDismiss` が無効になっている場合は、これらのコールバック内でダイアログを手動で閉じる必要があります。自動的に閉じる機能（auto dismiss）はデフォルトで有効になっています。

---

# リストダイアログ

リストダイアログの作成には、文字列の配列を渡すだけで済みます。コールバック(`itemsCallback`)も非常にシンプルです。

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

`autoDismiss` が無効になっている場合は、コールバック内で手動でダイアログを閉じる必要があります。自動的に閉じる機能(auto dismiss)はデフォルトで有効です。
`positiveText()` やその他のアクションボタンをビルダーに渡すことで、リストの下にアクションボタンを強制的に表示させることもできますが、これは一部の特定のケースでのみ有用です。

---

# 単一選択リストダイアログ

単一選択リストダイアログは、通常のリストダイアログとほぼ同一です。唯一の違いは、`itemsCallback` の代わりに `itemsCallbackSingleChoice` を使用してコールバックを設定することです。これにより、リスト項目の横にラジオボタンが表示されるようになります。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .items(R.array.items)
        .itemsCallbackSingleChoice(-1, new MaterialDialog.ListCallbackSingleChoice() {
            @Override
            public boolean onSelection(MaterialDialog dialog, View view, int which, CharSequence text) {
                /**
                 * If you use alwaysCallSingleChoiceCallback(), which is discussed below,
                 * returning false here won't allow the newly selected radio button to actually be selected.
                 **/
                return true;
            }
        })
        .positiveText(R.string.choose)
        .show();
```

項目を事前に選択しておきたい場合は、`itemsCallbackSingleChoice()` の -1 の代わりに 0 以上のインデックスを渡してください。カスタムアダプターを使用していない場合は、後から `MaterialDialog` インスタンスの `setSelectedIndex(int)` を使って選択されたインデックスを更新できます。

`positiveText()` を使用してポジティブアクションボタンを設定しなかった場合、ユーザーがポジティブアクションボタンを押すと、ダイアログは自動的に単一選択コールバックを呼び出します。自動的に閉じる設定がオフになっていない限り、ダイアログは自身を閉じます。

`alwaysCallSingleChoiceCallback()` を呼び出すと、ユーザーが項目を選択するたびに単一選択コールバックが呼び出されるようになります。

## ラジオボタンの色設定

アクションボタンやMaterialダイアログの他の多くの要素と同様に、ダイアログのラジオボタンの色をカスタマイズできます。`Builder` クラスには、`widgetColor()`、`widgetColorRes()`、`widgetColorAttr()` メソッドが用意されています。これらの名前とパラメーターのアノテーションを見れば、用途は自明です。デフォルトでは、ラジオボタンはActivityのテーマ内の `colorAccent`（AppCompatの場合）または `android:colorAccent`（Materialテーマの場合）に設定された色で着色されることに注意してください。

また、このREADMEの「Global Theming」セクションで示されているように、グローバルテーマ属性 `md_widget_color` もあります。

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

項目を事前に選択状態にしておきたい場合は、`itemsCallbackMultiChoice()` の null の代わりにインデックスの配列(リソースまたはリテラル)を渡します。カスタムアダプターを使用していない場合、後から `MaterialDialog` インスタンスの `setSelectedIndices(Integer[])` を使用して、選択されたインデックスを更新できます。

`positiveText()` を使用してポジティブアクションボタンを設定しなかった場合、ユーザーがポジティブアクションボタンを押すと、ダイアログは自動的に複数選択コールバックを呼び出します。自動 dismiss が無効化されていない限り、ダイアログは自身を dismiss します。

`alwaysCallMultiChoiceCallback()` を呼び出すと、ユーザーが項目を選択するたびに複数選択コールバックが呼び出されます。

## チェックボックスの色設定

アクションボタンや Material ダイアログの他の多くの要素と同様に、ダイアログのチェックボックスの色もカスタマイズできます。`Builder` クラスには、`widgetColor()`、`widgetColorRes()`、`widgetColorAttr()` メソッドが用意されています。名前とパラメーターのアノテーションから、その役割は自明です。デフォルトでは、チェックボックスは Activity のテーマ内の `colorAccent`(AppCompat の場合)または `android:colorAccent`(Material テーマの場合)に設定された色で着色されることに注意してください。

また、この README の Global Theming セクションで示されているように、グローバルテーマ属性 `md_widget_color` もあります。

---

# カスタムリストダイアログ

Android のネイティブダイアログと同様に、`.adapter()` で独自のアダプターを渡すことで、リストの動作を細かくカスタマイズできます。

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

`ListView` にアクセスする必要がある場合は、`MaterialDialog` インスタンスを使用できます:

```java
MaterialDialog dialog = new MaterialDialog.Builder(this)
        ...
        .build();

ListView list = dialog.getListView();
// Do something with it

dialog.show();
```

`ListView` にアクセスするためにカスタムアダプターを使う必要はないことに注意してください。`ListView` は、単一/複数選択ダイアログや通常のリストダイアログなどでも利用できます。

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
これにより、必要に応じて（小さい画面、長いコンテンツなどの場合に）ユーザーがカスタムビューをスクロールできるようになります。しかし、この動作を望まない場合もあります。これは主に、カスタムレイアウト内に ScrollView を含める場合、つまり ListView、RecyclerView、WebView、GridView などを使用する場合に該当します。サンプルプロジェクトには、このパラメーターに true と false の両方を使用した例が含まれています。

`wrapInScrollView` が true の場合、カスタムビューの周囲には自動的にパディングが設定されます。そうでない場合は、コンテンツに見合った適切なパディングの値を使用するのはあなたの責任となります。

## 後からビューにアクセスする

ダイアログを構築した後にカスタムビュー内の View にアクセスする必要がある場合は、`MaterialDialog` の `getCustomView()` を使用できます。特に `Builder` にレイアウトリソースを渡した場合に便利で、ダイアログがビューのインフレーションを代わりに処理してくれます。

```java
MaterialDialog dialog = //... initialization via the builder ...
View view = dialog.getCustomView();
```

---

# 書体（Typefaces）

デフォルトでは、Material Dialogs はダイアログのタイトルとアクションボタンに `Roboto Medium` フォントを、コンテンツやリスト項目などに `Roboto Regular` フォントを使用します。これはこのライブラリに含まれているフォントアセットを使用して行われるため、デフォルトで奇妙な手書き風書体を使用する Samsung 端末でも、これらのフォントが使用されます。

このデフォルトの動作を避けたい場合は、`Builder` を使用する際に `disableDefaultFonts()` を呼び出してください。これにより、ライブラリは Roboto および Roboto Medium フォントを適用せず、すべてが通常のシステムフォントで表示されます。

カスタムフォントを明示的に使用したい場合は、`Builder` を使用する際に `typeface(String, String)` を呼び出してください。これにより、プロジェクトの `assets` フォルダ内の TTF ファイルからフォントが読み込まれます。例えば、`/src/main/assets/fonts` に `Roboto.ttf` と `Roboto-Light.ttf` がある場合は、`typeface("Roboto", "Roboto-Light")` を呼び出します。名前には拡張子を付けないことに注意してください。このメソッドは、`TypefaceHelper` を介した Typeface のリサイクルも処理します。この `TypefaceHelper` は、重複した割り当てを避けるために自分のプロジェクトでも使用できます。ttf ファイル以外の Typeface ファイルを読み込みたい場合は、`typeface(Typeface, Typeface)` という Builder メソッドを使用できます。

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

Lollipop 以前は、リフレクションやカスタムドローアブルを使わずに AlertDialog のテーマを設定することはほぼ不可能でした。KitKat 以降、Android はよりカラーニュートラルになりましたが、AlertDialog はタイトルとタイトルの区切り線に Holo Blue を使い続けていました。Lollipop ではさらに改善され、デフォルトではアクションボタン以外にダイアログ内で色が使われなくなりました。このライブラリはテーマ設定をさらに簡単にします。

## 基本操作

デフォルトでは、Material Dialogs はダイアログを生成するコンテキストから取得した `?android:textColorPrimary` 属性に基づいて、ライトテーマまたはダークテーマを適用します。色が明るい場合(例: 白に近い場合)、その Activity はダークテーマを使用していると推測し、ダイアログのダークテーマを使用します。ライトテーマの場合はその逆です。`Builder#theme()` メソッドから使用するテーマを手動で設定することもできます:

```java
new MaterialDialog.Builder(this)
        .content("Hi")
        .theme(Theme.DARK)
        .show();
```

または、次のセクションで説明するグローバルテーマ属性を使用することもできます。グローバルテーマを使用すれば、表示するすべてのダイアログに対してテーマセッターを毎回呼び出す必要がなくなります。

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

名前はほとんどの場合、見れば意味が分かるようになっています。このチュートリアルの他のいくつかのセクションで説明した `widgetColor` メソッドは、プログレスバー、チェックボックス、ラジオボタンに適用されます。また、これらの各メソッドには、色を直接指定する方法、カラーリソースを使用する方法、カラー属性を使用する方法の3つのバリエーションがあることにも注意してください。

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

最初の `btnSelector` の行は、すべてのアクションボタンに使用されるセレクター drawable を設定します。2 番目の `btnSelector` の行は、ポジティブボタンにのみ使用される drawable を上書きします。その結果、ポジティブボタンはニュートラルボタンやネガティブボタンとは異なるセレクターを持つことになります。`btnSelectorStacked` は、ボタンが積み重ねて表示されるときに使用されるセレクター drawable を設定します。これは、すべてのボタンを 1 行に収めるスペースがない場合や、`Builder` で `forceStacked(true)` を使用した場合に発生します。`listSelector` は、カスタムアダプターを使用していない場合のリスト項目に使用されます。

***カスタムアクションボタンセレクターの使用に関する重要な注意点***: デフォルトのものと同様に、セレクター drawable が inset drawable を参照するようにしてください。これは、アクションボタンの正しいパディングのために重要です。

## Gravity

ダイアログ内の要素の gravity を変更したいケースはおそらく少ないと思いますが、変更は可能です。

```java
new MaterialDialog.Builder(this)
        .titleGravity(GravityEnum.CENTER_HORIZONTAL)
        .contentGravity(GravityEnum.CENTER_HORIZONTAL)
        .btnStackedGravity(GravityEnum.START)
        .itemsGravity(GravityEnum.END)
        .buttonsGravity(GravityEnum.END)
        .show();
```

これらはほぼ自明です。`titleGravity` はダイアログタイトルの gravity を設定し、`contentGravity` はダイアログコンテンツの gravity を設定し、`btnStackedGravity` は積み重ねられたアクションボタンの gravity を設定し、`itemsGravity` はリスト項目の gravity を設定します(カスタムアダプターを NOT 使用している場合)。

`buttonsGravity` については、以下を参照してください:

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

Positive ボタンがない場合、Negative ボタンがその位置を引き継ぎます。ただし、CENTER の場合は除きます。

## マテリアルパレット

Material デザインのパレットに合う色を確認するには、次のページを参照してください: http://www.google.com/design/spec/style/color.html#color-color-palette

---

# グローバルテーマ

前のセクションで説明したテーマの多くの側面は、これらの属性のいずれかを含むテーマを持つ Activity から表示されるすべてのダイアログに自動的に適用できます:

```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">

    <!--
        All dialogs will default to Theme.DARK with this set to true.
    -->
    <item name="md_dark_theme">true</item>

    <!--
        This overrides the default dark or light dialog background color.
        Note that if you use a dark color here, you should set md_dark_theme to
        true so text and selectors look visible
    -->
    <item name="md_background_color">#37474F</item>

    <!--
        Applies an icon next to the title in all dialogs.
    -->
    <item name="md_icon">@drawable/ic_launcher</item>
  
    <!--
        Limit icon to a max size.
    -->
    <attr name="md_icon_max_size" format="dimension" />
    
    <!--
        Limit the icon to a default max size (48dp).
    -->
    <attr name="md_icon_limit_icon_to_default_size" format="boolean" />

    <!--
        By default, the title text color is derived from the
        ?android:textColorPrimary system attribute.
    -->
    <item name="md_title_color">#E91E63</item>


    <!--
        By default, the content text color is derived from the
        ?android:textColorSecondary system attribute.
    -->
    <item name="md_content_color">#9C27B0</item>


    <!--
        By default, the positive action text color is derived
        from the colorAccent attribute of AppCompat or android:colorAccent
        attribute of the Material theme.
    -->
    <item name="md_positive_color">#673AB7</item>

    <!--
        By default, the positive action text color is derived
        from the colorAccent attribute of AppCompat or android:colorAccent
        attribute of the Material theme.
    -->
    <item name="md_neutral_color">#673AB7</item>

    <!--
        By default, the positive action text color is derived
        from the colorAccent attribute of AppCompat or android:colorAccent
        attribute of the Material theme.
    -->
    <item name="md_negative_color">#673AB7</item>

    <!--
        By default, a progress dialog's progress bar, check boxes, and radio buttons 
        have a color is derived from the colorAccent attribute of AppCompat or 
        android:colorAccent attribute of the Material theme.
    -->
    <item name="md_widget_color">#673AB7</item>

    <!--
        By default, the list item text color is black for the light
        theme and white for the dark theme.
    -->
    <item name="md_item_color">#9C27B0</item>

    <!--
        This overrides the color used for the top and bottom dividers used when
        content is scrollable
    -->
    <item name="md_divider_color">#E91E63</item>

    <!--
        This overrides the selector used on list items.
    -->
    <item name="md_list_selector">@drawable/selector</item>

    <!--
        This overrides the selector used on stacked action buttons.
    -->
    <item name="md_btn_stacked_selector">@drawable/selector</item>

    <!--
        This overrides the background selector used on the positive action button.
    -->
    <item name="md_btn_positive_selector">@drawable/selector</item>

    <!--
        This overrides the background selector used on the neutral action button.
    -->
    <item name="md_btn_neutral_selector">@drawable/selector</item>

    <!--
        This overrides the background selector used on the negative action button.
    -->
    <item name="md_btn_negative_selector">@drawable/selector</item>
    
    <!-- 
        This sets the gravity used while displaying the dialog title, defaults to start.
        Can be start, center, or end.
    -->
    <item name="md_title_gravity">start</item>
    
    <!-- 
        This sets the gravity used while displaying the dialog content, defaults to start.
        Can be start, center, or end.
    -->
    <item name="md_content_gravity">start</item>
    
    <!--
        This sets the gravity used while displaying the list items (not including custom adapters), defaults to start.
        Can be start, center, or end.
    -->
    <item name="md_items_gravity">start</item>
    
    <!--
        This sets the gravity used while displaying the dialog action buttons, defaults to start.
        
        START (Default)    Neutral     Negative    Positive
        CENTER:            Negative    Neutral     Positive
        END:	           Positive    Negative    Neutral
    -->
    <item name="md_buttons_gravity">start</item>
    
    <!--
        This sets the gravity used while displaying the stacked action buttons, defaults to end.
        Can be start, center, or end.
    -->
    <item name="md_btnstacked_gravity">end</item>

</style>
```

アクションボタンの色も、サンプルプロジェクトに見られるように、Material テーマの `android:colorAccent` 属性、または AppCompat Material テーマの `colorAccent` 属性から取得されます。手動で色を設定すると、その動作は上書きされます。

---

# 表示・キャンセル・解除のコールバック

生成される `MaterialDialog` インスタンス上ではなく、`Builder` から直接 show/cancel/dismiss のリスナーを設定できます:

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

入力ダイアログはその名の通り、入力フィールド(EditText)を使ってアプリケーションのユーザーから入力を受け取るためのものです。必要に応じて、EditText の上にコンテンツを表示することもできます。

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

このダイアログでは、肯定的(positive)アクションボタンが強制的に表示されることに注意してください。ボタンが押されると、入力内容がコールバックに渡されます。

入力ダイアログは、EditText へのフォーカス設定とキーボードの表示を自動的に処理するため、ユーザーはすぐに入力を始められます。ダイアログが閉じられると、キーボードも自動的に閉じられます。

## EditText の色設定

アクションボタンや Material ダイアログの他の多くの要素と同様に、入力ダイアログの `EditText` の色もカスタマイズできます。`Builder` クラスには `widgetColor()`、`widgetColorRes()`、`widgetColorAttr()` メソッドが用意されています。これらの名前とパラメーターのアノテーションを見れば、使い方は自明です。デフォルトでは、EditText は Activity のテーマ内の `colorAccent`（AppCompat の場合）または `android:colorAccent`（Material テーマの場合）に設定された色で着色されることに注意してください。

また、この README の Global Theming セクションで示されているように、グローバルテーマ属性 `md_widget_color` もあります。

---

# プログレスダイアログ

このライブラリを使用すると、Materialデザインのプログレスダイアログを表示できます。さらに、アプリのアクセントカラーを使ってプログレスバーを色付けすることもできます（AppCompatでアプリのテーマを設定している場合、またはLollipopのMaterialテーマを使用している場合）。

## 不確定プログレスダイアログ

これはスピニングサークル（回転する円）が表示される古典的なプログレスダイアログを表示します。実際の動作についてはサンプルプロジェクトを参照してください:

```java
new MaterialDialog.Builder(this)
    .title(R.string.progress_dialog)
    .content(R.string.please_wait)
    .progress(true, 0)
    .show();
```

## 確定型(シークバー)プログレスダイアログ

ダイアログが不確定(indeterminate)でない場合、最大値まで増加する水平のプログレスバーが表示されます。
コード内のコメントに、この動作の説明があります。

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

このダイアログの動作については、スレッド処理を追加したサンプルプロジェクトを参照してください。

## プログレスバーの色設定

アクションボタンや Material ダイアログの他の多くの要素と同様に、プログレスダイアログのプログレスバーの色をカスタマイズできます。`Builder` クラスには `widgetColor()`、`widgetColorRes()`、`widgetColorAttr()` メソッドが用意されています。名前とパラメーターのアノテーションを見れば、それぞれの役割は自明です。デフォルトでは、プログレスバーは Activity のテーマ内の `colorAccent`（AppCompat の場合）または `android:colorAccent`（Material テーマの場合）に設定された色で着色されることに注意してください。

また、この README の Global Theming セクションで示されているように、グローバルテーマ属性 `md_widget_color` もあります。

---

# 設定ダイアログ

Android の `EditTextPreference`、`ListPreference`、`MultiSelectListPreference` を使うと、設定アクティビティの設定項目を、ユーザーが入力や選択で行う操作と関連付けることができます。Material Dialogs には `MaterialEditTextPreference`、`MaterialListPreference`、`MaterialMultiSelectListPreference` クラスが含まれており、preferences XML でこれらを使用することで、自動的に Material テーマのダイアログを利用できます。詳細はサンプルプロジェクトを参照してください。

---

# Tint Helper

`MDTintHelper` クラスを使用すると、チェックボックス、ラジオボタン、エディットテキスト、プログレスバーを動的に着色できます(実行時に `styles.xml` を変更できないことへの回避策です)。このクラスはライブラリ内で、UI 要素を設定した `widgetColor` に合わせて動的に着色するために使用されています。

---

# その他

アクションボタンが押されたときやユーザーがリスト項目を選択したときに、ダイアログを自動的に閉じたくない場合:

```java
MaterialDialog dialog new MaterialDialog.Builder(this)
        // ... other initialization
        .autoDismiss(false)
        .show();
```