# Material Dialogs

<!-- github-global:langs:start -->
[简体中文](./README.md) | [日本語](../ja/README.md)
<!-- github-global:langs:end -->


![截图](https://raw.githubusercontent.com/afollestad/material-dialogs/master/art/mdshowcase.png)

# 示例项目

你可以从此仓库下载最新的示例 APK：https://github.com/afollestad/material-dialogs/blob/master/sample/sample.apk

它也上架了 Google Play：

<a href="https://play.google.com/store/apps/details?id=com.afollestad.materialdialogssample">
  <img alt="Get it on Google Play"
       src="https://developer.android.com/images/brand/en_generic_rgb_wo_60.png" />
</a>

安装示例项目是获取新版本发布通知的好方法。当然，关注（Watch）此仓库后，每当我发布新版本时，GitHub 也会向你发送电子邮件通知。

---

# Gradle 依赖 (jCenter)

在你的 Android 项目的模块级 `build.gradle` 文件中使用以下依赖，即可轻松引用该库：

```Gradle
dependencies {
    compile 'com.afollestad:material-dialogs:0.7.1.3'
}
```

[ ![下载](https://api.bintray.com/packages/drummer-aidan/maven/material-dialogs/images/download.svg) ](https://bintray.com/drummer-aidan/maven/material-dialogs/_latestVersion)

---

# 更新日志

请查看项目的 Releases 页面，获取各版本及其变更日志的列表。

### [查看发布版本](https://github.com/afollestad/material-dialogs/releases)

如果你 Watch（关注）了这个仓库，那么每当我发布更新时，GitHub 都会给你发送一封电子邮件。

---

# 基本对话框

首先请注意，`MaterialDialog` 继承自 `DialogBase`，而 `DialogBase` 又继承自 `AlertDialog`。虽然少数原生方法被有意弃用且无法使用，但你仍然可以使用诸如 `dismiss()`、`setTitle()`、`setIcon()` 等方法。替代方案将在下文讨论。

下面是一个基本示例，它模仿了你在 Google 的 Material 设计指南中看到的对话框（见此处：http://www.google.com/design/spec/components/dialogs.html#dialogs-usage）。请注意，对于接受字符串的方法，你始终可以替换使用字面字符串或字符串资源，颜色资源也是如此（例如 `titleColor` 和 `titleColorRes`）。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .negativeText(R.string.disagree)
        .show();
```

在 Lollipop（API 21+）上，或者如果你使用 AppCompat，Material 对话框会自动将 `positiveColor`（用于肯定操作按钮）匹配到你 styles.xml 主题中的 `colorAccent` 属性。

如果内容足够长，它将变为可滚动状态，并且会在操作按钮上方显示一条分隔线。

---

# 从 AlertDialogs 迁移

如果你正在迁移旧的对话框，可以使用 ```AlertDialogWrapper```。你需要修改导入，并将 ```AlertDialog.Builder``` 替换为 ```AlertDialogWrapper.Builder```：

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

但对于新的使用场景，强烈建议使用原生的 ```MaterialDialog``` API。

---

# 显示图标

MaterialDialog 支持像原生 AlertDialog 一样显示图标；图标会显示在标题的左侧。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .icon(R.drawable.icon)
        .show();
```

你可以使用 `limitIconToDefaultSize()`、`maxIconSize(int size)` 或 `maxIconSizeRes(int sizeRes)` 这些 Builder 方法来限制图标的最大尺寸。

---

# 堆叠操作按钮

如果你有多个操作按钮，它们加在一起太宽而无法在一行内显示时，对话框会将按钮堆叠为垂直排列。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.longer_positive)
        .negativeText(R.string.negative)
        .show();
```

你也可以使用 `Builder` 的 `forceStacking()` 方法强制对话框堆叠其按钮。

---

# 中性操作按钮

除了肯定和否定文本之外，你还可以指定中性文本。中性操作将显示在最左侧。

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

# 回调

要获知用户何时点击了操作按钮，你需要设置一个回调。为此，请使用 `ButtonCallback` 类，并根据需要重写其 `onPositive()`、`onNegative()` 或 `onNeutral()` 方法。这样做的好处是你可以*按需*重写按钮功能，无需为空方法编写占位代码。

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

如果关闭了 `autoDismiss`，那么你必须在这些回调中手动关闭对话框。自动关闭默认是开启的。

---

# 列表对话框

创建列表对话框只需要传入一个字符串数组。回调（`itemsCallback`）也非常简单。

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

如果关闭了 `autoDismiss`，那么你必须在回调中手动关闭对话框。自动关闭默认是开启的。
你可以向 builder 传入 `positiveText()` 或其他操作按钮，强制在列表下方显示操作按钮，但这只在某些特定情况下有用。

---

# 单选列表对话框

单选列表对话框与普通列表对话框几乎完全相同。唯一的区别是，你需要使用 `itemsCallbackSingleChoice` 而不是 `itemsCallback` 来设置回调。这会让对话框在列表项旁边显示单选按钮。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .items(R.array.items)
        .itemsCallbackSingleChoice(-1, new MaterialDialog.ListCallbackSingleChoice() {
            @Override
            public boolean onSelection(MaterialDialog dialog, View view, int which, CharSequence text) {
                /**
                 * 如果你使用了 alwaysCallSingleChoiceCallback()（下文会讨论），
                 * 在这里返回 false 将不允许新选中的单选按钮被真正选中。
                 **/
                return true;
            }
        })
        .positiveText(R.string.choose)
        .show();
```

如果你想预选某个列表项，请在 `itemsCallbackSingleChoice()` 中传入一个大于等于 0 的索引来代替 -1。之后，如果你没有使用自定义适配器，可以通过 `MaterialDialog` 实例上的 `setSelectedIndex(int)` 来更新选中的索引。

如果你没有通过 `positiveText()` 设置正向操作按钮，当用户按下正向操作按钮时，对话框会自动调用单选回调。除非关闭了自动消失功能，否则对话框也会自行关闭。

如果你调用了 `alwaysCallSingleChoiceCallback()`，那么每当用户选择一个列表项时，单选回调都会被调用。

## 单选按钮着色

与操作按钮和 Material 对话框的许多其他元素一样，你可以自定义对话框中单选按钮的颜色。`Builder` 类包含 `widgetColor()`、`widgetColorRes()` 和 `widgetColorAttr()` 方法。它们的方法名和参数注释已经说明了一切。请注意，默认情况下，单选按钮会使用你 Activity 主题中 `colorAccent`（针对 AppCompat）或 `android:colorAccent`（针对 Material 主题）所定义的颜色进行着色。

此外还有一个全局主题属性，如本 README 的全局主题部分所示：`md_widget_color`。

---

# 多选列表对话框

多选列表对话框与普通列表对话框几乎完全相同。唯一的区别是使用 `itemsCallbackMultiChoice` 而不是 `itemsCallback` 来设置回调。这会让对话框在列表项旁边显示复选框，并且回调可以返回多个选中项。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .items(R.array.items)
        .itemsCallbackMultiChoice(null, new MaterialDialog.ListCallbackMultiChoice() {
            @Override
            public boolean onSelection(MaterialDialog dialog, Integer[] which, CharSequence[] text) {
                /**
                 * 如果你使用了 alwaysCallMultiChoiceCallback()（下文会讨论），
                 * 在这里返回 false 将不允许新选中的复选框真正被选中。
                 * 详情请参阅示例项目中的受限多选对话框示例。
                 **/
                 return true;
            }
        })
        .positiveText(R.string.choose)
        .show();
```

如果你想预选某些项，可以在 `itemsCallbackMultiChoice()` 中传入一个索引数组（资源或字面量）来代替 null。之后，如果你没有使用自定义适配器，可以通过 `MaterialDialog` 实例上的 `setSelectedIndices(Integer[])` 来更新选中的索引。

如果你没有使用 `positiveText()` 设置确定操作按钮，当用户按下确定操作按钮时，对话框会自动调用多选回调。除非关闭了自动消失功能，否则对话框也会自行消失。

如果你调用了 `alwaysCallMultiChoiceCallback()`，那么每次用户选中一个项时都会调用多选回调。

## 着色复选框

与操作按钮和 Material 对话框中的许多其他元素一样，你可以自定义对话框复选框的颜色。`Builder` 类包含 `widgetColor()`、`widgetColorRes()` 和 `widgetColorAttr()` 方法。它们的方法名和参数注释已经说明了一切。请注意，默认情况下，复选框会使用你 Activity 主题中 `colorAccent`（适用于 AppCompat）或 `android:colorAccent`（适用于 Material 主题）所定义的颜色。

此外还有一个全局主题属性，如本 README 的全局主题部分所示：`md_widget_color`。

---

# 自定义列表对话框

与 Android 原生对话框一样，你也可以通过 `.adapter()` 传入自己的适配器，来自定义列表的具体工作方式。

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

如果你需要访问 `ListView`，可以使用 `MaterialDialog` 实例：

```java
MaterialDialog dialog = new MaterialDialog.Builder(this)
        ...
        .build();

ListView list = dialog.getListView();
// 对它进行一些操作

dialog.show();
```

请注意，访问 `ListView` 并不需要使用自定义适配器，单选/多选对话框、普通列表对话框等都可以使用它。

---

# 自定义视图

自定义视图非常容易实现。

```java
boolean wrapInScrollView = true;
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .customView(R.layout.custom_view, wrapInScrollView)
        .positiveText(R.string.positive)
        .show();
```

如果 `wrapInScrollView` 为 true，库会自动将你的自定义视图放入一个 ScrollView 中。
这样用户在必要时（小屏幕、内容较长等）就可以滚动你的自定义视图。不过，有些情况下
你可能不希望这种行为。这主要是指你的自定义布局中本身包含 ScrollView 的情况，
包括 ListView、RecyclerView、WebView、GridView 等。示例项目中包含了该参数分别使用
true 和 false 的示例。

当 `wrapInScrollView` 为 true 时，你的自定义视图周围会自动添加内边距。否则，
你需要自行设置合适的内边距值，使内容看起来美观。

## 后续访问

如果你需要在对话框构建之后访问自定义视图中的某个 View，可以使用 `MaterialDialog` 的 `getCustomView()`。当你向 `Builder` 传入布局资源时，这尤其有用，因为对话框会为你处理视图的填充（inflation）。

```java
MaterialDialog dialog = //... initialization via the builder ...
View view = dialog.getCustomView();
```

---

# 字体

默认情况下，Material Dialogs 会对对话框标题和操作按钮使用 `Roboto Medium` 字体，
对内容、列表项等使用 `Roboto Regular` 字体。这是通过使用本库中包含的字体资源实现的，
因此即使在默认使用奇怪手写字体的三星设备上，也会使用这些字体。

如果你想避免这种默认行为，可以在使用 `Builder` 时调用 `disableDefaultFonts()`。
这将使库不再应用 Roboto 和 Roboto Medium 字体，所有内容都会使用常规系统字体。

如果你想显式使用自定义字体，可以在使用 `Builder` 时调用 `typeface(String, String)`。
这会从项目 `assets` 文件夹中的 TTF 文件加载字体。例如，
如果你在 `/src/main/assets/fonts` 中有 `Roboto.ttf` 和 `Roboto-Light.ttf`，则应调用 `typeface("Roboto", "Roboto-Light")`。
注意名称中不使用扩展名。该方法还会通过 `TypefaceHelper` 处理 Typeface 的复用，
你可以在自己的项目中使用它以避免重复分配。如果你想加载其他非 ttf 文件的 Typeface 文件，
可以使用 `typeface(Typeface, Typeface)` Builder 方法。

---

# 获取和设置操作按钮

如果你想在对话框构建并显示之后获取其中某个操作按钮的引用（例如用于启用或禁用按钮）：

```java
MaterialDialog dialog = //... initialization via the builder ...
View negative = dialog.getActionButton(DialogAction.NEGATIVE);
View neutral = dialog.getActionButton(DialogAction.NEUTRAL);
View positive = dialog.getActionButton(DialogAction.POSITIVE);
```

如果你想更新对话框操作按钮的标题（也可以传入字符串资源 ID 来代替字面字符串）：

```java
MaterialDialog dialog = //... initialization via the builder ...
dialog.setActionButton(DialogAction.NEGATIVE, "New Title");
```

---

# 主题定制

在 Lollipop 之前，如果不使用反射和自定义 drawable，基本上无法对 AlertDialog 进行主题定制。
从 KitKat 开始，Android 的配色变得更加中性，但 AlertDialog 的标题和标题分隔线仍然使用 Holo Blue。
Lollipop 改进得更多，默认情况下对话框中除了操作按钮外不再包含任何颜色。这个库让主题定制变得更加简单。

## 基础

默认情况下，Material Dialogs 会根据创建对话框的上下文（context）中获取的 `?android:textColorPrimary` 属性来应用浅色主题或深色主题。如果该颜色是浅色（例如偏白色），它会推测 Activity 使用的是深色主题，从而使用对话框的深色主题。反之亦然，浅色主题同理。你可以通过 `Builder#theme()` 方法手动设置主题：

```java
new MaterialDialog.Builder(this)
        .content("Hi")
        .theme(Theme.DARK)
        .show();
```

或者，你也可以使用全局主题属性，这将在下一节中讨论。全局主题可以避免每次显示对话框时都要反复调用主题设置方法。

## 颜色

使用本库创建的对话框，几乎每个方面都可以着色：

```java
new MaterialDialog.Builder(this)
        .titleColorRes(R.color.material_red_500)
        .contentColor(Color.WHITE) // 注意：直接使用颜色值时没有 'res' 后缀
        .dividerColorRes(R.color.material_pink_500)
        .backgroundColorRes(R.color.material_blue_grey_800)
        .positiveColorRes(R.color.material_red_500)
        .neutralColorRes(R.color.material_red_500)
        .negativeColorRes(R.color.material_red_500)
        .widgetColorRes(R.color.material_red_500)
        .show();
```

这些方法名称大多一目了然。`widgetColor` 方法（在本教程的其他几个章节中有所讨论）适用于进度条、复选框和单选按钮。另请注意，这些方法中的每一个都有 3 种变体：直接设置颜色值、使用颜色资源（color resources）以及使用颜色属性（color attributes）。

## 选择器

主题选择器允许你更改可按压元素的颜色：

```java
new MaterialDialog.Builder(this)
        .btnSelector(R.drawable.custom_btn_selector)
        .btnSelector(R.drawable.custom_btn_selector_primary, DialogAction.POSITIVE)
        .btnSelectorStacked(R.drawable.custom_btn_selector_stacked)
        .listSelector(R.drawable.custom_list_and_stackedbtn_selector)
        .show();
```

第一行 `btnSelector` 设置了一个用于所有操作按钮的 selector drawable。第二行 `btnSelector` 则覆盖了仅用于肯定按钮的 drawable。这使得肯定按钮的 selector 与中性按钮和否定按钮不同。`btnSelectorStacked` 设置了一个在按钮堆叠时使用的 selector drawable，堆叠的原因可能是空间不足以将所有按钮放在一行，或者你在 `Builder` 上使用了 `forceStacked(true)`。`listSelector` 用于列表项，前提是你没有使用自定义 adapter。

***关于使用自定义操作按钮 selector 的重要说明***：请确保你的 selector drawable 像默认的那样引用 inset drawable——这对于正确的操作按钮内边距非常重要。

## Gravity

虽然在对话框中更改元素的重力（gravity）可能不太常见，但这是可以做到的。

```java
new MaterialDialog.Builder(this)
        .titleGravity(GravityEnum.CENTER_HORIZONTAL)
        .contentGravity(GravityEnum.CENTER_HORIZONTAL)
        .btnStackedGravity(GravityEnum.START)
        .itemsGravity(GravityEnum.END)
        .buttonsGravity(GravityEnum.END)
        .show();
```

这些选项的含义一目了然。`titleGravity` 设置对话框标题的重力，`contentGravity` 设置对话框内容的重力，`btnStackedGravity` 设置堆叠操作按钮的重力，`itemsGravity` 设置列表项的重力（当你不使用自定义适配器时）。

关于 `buttonsGravity`，请参考下表：

<table>
<tr>
<td><b>START（默认）</b></td>
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

当没有 Positive 按钮时，Negative 按钮会取代它的位置（CENTER 除外）。

## Material 调色板

要查看符合 Material 设计调色板的颜色，请参阅此页面：http://www.google.com/design/spec/style/color.html#color-color-palette

---

# 全局主题

上一节讨论的大多数主题设置都可以自动应用到你在 Activity 中显示的所有对话框，只要该 Activity 的主题包含以下任意属性：

```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">

    <!--
        将 md_dark_theme 设为 true 后，所有对话框默认使用 Theme.DARK。
    -->
    <item name="md_dark_theme">true</item>

    <!--
        覆盖默认的深色或浅色对话框背景色。
        注意，如果此处使用深色，应将 md_dark_theme 设为 true，
        以确保文本和选择器清晰可见。
    -->
    <item name="md_background_color">#37474F</item>

    <!--
        在所有对话框的标题旁显示一个图标。
    -->
    <item name="md_icon">@drawable/ic_launcher</item>
  
    <!--
        限制图标的最大尺寸。
    -->
    <attr name="md_icon_max_size" format="dimension" />
    
    <!--
        将图标限制为默认最大尺寸（48dp）。
    -->
    <attr name="md_icon_limit_icon_to_default_size" format="boolean" />

    <!--
        默认情况下，标题文本颜色继承自
        ?android:textColorPrimary 系统属性。
    -->
    <item name="md_title_color">#E91E63</item>


    <!--
        默认情况下，内容文本颜色继承自
        ?android:textColorSecondary 系统属性。
    -->
    <item name="md_content_color">#9C27B0</item>


    <!--
        默认情况下，肯定操作按钮的文本颜色继承自
        AppCompat 的 colorAccent 属性或 Material 主题的
        android:colorAccent 属性。
    -->
    <item name="md_positive_color">#673AB7</item>

    <!--
        默认情况下，肯定操作按钮的文本颜色继承自
        AppCompat 的 colorAccent 属性或 Material 主题的
        android:colorAccent 属性。
    -->
    <item name="md_neutral_color">#673AB7</item>

    <!--
        默认情况下，肯定操作按钮的文本颜色继承自
        AppCompat 的 colorAccent 属性或 Material 主题的
        android:colorAccent 属性。
    -->
    <item name="md_negative_color">#673AB7</item>

    <!--
        默认情况下，进度对话框的进度条、复选框和单选按钮的颜色
        继承自 AppCompat 的 colorAccent 属性或
        Material 主题的 android:colorAccent 属性。
    -->
    <item name="md_widget_color">#673AB7</item>

    <!--
        默认情况下，浅色主题下列表项文本为黑色，
        深色主题下为白色。
    -->
    <item name="md_item_color">#9C27B0</item>

    <!--
        覆盖内容可滚动时顶部和底部分隔线使用的颜色。
    -->
    <item name="md_divider_color">#E91E63</item>

    <!--
        覆盖列表项使用的选择器。
    -->
    <item name="md_list_selector">@drawable/selector</item>

    <!--
        覆盖堆叠操作按钮使用的选择器。
    -->
    <item name="md_btn_stacked_selector">@drawable/selector</item>

    <!--
        覆盖肯定操作按钮使用的背景选择器。
    -->
    <item name="md_btn_positive_selector">@drawable/selector</item>

    <!--
        覆盖中性操作按钮使用的背景选择器。
    -->
    <item name="md_btn_neutral_selector">@drawable/selector</item>

    <!--
        覆盖否定操作按钮使用的背景选择器。
    -->
    <item name="md_btn_negative_selector">@drawable/selector</item>
    
    <!-- 
        设置对话框标题显示时使用的 gravity，默认为 start。
        可选值为 start、center 或 end。
    -->
    <item name="md_title_gravity">start</item>
    
    <!-- 
        设置对话框内容显示时使用的 gravity，默认为 start。
        可选值为 start、center 或 end。
    -->
    <item name="md_content_gravity">start</item>
    
    <!--
        设置列表项（不包括自定义适配器）显示时使用的 gravity，默认为 start。
        可选值为 start、center 或 end。
    -->
    <item name="md_items_gravity">start</item>
    
    <!--
        设置对话框操作按钮显示时使用的 gravity，默认为 start。
        
        START（默认）    Neutral     Negative    Positive
        CENTER:          Negative    Neutral     Positive
        END:	           Positive    Negative    Neutral
    -->
    <item name="md_buttons_gravity">start</item>
    
    <!--
        设置堆叠操作按钮显示时使用的 gravity，默认为 end。
        可选值为 start、center 或 end。
    -->
    <item name="md_btnstacked_gravity">end</item>

</style>
```

操作按钮的颜色同样继承自 Material 主题的 `android:colorAccent` 属性，或 AppCompat Material 主题的 `colorAccent` 属性（如示例项目所示）。手动设置颜色将覆盖该行为。

---

# 显示、取消和关闭回调

你可以直接在 `Builder` 上设置显示/取消/关闭监听器，而不必在生成的 `MaterialDialog` 实例上设置：

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

# 输入对话框

输入对话框顾名思义，它通过一个输入框（EditText）从应用程序的用户那里获取输入。如果需要，你还可以在 EditText 上方显示内容。

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

请注意，该对话框会强制显示肯定操作按钮，当它被按下时，输入内容会被提交到回调中。

输入对话框会自动处理 EditText 的焦点获取并显示键盘，以便用户可以立即输入内容。当对话框关闭时，键盘也会自动收起。

## 为 EditText 着色

与操作按钮和 Material 对话框的许多其他元素一样，你可以自定义输入对话框中 `EditText` 的颜色。`Builder` 类包含 `widgetColor()`、`widgetColorRes()` 和 `widgetColorAttr()` 方法。它们的方法名和参数注释已经足够一目了然。请注意，默认情况下，EditText 会使用你 Activity 主题中 `colorAccent`（适用于 AppCompat）或 `android:colorAccent`（适用于 Material 主题）所持有的颜色进行着色。

此外还有一个全局主题属性，如本 README 的“全局主题”部分所示：`md_widget_color`。

---

# 进度对话框

本库允许你显示 Material 设计风格的进度对话框，甚至会使用应用的主题色来为进度条着色（如果你使用 AppCompat 为应用设置主题，或在 Lollipop 上使用 Material 主题）。

## 不确定进度对话框

这将显示带有旋转圆圈的经典进度对话框，请查看示例项目以了解实际效果：

```java
new MaterialDialog.Builder(this)
    .title(R.string.progress_dialog)
    .content(R.string.please_wait)
    .progress(true, 0)
    .show();
```

## 确定型（进度条）进度对话框

如果对话框不是不确定型的，它会显示一个水平进度条，进度会一直增加直到达到最大值。
代码中的注释解释了其作用。

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

请查看示例项目以了解此对话框的实际效果，其中还加入了多线程处理。

## 进度条着色

与操作按钮和 Material 对话框的许多其他元素一样，你可以自定义进度对话框中进度条的颜色。`Builder` 类包含 `widgetColor()`、`widgetColorRes()` 和 `widgetColorAttr()` 方法。它们的方法名和参数注释已经足以说明其用途。请注意，默认情况下，进度条会使用你 Activity 主题中 `colorAccent`（适用于 AppCompat）或 `android:colorAccent`（适用于 Material 主题）所定义的颜色进行着色。

此外还有一个全局主题属性，如本 README 的“全局主题”部分所示：`md_widget_color`。

---

# 偏好设置对话框

Android 的 `EditTextPreference`、`ListPreference` 和 `MultiSelectListPreference` 允许你将偏好设置界面中的设置项与用户通过输入或选择产生的内容关联起来。Material Dialogs 提供了 `MaterialEditTextPreference`、`MaterialListPreference` 和 `MaterialMultiSelectListPreference` 类，可以在你的偏好设置 XML 中使用，从而自动使用 Material 风格的对话框。详情请参阅示例项目。

# Tint Helper

你可以使用 `MDTintHelper` 类来动态为复选框、单选按钮、文本输入框和进度条着色（以解决无法在运行时修改 `styles.xml` 的问题）。该库内部使用它来动态为 UI 元素着色，以匹配你设置的 `widgetColor`。

---

# 其他

如果你不希望在按下操作按钮或用户选择列表项时对话框自动关闭：

```java
MaterialDialog dialog new MaterialDialog.Builder(this)
        // ... other initialization
        .autoDismiss(false)
        .show();
```