# Material Dialogs

<!-- github-global:langs:start -->
[简体中文](../zh-CN/README.md) | [日本語](../ja/README.md) | [繁體中文](../zh-TW/README.md) | [Español](../es/README.md) | [한국어](./README.md)
<!-- github-global:langs:end -->


![스크린샷](https://raw.githubusercontent.com/afollestad/material-dialogs/master/art/mdshowcase.png)

# 샘플 프로젝트

이 저장소에서 최신 샘플 APK를 여기서 다운로드할 수 있습니다: https://github.com/afollestad/material-dialogs/blob/master/sample/sample.apk

Google Play에서도 이용할 수 있습니다:

<a href="https://play.google.com/store/apps/details?id=com.afollestad.materialdialogssample">
  <img alt="Get it on Google Play"
       src="https://developer.android.com/images/brand/en_generic_rgb_wo_60.png" />
</a>

샘플 프로젝트를 설치해 두면 새 릴리스 알림을 받는 좋은 방법이 됩니다. 물론 이 저장소를 Watching하면 GitHub가 제가 릴리스를 게시할 때마다 이메일로 알려드립니다.

---

# Gradle 의존성 (jCenter)

모듈의 `build.gradle` 파일에 다음 의존성을 추가하여 Android 프로젝트에서 이 라이브러리를 쉽게 참조할 수 있습니다:

```Gradle
dependencies {
    compile 'com.afollestad:material-dialogs:0.7.1.3'
}
```

[ ![Download](https://api.bintray.com/packages/drummer-aidan/maven/material-dialogs/images/download.svg) ](https://bintray.com/drummer-aidan/maven/material-dialogs/_latestVersion)

---

# 새 소식

버전 목록과 변경 로그는 프로젝트의 Releases 페이지를 참고하세요.

### [릴리스 보기](https://github.com/afollestad/material-dialogs/releases)

이 저장소를 Watch하면, 업데이트를 게시할 때마다 GitHub에서 이메일을 보내드립니다.

---

# 기본 다이얼로그

먼저, `MaterialDialog`는 `AlertDialog`를 확장하는 `DialogBase`를 확장한다는 점에 유의하세요. 기본 제공 메서드 중 아주 소수는 의도적으로 deprecated 되어 동작하지 않지만, `dismiss()`, `setTitle()`, `setIcon()` 등의 메서드에는 접근할 수 있습니다. 대안은 아래에서 설명합니다.

Google의 Material 디자인 가이드라인(여기: http://www.google.com/design/spec/components/dialogs.html#dialogs-usage)에서 보이는 다이얼로그를 흉내 내는 기본 예제입니다. 문자열을 받는 메서드에는 항상 리터럴 문자열과 문자열 리소스를 대신 사용할 수 있으며, 색상 리소스도 마찬가지입니다(예: `titleColor`와 `titleColorRes`).

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .negativeText(R.string.disagree)
        .show();
```

Lollipop(API 21+)이거나 AppCompat을 사용하는 경우, Material 다이얼로그는 `positiveColor`(positive 액션 버튼에 사용됨)를 styles.xml 테마의 `colorAccent` 속성에 자동으로 맞춥니다.

콘텐츠가 충분히 길면 스크롤 가능해지고, 액션 버튼 위에 구분선이 표시됩니다.

---

# AlertDialog에서 마이그레이션하기

기존 다이얼로그를 마이그레이션하는 경우 ```AlertDialogWrapper```를 사용할 수 있습니다. import를 변경하고 ```AlertDialog.Builder```를 ```AlertDialogWrapper.Builder```로 교체하면 됩니다:

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

하지만 새로운 용도로는 원래의 ```MaterialDialog``` API를 사용하는 것이 강력히 권장됩니다.

---

# 아이콘 표시하기

MaterialDialog는 기본 AlertDialog와 마찬가지로 아이콘 표시를 지원하며, 아이콘은 제목 왼쪽에 표시됩니다.

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .icon(R.drawable.icon)
        .show();
```

`limitIconToDefaultSize()`, `maxIconSize(int size)`, 또는 `maxIconSizeRes(int sizeRes)` Builder 메서드를 사용하여 아이콘의 최대 크기를 제한할 수 있습니다.

---

# 세로로 쌓이는 액션 버튼

여러 개의 액션 버튼이 함께 있어서 한 줄에 들어가기에는 너무 넓을 경우, 다이얼로그는 버튼들을 세로 방향으로 쌓아서 표시합니다.

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.longer_positive)
        .negativeText(R.string.negative)
        .show();
```

또한 `Builder`의 `forceStacking()` 메서드를 사용하여 다이얼로그가 버튼을 쌓도록 강제할 수도 있습니다.

---

# 중립 액션 버튼

긍정 및 부정 텍스트 외에 중립 텍스트를 지정할 수 있습니다. 중립 액션은 맨 왼쪽에 표시됩니다.

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

# 콜백

사용자가 액션 버튼을 선택하는 시점을 알고 싶다면 콜백을 설정합니다. 이를 위해 `ButtonCallback` 클래스를 사용하고 필요에 따라 `onPositive()`, `onNegative()`, 또는 `onNeutral()` 메서드를 오버라이드하면 됩니다. 이 방식의 장점은 버튼 기능을 *À la carte* 방식으로 오버라이드할 수 있어 빈 메서드를 스텁으로 만들 필요가 없다는 것입니다.

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

`autoDismiss`가 꺼져 있다면, 이 콜백들에서 다이얼로그를 수동으로 닫아야 합니다. 자동 닫기(auto dismiss)는 기본적으로 활성화되어 있습니다.

---

# 리스트 다이얼로그

리스트 다이얼로그를 만드는 것은 문자열 배열을 전달하기만 하면 됩니다. 콜백(`itemsCallback`) 또한 매우 간단합니다.

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

`autoDismiss`가 꺼져 있다면, 콜백에서 직접 다이얼로그를 닫아야(dismiss) 합니다. 자동 닫기는 기본적으로 활성화되어 있습니다. 빌더에 `positiveText()`나 다른 액션 버튼을 전달하여 리스트 아래에 액션 버튼을 표시하도록 강제할 수 있지만, 이는 일부 특정한 경우에만 유용합니다.

# 단일 선택 리스트 다이얼로그

단일 선택 리스트 다이얼로그는 일반 리스트 다이얼로그와 거의 동일합니다. 유일한 차이점은 `itemsCallback` 대신 `itemsCallbackSingleChoice`를 사용하여 콜백을 설정한다는 것입니다. 이는 다이얼로그가 리스트 항목 옆에 라디오 버튼을 표시하도록 신호를 보냅니다.

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

항목을 미리 선택하려면 `itemsCallbackSingleChoice()`에서 -1 대신 0 이상의 인덱스를 전달하세요. 이후 커스텀 어댑터를 사용하지 않는 경우, `MaterialDialog` 인스턴스에서 `setSelectedIndex(int)`를 사용하여 선택된 인덱스를 업데이트할 수 있습니다.

`positiveText()`를 사용하여 긍정 액션 버튼을 설정하지 않으면, 사용자가 긍정 액션 버튼을 누를 때 다이얼로그가 자동으로 단일 선택 콜백을 호출합니다. 또한 자동 닫기(auto dismiss)가 꺼져 있지 않은 한 다이얼로그는 스스로 닫힙니다.

`alwaysCallSingleChoiceCallback()`을 호출하면, 사용자가 항목을 선택할 때마다 단일 선택 콜백이 호출됩니다.

## 라디오 버튼 색상 지정

액션 버튼과 Material 다이얼로그의 많은 다른 요소들처럼, 다이얼로그의 라디오 버튼 색상을 커스터마이즈할 수 있습니다. `Builder` 클래스에는 `widgetColor()`, `widgetColorRes()`, `widgetColorAttr()` 메서드가 포함되어 있습니다. 이름과 파라미터 어노테이션만 봐도 용도를 쉽게 알 수 있습니다. 기본적으로 라디오 버튼은 Activity 테마의 `colorAccent`(AppCompat의 경우) 또는 `android:colorAccent`(Material 테마의 경우)에 설정된 색상으로 지정된다는 점에 유의하세요.

또한 이 README의 Global Theming 섹션에서 보여주는 것처럼 전역 테마 속성도 있습니다: `md_widget_color`.

---

# 다중 선택 리스트 다이얼로그

다중 선택 리스트 다이얼로그는 일반 리스트 다이얼로그와 거의 동일합니다. 유일한 차이점은 `itemsCallback` 대신 `itemsCallbackMultiChoice`를 사용하여 콜백을 설정한다는 것입니다. 이렇게 하면 다이얼로그가 리스트 항목 옆에 체크박스를 표시하며, 콜백은 여러 개의 선택 항목을 반환할 수 있습니다.

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .items(R.array.items)
        .itemsCallbackMultiChoice(null, new MaterialDialog.ListCallbackMultiChoice() {
            @Override
            public boolean onSelection(MaterialDialog dialog, Integer[] which, CharSequence[] text) {
                /**
                 * 아래에서 설명할 alwaysCallMultiChoiceCallback()을 사용하는 경우,
                 * 여기서 false를 반환하면 새로 선택한 체크박스가 실제로 선택되지 않습니다.
                 * 자세한 내용은 샘플 프로젝트의 제한된 다중 선택 다이얼로그 예제를 참조하세요.
                 **/
                 return true;
            }
        })
        .positiveText(R.string.choose)
        .show();
```

항목을 미리 선택하려면 `itemsCallbackMultiChoice()`에서 null 대신 인덱스 배열(리소스 또는 리터럴)을 전달하세요. 이후 커스텀 어댑터를 사용하지 않는 경우, `MaterialDialog` 인스턴스의 `setSelectedIndices(Integer[])`를 사용하여 선택된 인덱스를 업데이트할 수 있습니다.

`positiveText()`를 사용하여 긍정 액션 버튼을 설정하지 않으면, 사용자가 긍정 액션 버튼을 누를 때 다이얼로그가 자동으로 다중 선택 콜백을 호출합니다. 또한 자동 닫기(auto dismiss)가 꺼져 있지 않은 한 다이얼로그는 스스로 닫힙니다.

`alwaysCallMultiChoiceCallback()`을 호출하면, 사용자가 항목을 선택할 때마다 다중 선택 콜백이 호출됩니다.

## 체크박스 색상 지정

Material 대화상자의 액션 버튼 및 기타 많은 요소들과 마찬가지로, 대화상자의 체크박스 색상을 커스터마이즈할 수 있습니다. `Builder` 클래스에는 `widgetColor()`, `widgetColorRes()`, `widgetColorAttr()` 메서드가 포함되어 있습니다. 이름과 파라미터 주석만 봐도 기능을 쉽게 알 수 있습니다. 기본적으로 체크박스는 Activity 테마의 `colorAccent`(AppCompat의 경우) 또는 `android:colorAccent`(Material 테마의 경우)에 지정된 색상으로 표시됩니다.

또한 이 README의 Global Theming 섹션에 표시된 것처럼 전역 테마 속성 `md_widget_color`도 있습니다.

---

# 사용자 지정 리스트 다이얼로그

Android의 기본 다이얼로그와 마찬가지로, `.adapter()`를 통해 자신만의 어댑터를 전달하여 리스트가 동작하는 방식을 원하는 대로 정확하게 커스터마이즈할 수 있습니다.

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

`ListView`에 접근해야 하는 경우, `MaterialDialog` 인스턴스를 사용할 수 있습니다:

```java
MaterialDialog dialog = new MaterialDialog.Builder(this)
        ...
        .build();

ListView list = dialog.getListView();
// Do something with it

dialog.show();
```

`ListView`에 접근하기 위해 반드시 커스텀 어댑터를 사용할 필요는 없다는 점에 유의하세요. `ListView`는 단일/다중 선택 다이얼로그, 일반 리스트 다이얼로그 등에서도 사용할 수 있습니다.

---

# 커스텀 뷰

커스텀 뷰는 매우 쉽게 구현할 수 있습니다.

```java
boolean wrapInScrollView = true;
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .customView(R.layout.custom_view, wrapInScrollView)
        .positiveText(R.string.positive)
        .show();
```

`wrapInScrollView`이 true이면, 라이브러리가 여러분의 커스텀 뷰를 자동으로 ScrollView 안에 넣어줍니다.
이를 통해 사용자는 필요한 경우(작은 화면, 긴 콘텐츠 등) 커스텀 뷰를 스크롤할 수 있습니다. 하지만 이 동작을
원하지 않는 경우도 있습니다. 이는 주로 커스텀 레이아웃 안에 ScrollView가 포함되는 경우, 즉 ListView,
RecyclerView, WebView, GridView 등을 사용하는 경우에 해당합니다. 샘플 프로젝트에는 이 파라미터에 true와
false를 각각 사용하는 예제가 포함되어 있습니다.

`wrapInScrollView`이 true이면 커스텀 뷰 주위에 자동으로 패딩이 적용됩니다. 그렇지 않은 경우에는
콘텐츠에 어울리는 패딩 값을 직접 지정해야 합니다.

## 나중에 접근하기

다이얼로그가 빌드된 후 커스텀 뷰 내의 View에 접근해야 하는 경우, `MaterialDialog`의 `getCustomView()`를
사용할 수 있습니다. `Builder`에 레이아웃 리소스를 전달한 경우 특히 유용하며, 다이얼로그가 뷰 인플레이션을
대신 처리해 줍니다.

```java
MaterialDialog dialog = //... initialization via the builder ...
View view = dialog.getCustomView();
```

---

# 서체 (Typefaces)

기본적으로 Material Dialogs는 다이얼로그 제목과 액션 버튼에 `Roboto Medium` 폰트를 사용하고,
콘텐츠, 리스트 아이템 등에는 `Roboto Regular`를 사용합니다. 이는 이 라이브러리에 포함된 폰트 에셋을 통해 수행되므로,
기본적으로 이상한 손글씨 서체를 사용하는 삼성 기기에서도 이 폰트들이 사용됩니다.

이 기본 동작을 피하고 싶다면, `Builder`를 사용할 때 `disableDefaultFonts()`를 호출하면 됩니다. 이렇게 하면
라이브러리가 Roboto 및 Roboto Medium 폰트를 적용하지 않고, 모든 것이 일반 시스템 폰트를 사용하게 됩니다.

명시적으로 커스텀 폰트를 사용하고 싶다면, `Builder`를 사용할 때 `typeface(String, String)`를 호출하면 됩니다.
이는 프로젝트의 `assets` 폴더에 있는 TTF 파일에서 폰트를 가져옵니다. 예를 들어,
`/src/main/assets/fonts`에 `Roboto.ttf`와 `Roboto-Light.ttf`가 있다면 `typeface("Roboto", "Roboto-Light")`를 호출하면 됩니다.
이름에 확장자는 사용하지 않는다는 점에 유의하세요. 이 메서드는 `TypefaceHelper`를 통해 Typeface를 재활용하며,
이를 여러분의 프로젝트에서 사용하여 중복 할당을 피할 수 있습니다. ttf 파일이 아닌 다른 Typeface 파일을 로드하고 싶다면,
`typeface(Typeface, Typeface)` Builder 메서드를 사용할 수 있습니다.

---

# 액션 버튼 가져오기 및 설정

다이얼로그가 빌드되고 표시된 후 다이얼로그 액션 버튼 중 하나에 대한 참조를 가져오려면(예: 버튼 활성화 또는 비활성화):

```java
MaterialDialog dialog = //... 빌더를 통한 초기화 ...
View negative = dialog.getActionButton(DialogAction.NEGATIVE);
View neutral = dialog.getActionButton(DialogAction.NEUTRAL);
View positive = dialog.getActionButton(DialogAction.POSITIVE);
```

다이얼로그 액션 버튼의 제목을 업데이트하려면(리터럴 문자열 대신 문자열 리소스 ID를 전달할 수도 있습니다):

```java
MaterialDialog dialog = //... 빌더를 통한 초기화 ...
dialog.setActionButton(DialogAction.NEGATIVE, "New Title");
```

---

# 테마 적용

Lollipop 이전에는 리플렉션과 커스텀 drawable을 사용하지 않고서는 AlertDialog의 테마를 적용하는 것이 사실상 불가능했습니다. KitKat부터 Android는 색상 면에서 더 중립적으로 변했지만, AlertDialog는 여전히 제목과 제목 구분선에 Holo Blue를 사용했습니다. Lollipop은 한층 더 개선되어, 기본적으로 다이얼로그에는 액션 버튼 외에 색상이 사용되지 않습니다. 이 라이브러리는 테마 적용을 더욱 쉽게 만들어 줍니다.

## 기본 사항

기본적으로 Material Dialogs는 다이얼로그를 생성하는 컨텍스트에서 가져온 `?android:textColorPrimary` 속성을 기반으로 라이트 테마 또는 다크 테마를 적용합니다. 색상이 밝은 경우(예: 흰색에 가까운 경우) Activity가 다크 테마를 사용한다고 판단하여 다이얼로그의 다크 테마를 사용합니다. 라이트 테마의 경우 그 반대로 동작합니다. `Builder#theme()` 메서드를 통해 사용할 테마를 직접 설정할 수 있습니다:

```java
new MaterialDialog.Builder(this)
        .content("Hi")
        .theme(Theme.DARK)
        .show();
```

또는 아래 섹션에서 설명할 전역 테마 속성을 사용할 수도 있습니다. 전역 테마를 사용하면 표시하는 모든 다이얼로그마다 일일이 테마 설정자를 호출할 필요가 없습니다.

## 색상

이 라이브러리로 생성한 다이얼로그의 거의 모든 요소에 색상을 지정할 수 있습니다:

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

메서드 이름은 대부분 그 자체로 설명이 됩니다. 이 튜토리얼의 다른 여러 섹션에서 다루는 `widgetColor` 메서드는 프로그레스 바, 체크박스, 라디오 버튼에 적용됩니다. 또한 이 메서드들은 각각 색상을 직접 설정하는 방식, 색상 리소스를 사용하는 방식, 색상 속성을 사용하는 방식의 3가지 변형이 있다는 점에 유의하세요.

## 셀렉터(Selectors)

테마 셀렉터를 사용하면 누를 수 있는 요소들의 색상을 변경할 수 있습니다:

```java
new MaterialDialog.Builder(this)
        .btnSelector(R.drawable.custom_btn_selector)
        .btnSelector(R.drawable.custom_btn_selector_primary, DialogAction.POSITIVE)
        .btnSelectorStacked(R.drawable.custom_btn_selector_stacked)
        .listSelector(R.drawable.custom_list_and_stackedbtn_selector)
        .show();
```

첫 번째 `btnSelector` 줄은 모든 액션 버튼에 사용되는 셀렉터 drawable을 설정합니다. 두 번째 `btnSelector`
줄은 positive 버튼에만 사용되는 drawable을 덮어씁니다. 그 결과 positive 버튼이 neutral 및 negative 버튼과
다른 셀렉터를 갖게 됩니다. `btnSelectorStacked`는 버튼들이 세로로 쌓일 때 사용되는 셀렉터 drawable을 설정하며, 버튼들이 쌓이는 경우는 한 줄에 모두 표시할 공간이 부족하거나 `Builder`에서 `forceStacked(true)`를 사용한 경우입니다. `listSelector`는 커스텀 어댑터를 사용하지 않을 때 리스트 아이템에 사용됩니다.

***커스텀 액션 버튼 셀렉터 사용과 관련된 중요한 참고 사항***: 기본 셀렉터처럼 셀렉터 drawable이 inset drawable을 참조하도록 하세요 - 이는 올바른 액션 버튼 패딩을 위해 중요합니다.

## Gravity

대화상자 내 요소의 gravity를 변경하고 싶은 경우는 드물겠지만, 가능합니다.

```java
new MaterialDialog.Builder(this)
        .titleGravity(GravityEnum.CENTER_HORIZONTAL)
        .contentGravity(GravityEnum.CENTER_HORIZONTAL)
        .btnStackedGravity(GravityEnum.START)
        .itemsGravity(GravityEnum.END)
        .buttonsGravity(GravityEnum.END)
        .show();
```

이름만 봐도 직관적으로 이해할 수 있습니다. `titleGravity`는 대화상자 제목의 gravity를 설정하고, `contentGravity`는 대화상자 내용의 gravity를 설정하며, `btnStackedGravity`는 세로로 쌓인(stacked) 액션 버튼의 gravity를 설정하고, `itemsGravity`는 리스트 항목의 gravity를 설정합니다(커스텀 어댑터를 사용하지 않는 경우).

`buttonsGravity`에 대해서는 다음을 참고하세요:

<table>
<tr>
<td><b>START (기본값)</b></td>
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

Positive 버튼이 없을 경우, CENTER를 제외하고는 Negative 버튼이 그 자리를 대신합니다.

## Material 팔레트

Material 디자인 팔레트에 어울리는 색상을 보려면 다음 페이지를 참고하세요: http://www.google.com/design/spec/style/color.html#color-color-palette

---

# 글로벌 테마 설정

위 섹션에서 논의한 대부분의 테마 관련 사항은, 다음 속성 중 하나를 포함하는 테마를 가진 Activity에서 표시하는 모든 다이얼로그에 자동으로 적용될 수 있습니다:

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

액션 버튼 색상 역시 Material 테마의 `android:colorAccent` 속성 또는 샘플 프로젝트에서 볼 수 있는 AppCompat Material 테마의 `colorAccent` 속성에서 파생됩니다. 색상을 수동으로 설정하면 이 동작이 재정의됩니다.

---

# 표시, 취소 및 해제 콜백

결과로 생성되는 `MaterialDialog` 인스턴스가 아닌 `Builder`에서 직접 표시/취소/해제 리스너를 설정할 수 있습니다:

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

# 입력 다이얼로그

입력 다이얼로그는 이름 그대로 사용자로부터 입력 필드(EditText)를 통해 입력을 받는 다이얼로그입니다. 원한다면 EditText 위에 콘텐츠를 표시할 수도 있습니다.

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

다이얼로그는 긍정(positive) 액션 버튼을 항상 표시하도록 강제하며, 해당 버튼이 눌리면 입력이 콜백으로 전달됩니다.

입력 다이얼로그는 EditText에 포커스를 맞추고 키보드를 표시하는 과정을 자동으로 처리하여, 사용자가 즉시 입력을 시작할 수 있도록 합니다. 다이얼로그가 닫히면 키보드도 자동으로 사라집니다.

## EditText 색상 지정하기

액션 버튼 및 Material dialog의 다른 많은 요소들과 마찬가지로, 입력 다이얼로그의 `EditText` 색상을 커스터마이즈할 수 있습니다. `Builder` 클래스에는 `widgetColor()`, `widgetColorRes()`, `widgetColorAttr()` 메서드가 포함되어 있습니다. 이름과 파라미터 주석만 봐도 용도를 쉽게 알 수 있습니다. 기본적으로 EditText는 Activity 테마의 `colorAccent`(AppCompat의 경우) 또는 `android:colorAccent`(Material 테마의 경우)에 지정된 색상으로 표시된다는 점에 유의하세요.

이 README의 Global Theming 섹션에서 설명하는 것처럼 전역 테마 속성도 있습니다: `md_widget_color`.

---

# 진행 다이얼로그

이 라이브러리를 사용하면 Material 디자인의 진행 다이얼로그를 표시할 수 있으며, 앱의 강조 색상(accent color)을 사용하여 진행 막대(progress bar)를 색칠할 수도 있습니다 (AppCompat으로 앱 테마를 지정하거나 Lollipop의 Material 테마를 사용하는 경우).

## 결정되지 않은 진행 다이얼로그 (Indeterminate Progress Dialogs)

회전하는 원이 있는 클래식한 진행 다이얼로그를 표시합니다. 실제 동작은 샘플 프로젝트를 참고하세요:

```java
new MaterialDialog.Builder(this)
    .title(R.string.progress_dialog)
    .content(R.string.please_wait)
    .progress(true, 0)
    .show();
```

## 결정적(Seek Bar) 진행 다이얼로그

다이얼로그가 비(非)결정적(indeterminate)이 아니면, 최대값까지 증가하는 가로형 진행 바가 표시됩니다.
코드의 주석이 이 기능이 무엇인지 설명합니다.

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

스레딩을 추가한 이 다이얼로그의 실제 동작은 샘플 프로젝트를 참고하세요.

## 진행 바 색상 지정하기

액션 버튼과 Material 다이얼로그의 많은 다른 요소들처럼, 진행 다이얼로그의 진행 바 색상도 커스터마이즈할 수 있습니다. `Builder` 클래스에는 `widgetColor()`, `widgetColorRes()`, `widgetColorAttr()` 메서드가 포함되어 있습니다. 이름과 파라미터 어노테이션만 봐도 용도를 쉽게 알 수 있습니다. 기본적으로 진행 바는 Activity 테마의 `colorAccent` (AppCompat의 경우) 또는 `android:colorAccent` (Material 테마의 경우)에 설정된 색상으로 지정된다는 점에 유의하세요.

또한 이 README의 Global Theming 섹션에서 보여지는 것처럼 전역 테마 속성도 있습니다: `md_widget_color`.

---

# 환경설정 대화상자

Android의 `EditTextPreference`, `ListPreference`, `MultiSelectListPreference`는 환경설정 액티비티의 설정을 입력 또는 선택을 통해 받은 사용자 입력과 연결할 수 있게 해줍니다. Material Dialogs는 `MaterialEditTextPreference`, `MaterialListPreference`, `MaterialMultiSelectListPreference` 클래스를 제공하여, 환경설정 XML에서 사용함으로써 자동으로 Material 테마가 적용된 대화상자를 사용할 수 있습니다. 자세한 내용은 샘플 프로젝트를 참고하세요.

# Tint Helper

`MDTintHelper` 클래스를 사용하면 체크박스, 라디오 버튼, 에디트 텍스트, 프로그레스 바를 동적으로 색상 변경할 수 있습니다(런타임에 `styles.xml`을 변경할 수 없는 문제를 우회하기 위함입니다). 이 클래스는 라이브러리 내부에서 UI 요소를 동적으로 색상 변경하여 설정한 `widgetColor`에 맞추는 데 사용됩니다.

---

# 기타

액션 버튼이 눌렸을 때나 사용자가 목록 항목을 선택했을 때 다이얼로그가 자동으로 닫히는 것을 원하지 않는 경우:

```java
MaterialDialog dialog new MaterialDialog.Builder(this)
        // ... other initialization
        .autoDismiss(false)
        .show();
```