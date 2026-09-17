# Material Dialogs

<!-- github-global:langs:start -->
[简体中文](../zh-CN/README.md) | [日本語](../ja/README.md) | [繁體中文](./README.md) | [Español](../es/README.md) | [한국어](../ko/README.md)
<!-- github-global:langs:end -->


![螢幕截圖](https://raw.githubusercontent.com/afollestad/material-dialogs/master/art/mdshowcase.png)

# 範例專案

您可以從此 repo 下載最新的範例 APK：https://github.com/afollestad/material-dialogs/blob/master/sample/sample.apk

也可以在 Google Play 上取得：

<a href="https://play.google.com/store/apps/details?id=com.afollestad.materialdialogssample">
  <img alt="Get it on Google Play"
       src="https://developer.android.com/images/brand/en_generic_rgb_wo_60.png" />
</a>

安裝範例專案是獲取新版本通知的好方法。此外，追蹤（Watching）此 repository 也能讓 GitHub 在我發布新版本時寄送電子郵件通知您。

---

# Gradle 相依性 (jCenter)

在你的 Android 專案中，只需在模組的 `build.gradle` 檔案中加入以下相依性，即可輕鬆參照此函式庫：

```Gradle
dependencies {
    compile 'com.afollestad:material-dialogs:0.7.1.3'
}
```

[ ![Download](https://api.bintray.com/packages/drummer-aidan/maven/material-dialogs/images/download.svg) ](https://bintray.com/drummer-aidan/maven/material-dialogs/_latestVersion)

---

# 最新動態

請參閱專案的 Releases 頁面，以取得各版本及其更新日誌的清單。

### [檢視 Releases](https://github.com/afollestad/material-dialogs/releases)

如果您 Watch 此儲存庫，每次我發布更新時，GitHub 都會寄送電子郵件通知您。

---

# 基本對話框

首先請注意，`MaterialDialog` 繼承自 `DialogBase`，而 `DialogBase` 又繼承自 `AlertDialog`。雖然少數原始方法被刻意標記為棄用且無法運作，但你仍然可以使用諸如 `dismiss()`、`setTitle()`、`setIcon()` 等方法。替代方案將在下方討論。

以下是一個基本範例，模擬你在 Google 的 Material design 設計指南中看到的對話框（網址：http://www.google.com/design/spec/components/dialogs.html#dialogs-usage）。請注意，對於接受字串的方法，你隨時可以改用字串常值或字串資源，顏色資源也是如此（例如 `titleColor` 和 `titleColorRes`）。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .negativeText(R.string.disagree)
        .show();
```

在 Lollipop（API 21+）上，或者如果你使用 AppCompat，Material 對話框會自動將 `positiveColor`（用於正向操作按鈕）與你 styles.xml 佈景主題中的 `colorAccent` 屬性保持一致。

如果內容足夠長，它將變成可捲動的，並會在操作按鈕上方顯示一條分隔線。

---

# 從 AlertDialogs 遷移

如果您正在遷移舊的對話框，可以使用 ```AlertDialogWrapper```。您需要更改 import 並將 ```AlertDialog.Builder``` 替換為 ```AlertDialogWrapper.Builder```：

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

但強烈建議在新的使用情境中使用原始的 ```MaterialDialog``` API。

---

# 顯示圖示

MaterialDialog 支援顯示圖示，就像原生的 AlertDialog 一樣；圖示會顯示在標題的左側。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .icon(R.drawable.icon)
        .show();
```

你可以使用 `limitIconToDefaultSize()`、`maxIconSize(int size)` 或 `maxIconSizeRes(int sizeRes)` 等 Builder 方法來限制圖示的最大尺寸。

---

# 堆疊式動作按鈕

如果你有多個動作按鈕，加起來太寬而無法容納在同一行，對話框會將按鈕堆疊成垂直排列。

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.longer_positive)
        .negativeText(R.string.negative)
        .show();
```

你也可以使用 `Builder` 的 `forceStacking()` 方法強制對話框堆疊其按鈕。

---

# 中性動作按鈕

除了正面與負面文字之外，您還可以指定中性文字。中性動作將會顯示在最左側。

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

# 回呼（Callbacks）

若要知道使用者何時選取了動作按鈕，你需要設定回呼。做法是使用 `ButtonCallback` 類別，並視需求覆寫其 `onPositive()`、`onNegative()` 或 `onNeutral()` 方法。這樣做的好處是你可以*按需選擇*（À la carte）覆寫按鈕功能，因此不需要保留空的 stub 方法。

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

如果關閉了 `autoDismiss`，那麼你必須在這些回呼中手動關閉對話框。自動關閉預設為開啟。

---

# 清單對話框

建立清單對話框只需要傳入一個字串陣列。回呼(`itemsCallback`)也非常簡單。

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

如果關閉了 `autoDismiss`,那麼你必須在回呼中手動關閉對話框。自動關閉預設是開啟的。
你可以將 `positiveText()` 或其他動作按鈕傳給 builder,強制它在清單下方顯示動作按鈕,但這只在某些特定情況下有用。

---

# 單選清單對話框

單選清單對話框與一般清單對話框幾乎完全相同。唯一的差別是使用 `itemsCallbackSingleChoice` 而非 `itemsCallback` 來設定回呼。這會讓對話框在清單項目旁顯示單選按鈕。

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

如果你想預先選取某個項目，請在 `itemsCallbackSingleChoice()` 中傳入 0 或更大的索引來取代 -1。
之後，如果你沒有使用自訂 adapter，可以透過 `MaterialDialog` 實例上的 `setSelectedIndex(int)` 來更新選取的索引。

如果你沒有使用 `positiveText()` 設定正向動作按鈕，當使用者按下正向動作按鈕時，對話框會自動呼叫單選回呼。對話框也會自行關閉，除非已關閉自動關閉功能。

如果你呼叫了 `alwaysCallSingleChoiceCallback()`，則每次使用者選取項目時都會呼叫單選回呼。

## 為單選按鈕上色

如同動作按鈕與 Material 對話框的許多其他元素一樣,你可以自訂對話框中單選按鈕的顏色。`Builder` 類別包含 `widgetColor()`、`widgetColorRes()` 與 `widgetColorAttr()` 方法。它們的名稱與參數註解已足以說明其用途。請注意,預設情況下,單選按鈕會使用你 Activity 佈景主題中 `colorAccent`(適用於 AppCompat)或 `android:colorAccent`(適用於 Material 主題)所指定的顏色。

此外,還有一個全域主題屬性,如本 README 的「全域主題」章節所示:`md_widget_color`。

---

# 多選清單對話框

多選清單對話框與一般清單對話框幾乎完全相同。唯一的差別是使用 `itemsCallbackMultiChoice` 而非 `itemsCallback` 來設定回呼。這會讓對話框在清單項目旁顯示核取方塊，且回呼可以回傳多個選取項目。

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

如果你想預先選取某些項目，請在 `itemsCallbackMultiChoice()` 中以索引陣列（資源或字面值）取代 null。之後，如果你沒有使用自訂 adapter，可以透過 `MaterialDialog` 實例上的 `setSelectedIndices(Integer[])` 來更新選取的索引。

如果你沒有使用 `positiveText()` 設定正向動作按鈕，當使用者按下正向動作按鈕時，對話框會自動呼叫多選回呼。對話框也會自行關閉，除非已關閉自動關閉功能。

如果你呼叫了 `alwaysCallMultiChoiceCallback()`，則每次使用者選取項目時都會呼叫多選回呼。

## 著色核取方塊

如同動作按鈕與 Material 對話框中的許多其他元素，你可以自訂對話框核取方塊的顏色。`Builder` 類別包含 `widgetColor()`、`widgetColorRes()` 與 `widgetColorAttr()` 方法。它們的名稱與參數註解已足以說明其用途。請注意，預設情況下，核取方塊會使用你 Activity 佈景主題中 `colorAccent`（適用於 AppCompat）或 `android:colorAccent`（適用於 Material 主題）所持有的顏色。

此外，還有一個全域主題屬性，如本 README 的全域主題章節所示：`md_widget_color`。

---

# 自訂清單對話框

如同 Android 原生的對話框，你也可以透過 `.adapter()` 傳入自己的 adapter，以完全自訂清單的運作方式。

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

如果你需要存取 `ListView`，可以使用 `MaterialDialog` 實例：

```java
MaterialDialog dialog = new MaterialDialog.Builder(this)
        ...
        .build();

ListView list = dialog.getListView();
// Do something with it

dialog.show();
```

請注意，你不需要使用自訂 adapter 才能存取 `ListView`，無論是單選/多選對話框、一般清單對話框等，它都存在。

# 自訂視圖

自訂視圖非常容易實作。

```java
boolean wrapInScrollView = true;
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .customView(R.layout.custom_view, wrapInScrollView)
        .positiveText(R.string.positive)
        .show();
```

如果 `wrapInScrollView` 為 true,函式庫會自動將你的自訂視圖放在 ScrollView 中。
這樣一來,使用者就能在必要時捲動你的自訂視圖(例如小螢幕、內容過長等情況)。不過,有些情況下
你可能不希望這種行為。這主要是指你的自訂版面配置中本身包含 ScrollView 的情況,
包括 ListView、RecyclerView、WebView、GridView 等。範例專案中包含了此參數設為 true
和 false 的使用範例。

當 `wrapInScrollView` 為 true 時,你的自訂視圖周圍會自動加上內距。否則,
你需要自行設定合適的內距值,讓內容呈現效果良好。

## 後續存取

如果您需要在對話框建立之後存取自訂視圖中的某個 View，可以使用 `MaterialDialog` 的 `getCustomView()`。當您將 layout 資源傳遞給 `Builder` 時，這特別有用，對話框會替您處理視圖的 inflate。

```java
MaterialDialog dialog = //... initialization via the builder ...
View view = dialog.getCustomView();
```

---

# 字型

預設情況下，Material Dialogs 會在對話框標題與動作按鈕使用 `Roboto Medium` 字型，
並在內容、清單項目等使用 `Roboto Regular`。這是透過本函式庫內附的字型資源來達成的，
因此即使在預設使用奇怪手寫字型的 Samsung 裝置上，也會使用這些字型。

如果你想避免這個預設行為，可以在使用 `Builder` 時呼叫 `disableDefaultFonts()`。
這樣函式庫就不會套用 Roboto 與 Roboto Medium 字型，所有內容都會使用一般的系統字型。

如果你想明確使用自訂字型，可以在使用 `Builder` 時呼叫 `typeface(String, String)`。
這會從你專案 `assets` 資料夾中的 TTF 檔案載入字型。例如，
如果你在 `/src/main/assets/fonts` 中有 `Roboto.ttf` 和 `Roboto-Light.ttf`，就呼叫 `typeface("Roboto", "Roboto-Light")`。
注意名稱中不包含副檔名。此方法也會透過 `TypefaceHelper` 處理 Typeface 的回收，
你也可以在自己的專案中使用它來避免重複的配置。如果你想載入非 ttf 檔案的其他 Typeface 檔案，
可以使用 `typeface(Typeface, Typeface)` 這個 Builder 方法。

---

# 取得與設定動作按鈕

如果你想在對話框建立並顯示之後，取得其中一個對話框動作按鈕的參考（例如：啟用或停用按鈕）：

```java
MaterialDialog dialog = //... initialization via the builder ...
View negative = dialog.getActionButton(DialogAction.NEGATIVE);
View neutral = dialog.getActionButton(DialogAction.NEUTRAL);
View positive = dialog.getActionButton(DialogAction.POSITIVE);
```

如果你想更新對話框動作按鈕的標題（也可以傳入字串資源 ID 來取代字面字串）：

```java
MaterialDialog dialog = //... initialization via the builder ...
dialog.setActionButton(DialogAction.NEGATIVE, "New Title");
```

---

# 主題化

在 Lollipop 之前，若不使用反射與自訂 drawable，幾乎不可能為 AlertDialog 套用主題。
自 KitKat 起，Android 的配色變得更加中性，但 AlertDialog 的標題與標題分隔線仍持續使用 Holo 藍色。
Lollipop 更進一步改善，對話框預設除了動作按鈕外不再使用任何顏色。這個函式庫讓主題化變得更加容易。

## 基本概念

預設情況下，Material Dialogs 會根據建立對話框的 context 中取得的 `?android:textColorPrimary` 屬性，套用淺色或深色主題。如果該顏色偏淺（例如偏白色），它會推測 Activity 正在使用深色主題，並使用對話框的深色主題；反之則使用淺色主題。你也可以透過 `Builder#theme()` 方法手動設定主題：

```java
new MaterialDialog.Builder(this)
        .content("Hi")
        .theme(Theme.DARK)
        .show();
```

或者，你可以使用全域主題屬性，這將在下一節中說明。全域主題可以避免每次顯示對話框時都要不斷呼叫主題設定方法。

## 顏色

使用此函式庫建立的對話框，幾乎每個層面都可以設定顏色：

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

這些名稱大多一目了然。`widgetColor` 方法在本教學的其他幾個章節中有討論過，適用於進度條、核取方塊與單選按鈕。另請注意，這些方法各有 3 種變化形式，分別用於直接設定顏色、使用顏色資源，以及使用顏色屬性。

## 選擇器

主題選擇器可讓你變更可按壓元件的顏色：

```java
new MaterialDialog.Builder(this)
        .btnSelector(R.drawable.custom_btn_selector)
        .btnSelector(R.drawable.custom_btn_selector_primary, DialogAction.POSITIVE)
        .btnSelectorStacked(R.drawable.custom_btn_selector_stacked)
        .listSelector(R.drawable.custom_list_and_stackedbtn_selector)
        .show();
```

第一行 `btnSelector` 設定用於所有動作按鈕的選擇器 drawable。第二行 `btnSelector` 則覆寫僅用於正面按鈕的 drawable。這會使正面按鈕的選擇器與中性按鈕和負面按鈕不同。`btnSelectorStacked` 設定當按鈕變為堆疊排列時所使用的選擇器 drawable——無論是因為空間不足以將所有按鈕排在同一行，或是因為你在 `Builder` 上使用了 `forceStacked(true)`。`listSelector` 用於清單項目，適用於你「沒有」使用自訂 adapter 的情況。

***關於使用自訂動作按鈕選擇器的重要注意事項***：請確保你的選擇器 drawable 參照 inset drawable，就像預設的那樣——這對於正確的動作按鈕內距非常重要。

## Gravity

雖然你可能不太會想改變對話框中元素的重力（gravity），但這是可行的。

```java
new MaterialDialog.Builder(this)
        .titleGravity(GravityEnum.CENTER_HORIZONTAL)
        .contentGravity(GravityEnum.CENTER_HORIZONTAL)
        .btnStackedGravity(GravityEnum.START)
        .itemsGravity(GravityEnum.END)
        .buttonsGravity(GravityEnum.END)
        .show();
```

這些方法都相當直觀。`titleGravity` 設定對話框標題的重力，`contentGravity` 設定對話框內容的重力，`btnStackedGravity` 設定堆疊動作按鈕的重力，`itemsGravity` 設定清單項目的重力（當你**沒有**使用自訂 adapter 時）。

關於 `buttonsGravity`，請參考以下說明：

<table>
<tr>
<td><b>START（預設）</b></td>
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

在沒有 positive 按鈕的情況下，negative 按鈕會取代它的位置，但 CENTER 除外。

## Material 調色盤

若要查看符合 Material 設計調色盤的顏色，請參閱此頁面：http://www.google.com/design/spec/style/color.html#color-color-palette

---

# 全域主題設定

上述章節討論的大部分主題設定，都可以自動套用到你從某個 Activity 顯示的所有對話框，只要該 Activity 的主題包含以下任一屬性：

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

動作按鈕的顏色同樣衍生自 Material 主題的 `android:colorAccent` 屬性，或 AppCompat Material 主題的 `colorAccent` 屬性（如範例專案所示）。手動設定顏色將會覆蓋此行為。

---

# 顯示、取消與關閉回呼

你可以直接在 `Builder` 上設定 show/cancel/dismiss 監聽器，而不必在產生的 `MaterialDialog` 實例上設定：

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

# 輸入對話框

輸入對話框相當容易理解，它透過輸入欄位（EditText）從應用程式的使用者取得輸入。如有需要，你也可以在 EditText 上方顯示內容。

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

請注意，對話框會強制顯示確認按鈕，當它被按下時，輸入內容會提交至 callback。

輸入對話框會自動處理 EditText 的焦點並顯示鍵盤，讓使用者可以立即輸入內容。當對話框關閉時，鍵盤也會自動收起。

## 為 EditText 上色

如同動作按鈕與 Material 對話框的許多其他元素，你可以自訂輸入對話框中 `EditText` 的顏色。`Builder` 類別包含 `widgetColor()`、`widgetColorRes()` 與 `widgetColorAttr()` 方法。它們的名稱與參數註解已足以說明其用途。請注意，預設情況下，EditText 會使用你 Activity 佈景主題中 `colorAccent`（適用於 AppCompat）或 `android:colorAccent`（適用於 Material 主題）所持有的顏色來上色。

此外還有一個全域主題屬性，如本 README 的「全域主題」章節所示：`md_widget_color`。

---

# 進度對話框

此函式庫讓你能夠顯示 Material 設計風格的進度對話框，甚至會使用你應用程式的主題色（accent color）來為進度條上色（如果你使用 AppCompat 為應用程式設定主題，或在 Lollipop 上使用 Material 主題）。

## 不確定進度對話框

這會顯示帶有旋轉圓圈的傳統進度對話框，請參閱範例專案以了解實際效果：

```java
new MaterialDialog.Builder(this)
    .title(R.string.progress_dialog)
    .content(R.string.please_wait)
    .progress(true, 0)
    .show();
```

## 確定型（進度條）進度對話框

如果對話框不是不確定型（indeterminate），它會顯示一條水平進度條，進度會一直增加到最大值。
程式碼中的註解說明了它的作用。

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

請參閱範例專案，查看此對話框的實際運作，其中還加入了執行緒處理。

## 為進度條上色

如同動作按鈕與 Material 對話框中的許多其他元素,你可以自訂進度對話框中進度條的顏色。`Builder` 類別包含 `widgetColor()`、`widgetColorRes()` 與 `widgetColorAttr()` 方法。它們的名稱與參數註解已足以說明其用途。請注意,預設情況下,進度條會使用你 Activity 佈景主題中 `colorAccent`(適用於 AppCompat)或 `android:colorAccent`(適用於 Material 主題)所持有的顏色。

此外,還有一個全域主題屬性,如本 README 的全域主題章節所示:`md_widget_color`。

---

# 偏好設定對話框

Android 的 `EditTextPreference`、`ListPreference` 與 `MultiSelectListPreference` 允許你將偏好設定活動的設定與透過輸入或選擇所接收的使用者輸入建立關聯。Material Dialogs 提供了 `MaterialEditTextPreference`、`MaterialListPreference` 與 `MaterialMultiSelectListPreference` 類別,可以在你的偏好設定 XML 中使用,以自動採用 Material 主題的對話框。詳細資訊請參閱範例專案。

---

# Tint Helper

您可以使用 `MDTintHelper` 類別來為核取方塊、單選按鈕、編輯文字框與進度條動態著色(以解決無法在執行階段更改 `styles.xml` 的問題)。此類別在函式庫中用於動態著色 UI 元素,以符合您設定的 `widgetColor`。

---

# 其他

如果您不希望在按下動作按鈕或使用者選取清單項目時自動關閉對話框：

```java
MaterialDialog dialog new MaterialDialog.Builder(this)
        // ... other initialization
        .autoDismiss(false)
        .show();
```