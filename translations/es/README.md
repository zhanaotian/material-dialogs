# Material Dialogs

<!-- github-global:langs:start -->
[简体中文](../zh-CN/README.md) | [日本語](../ja/README.md) | [繁體中文](../zh-TW/README.md) | [Español](./README.md) | [한국어](../ko/README.md)
<!-- github-global:langs:end -->


![Capturas de pantalla](https://raw.githubusercontent.com/afollestad/material-dialogs/master/art/mdshowcase.png)

# Proyecto de ejemplo

Puedes descargar el APK de ejemplo más reciente desde este repositorio aquí: https://github.com/afollestad/material-dialogs/blob/master/sample/sample.apk

También está en Google Play:

<a href="https://play.google.com/store/apps/details?id=com.afollestad.materialdialogssample">
  <img alt="Get it on Google Play"
       src="https://developer.android.com/images/brand/en_generic_rgb_wo_60.png" />
</a>

Tener instalado el proyecto de ejemplo es una buena forma de recibir notificaciones sobre nuevas versiones. Aunque si observas (Watching) este repositorio, GitHub te enviará un correo electrónico cada vez que publique una versión.

---

# Dependencia de Gradle (jCenter)

Referencia fácilmente la biblioteca en tus proyectos de Android usando esta dependencia en el archivo `build.gradle` de tu módulo:

```Gradle
dependencies {
    compile 'com.afollestad:material-dialogs:0.7.1.3'
}
```

[ ![Download](https://api.bintray.com/packages/drummer-aidan/maven/material-dialogs/images/download.svg) ](https://bintray.com/drummer-aidan/maven/material-dialogs/_latestVersion)

---

# Novedades

Consulta la página de Releases del proyecto para ver una lista de versiones con sus changelogs.

### [Ver versiones](https://github.com/afollestad/material-dialogs/releases)

Si observas (Watch) este repositorio, GitHub te enviará un correo electrónico cada vez que publique una actualización.

---

# Diálogo básico

En primer lugar, ten en cuenta que `MaterialDialog` extiende `DialogBase`, que a su vez extiende `AlertDialog`. Aunque un número muy pequeño de los métodos originales están descontinuados a propósito y no funcionan, tienes acceso a métodos como `dismiss()`, `setTitle()`, `setIcon()`, etc. Las alternativas se comentan más abajo.

Aquí tienes un ejemplo básico que imita el diálogo que ves en las guías de diseño Material de Google (aquí: http://www.google.com/design/spec/components/dialogs.html#dialogs-usage). Ten en cuenta que siempre puedes sustituir cadenas literales y recursos de cadena en los métodos que aceptan cadenas, y lo mismo aplica para los recursos de color (por ejemplo, `titleColor` y `titleColorRes`).

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .negativeText(R.string.disagree)
        .show();
```

En Lollipop (API 21+) o si usas AppCompat, el diálogo Material coincidirá automáticamente con el `positiveColor` (que se usa en el botón de acción positivo) con el atributo `colorAccent` de tu tema en styles.xml.

Si el contenido es lo suficientemente largo, se volverá desplazable y se mostrará un divisor encima de los botones de acción.

---

# Migración desde AlertDialogs

Si estás migrando diálogos antiguos puedes usar ```AlertDialogWrapper```. Necesitas cambiar los imports y reemplazar ```AlertDialog.Builder``` por ```AlertDialogWrapper.Builder```:

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

Pero se recomienda encarecidamente usar la API original de ```MaterialDialog``` para nuevos usos.

---

# Mostrar un icono

MaterialDialog admite la visualización de un icono al igual que el AlertDialog estándar; se colocará a la izquierda del título.

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.agree)
        .icon(R.drawable.icon)
        .show();
```

Puedes limitar el tamaño máximo del icono usando los métodos del Builder `limitIconToDefaultSize()`, `maxIconSize(int size)`,
 o `maxIconSizeRes(int sizeRes)`.

---

# Botones de acción apilados

Si tienes varios botones de acción que en conjunto son demasiado anchos para caber en una sola línea, el diálogo apilará los botones en orientación vertical.

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .content(R.string.content)
        .positiveText(R.string.longer_positive)
        .negativeText(R.string.negative)
        .show();
```

También puedes forzar que el diálogo apile sus botones con el método `forceStacking()` del `Builder`.

---

# Botón de Acción Neutral

Puedes especificar un texto neutral además del texto positivo y negativo. Mostrará la acción neutral en el extremo izquierdo.

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

# Callbacks

Para saber cuándo el usuario selecciona un botón de acción, debes establecer un callback. Para ello, usa la clase `ButtonCallback` y sobrescribe sus métodos `onPositive()`, `onNegative()` u `onNeutral()` según sea necesario. La ventaja de esto es que puedes sobrescribir la funcionalidad de los botones *À la carte*, por lo que no es necesario implementar métodos vacíos.

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

Si `autoDismiss` está desactivado, entonces debes cerrar el diálogo manualmente en estos callbacks. El cierre automático está activado por defecto.

---

# Diálogos de lista

Crear un diálogo de lista solo requiere pasar un array de cadenas. El callback (`itemsCallback`) también es muy simple.

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

Si `autoDismiss` está desactivado, entonces debes descartar manualmente el diálogo en el callback. El auto descarte está activado por defecto.
Puedes pasar `positiveText()` u otros botones de acción al builder para forzar que muestre los botones de acción
debajo de tu lista, aunque esto solo es útil en algunos casos específicos.

---

# Diálogos de lista de selección única

Los diálogos de lista de selección única son casi idénticos a los diálogos de lista normales. La única diferencia es que
usas `itemsCallbackSingleChoice` para establecer un callback en lugar de `itemsCallback`. Esto indica al diálogo que
muestre botones de opción junto a los elementos de la lista.

```java
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .items(R.array.items)
        .itemsCallbackSingleChoice(-1, new MaterialDialog.ListCallbackSingleChoice() {
            @Override
            public boolean onSelection(MaterialDialog dialog, View view, int which, CharSequence text) {
                /**
                 * Si usas alwaysCallSingleChoiceCallback(), que se explica a continuación,
                 * devolver false aquí no permitirá que el botón de opción recién seleccionado se seleccione realmente.
                 **/
                return true;
            }
        })
        .positiveText(R.string.choose)
        .show();
```

Si quieres preseleccionar un elemento, pasa un índice 0 o mayor en lugar de -1 en `itemsCallbackSingleChoice()`.
Más tarde, puedes actualizar el índice seleccionado usando `setSelectedIndex(int)` en la instancia de `MaterialDialog`,
si no estás usando un adaptador personalizado.

Si no estableces un botón de acción positiva usando `positiveText()`, el diálogo llamará automáticamente
al callback de selección única cuando el usuario pulse el botón de acción positiva. El diálogo también se descartará
a sí mismo, a menos que el auto descarte esté desactivado.

Si haces una llamada a `alwaysCallSingleChoiceCallback()`, el callback de selección única se llamará
cada vez que el usuario seleccione un elemento.

## Colorear botones de opción

Al igual que los botones de acción y muchos otros elementos del diálogo de Material, puedes personalizar el color de los botones de opción de un diálogo. La clase `Builder` contiene los métodos `widgetColor()`, `widgetColorRes()` y `widgetColorAttr()`. Sus nombres y las anotaciones de sus parámetros los hacen autoexplicativos. Ten en cuenta que, por defecto, los botones de opción se colorearán con el color definido en `colorAccent` (para AppCompat) o `android:colorAccent` (para el tema Material) en el tema de tu Activity.

También existe un atributo de tematización global, como se muestra en la sección Global Theming de este README: `md_widget_color`.

---

# Diálogos de lista de selección múltiple

Los diálogos de lista de selección múltiple son casi idénticos a los diálogos de lista normales. La única diferencia es que
usas `itemsCallbackMultiChoice` para establecer un callback en lugar de `itemsCallback`. Esto indica al diálogo que
muestre casillas de verificación junto a los elementos de la lista, y el callback puede devolver múltiples selecciones.

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

Si quieres preseleccionar algún elemento, pasa un array de índices (recurso o literal) en lugar de null
en `itemsCallbackMultiChoice()`. Más tarde, puedes actualizar los índices seleccionados usando `setSelectedIndices(Integer[])`
en la instancia de `MaterialDialog`, si no estás usando un adaptador personalizado.

Si no estableces un botón de acción positiva usando `positiveText()`, el diálogo llamará automáticamente
al callback de selección múltiple cuando el usuario pulse el botón de acción positiva. El diálogo también se descartará a sí mismo,
a menos que el auto descarte esté desactivado.

Si haces una llamada a `alwaysCallMultiChoiceCallback()`, el callback de selección múltiple se llamará
cada vez que el usuario seleccione un elemento.

## Casillas de verificación

Al igual que los botones de acción y muchos otros elementos del diálogo Material, puedes personalizar el color de las casillas de verificación de un diálogo. La clase `Builder` contiene los métodos `widgetColor()`, `widgetColorRes()` y `widgetColorAttr()`. Sus nombres y las anotaciones de sus parámetros los hacen autoexplicativos. Ten en cuenta que, de forma predeterminada, las casillas de verificación se colorearán con el color definido en `colorAccent` (para AppCompat) o `android:colorAccent` (para el tema Material) en el tema de tu Activity.

También existe un atributo de tematización global, como se muestra en la sección Global Theming de este README: `md_widget_color`.

---

# Diálogos de listas personalizadas

Al igual que los diálogos nativos de Android, también puedes pasar tu propio adaptador mediante `.adapter()` para personalizar
exactamente cómo quieres que funcione tu lista.

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

Si necesitas acceder al `ListView`, puedes usar la instancia de `MaterialDialog`:

```java
MaterialDialog dialog = new MaterialDialog.Builder(this)
        ...
        .build();

ListView list = dialog.getListView();
// Do something with it

dialog.show();
```

Ten en cuenta que no necesitas estar usando un adaptador personalizado para acceder al `ListView`; está disponible para diálogos de selección simple/múltiple, diálogos de listas normales, etc.

---

# Vistas personalizadas

Las vistas personalizadas son muy fáciles de implementar.

```java
boolean wrapInScrollView = true;
new MaterialDialog.Builder(this)
        .title(R.string.title)
        .customView(R.layout.custom_view, wrapInScrollView)
        .positiveText(R.string.positive)
        .show();
```

Si `wrapInScrollView` es verdadero, la biblioteca colocará tu vista personalizada dentro de un ScrollView por ti.
Esto permite a los usuarios desplazar tu vista personalizada si es necesario (pantallas pequeñas, contenido largo, etc.). Sin embargo, hay casos
en los que no deseas ese comportamiento. Esto se aplica principalmente a los casos en los que tendrías un ScrollView en tu diseño personalizado,
incluyendo ListViews, RecyclerViews, WebViews, GridViews, etc. El proyecto de ejemplo contiene ejemplos de uso tanto de verdadero
como de falso para este parámetro.

Tu vista personalizada tendrá automáticamente un padding a su alrededor cuando `wrapInScrollView` sea verdadero. De lo contrario,
serás responsable de usar valores de padding que se vean bien con tu contenido.

## Acceso posterior

Si necesitas acceder a una View en la vista personalizada después de construir el diálogo, puedes usar `getCustomView()` de
`MaterialDialog`. Esto es especialmente útil si pasas un recurso de layout al `Builder`, ya que el diálogo
se encargará de inflar la vista por ti.

```java
MaterialDialog dialog = //... inicialización mediante el builder ...
View view = dialog.getCustomView();
```

---

# Tipografías

Por defecto, Material Dialogs usará la fuente `Roboto Medium` para el título del diálogo y los botones de acción,
y `Roboto Regular` para el contenido, los elementos de la lista, etc. Esto se hace utilizando los recursos de fuentes incluidos en esta biblioteca,
por lo que estas fuentes se usarán incluso en dispositivos Samsung que por defecto usan tipografías extrañas de escritura a mano.

Si quieres evitar este comportamiento predeterminado, puedes hacer una llamada a `disableDefaultFonts()` al
usar el `Builder`. Esto hará que la biblioteca no aplique las fuentes Roboto y Roboto Medium,
y todo usará la fuente regular del sistema.

Si quieres usar fuentes personalizadas explícitamente, puedes hacer una llamada a `typeface(String, String)` al
usar el `Builder`. Esto cargará fuentes desde archivos TTF en la carpeta `assets` de tu proyecto. Por ejemplo,
si tuvieras `Roboto.ttf` y `Roboto-Light.ttf` en `/src/main/assets/fonts`, llamarías a `typeface("Roboto", "Roboto-Light")`.
Ten en cuenta que no se usa extensión en el nombre. Este método también se encarga de reciclar Typefaces mediante el `TypefaceHelper`, el cual
puedes usar en tu propio proyecto para evitar asignaciones duplicadas. Si quieres cargar otros archivos Typeface que
no sean archivos ttf, puedes usar el método del Builder `typeface(Typeface, Typeface)`.

---

# Obtener y establecer botones de acción

Si quieres obtener una referencia a uno de los botones de acción del diálogo después de que el diálogo haya sido construido y mostrado (por ejemplo, para habilitar o deshabilitar botones):

```java
MaterialDialog dialog = //... initialization via the builder ...
View negative = dialog.getActionButton(DialogAction.NEGATIVE);
View neutral = dialog.getActionButton(DialogAction.NEUTRAL);
View positive = dialog.getActionButton(DialogAction.POSITIVE);
```

Si quieres actualizar el título de un botón de acción del diálogo (también puedes pasar un ID de recurso de cadena en lugar de la cadena literal):

```java
MaterialDialog dialog = //... initialization via the builder ...
dialog.setActionButton(DialogAction.NEGATIVE, "New Title");
```

---

# Temas

Antes de Lollipop, aplicar temas a los AlertDialogs era prácticamente imposible sin usar reflexión y drawables personalizados.
Desde KitKat, Android se volvió más neutro en cuanto a colores, pero los AlertDialogs seguían usando el azul Holo para el título y
el divisor del título. Lollipop mejoró aún más, sin colores en el diálogo por defecto, salvo en los botones de acción. Esta biblioteca hace que aplicar temas sea aún más fácil.

## Conceptos básicos

Por defecto, Material Dialogs aplicará un tema claro u oscuro según el atributo `?android:textColorPrimary` 
obtenido del contexto que crea el diálogo. Si el color es claro (por ejemplo, más blanco), asumirá que la Activity 
está usando un tema oscuro y usará el tema oscuro del diálogo. Y viceversa para el tema claro. 
Puedes establecer manualmente el tema usado desde el método `Builder#theme()`:

```java
new MaterialDialog.Builder(this)
        .content("Hi")
        .theme(Theme.DARK)
        .show();
```

O puedes usar el atributo de tematización global, que se explica en la sección siguiente. La tematización global 
evita tener que llamar constantemente a los setters de tema para cada diálogo que muestres.

## Colores

Prácticamente todos los aspectos de un diálogo creado con esta librería pueden ser coloreados:

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

Los nombres son en su mayoría autoexplicativos. El método `widgetColor`, del que se habla en otras secciones de este tutorial, se aplica a las barras de progreso, las casillas de verificación y los botones de opción. Ten en cuenta también que cada uno de estos métodos tiene 3 variaciones para establecer un color directamente, usando recursos de color y usando atributos de color.

## Selectores

Los selectores de tematización te permiten cambiar los colores de los elementos pulsables:

```java
new MaterialDialog.Builder(this)
        .btnSelector(R.drawable.custom_btn_selector)
        .btnSelector(R.drawable.custom_btn_selector_primary, DialogAction.POSITIVE)
        .btnSelectorStacked(R.drawable.custom_btn_selector_stacked)
        .listSelector(R.drawable.custom_list_and_stackedbtn_selector)
        .show();
```

La primera línea `btnSelector` establece un drawable selector usado para todos los botones de acción. La segunda línea `btnSelector`
sobrescribe el drawable usado solo para el botón positivo. Esto hace que el botón positivo tenga
un selector diferente al de los botones neutral y negativo. `btnSelectorStacked` establece un drawable selector
usado cuando los botones se apilan, ya sea porque no hay espacio suficiente para colocarlos todos en una línea,
o porque usaste `forceStacked(true)` en el `Builder`. `listSelector` se usa para los elementos de la lista, cuando
NO estás usando un adaptador personalizado.

***Una nota importante relacionada con el uso de selectores personalizados para los botones de acción***: asegúrate de que tu drawable selector haga referencia
a drawables de tipo inset como lo hacen los predeterminados; esto es importante para un padding correcto de los botones de acción.

## Gravity

Probablemente sea poco probable que quieras cambiar la gravedad de los elementos en un diálogo, pero es posible.

```java
new MaterialDialog.Builder(this)
        .titleGravity(GravityEnum.CENTER_HORIZONTAL)
        .contentGravity(GravityEnum.CENTER_HORIZONTAL)
        .btnStackedGravity(GravityEnum.START)
        .itemsGravity(GravityEnum.END)
        .buttonsGravity(GravityEnum.END)
        .show();
```

Estas opciones son bastante autoexplicativas. `titleGravity` establece la gravedad del título del diálogo, `contentGravity`
establece la gravedad del contenido del diálogo, `btnStackedGravity` establece la gravedad de los botones de acción apilados,
`itemsGravity` establece la gravedad de los elementos de la lista (cuando NO estás usando un adaptador personalizado).

Para `buttonsGravity`, consulta esto:

<table>
<tr>
<td><b>START (Predeterminado)</b></td>
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

Sin un botón positivo, el botón negativo ocupa su lugar, excepto con CENTER.

## Paleta Material

Para ver los colores que se ajustan a la paleta de diseño Material, consulta esta página: http://www.google.com/design/spec/style/color.html#color-color-palette

---

# Tematización global

La mayoría de los aspectos de tematización discutidos en la sección anterior se pueden aplicar automáticamente a todos los diálogos
que muestres desde una Activity que tenga un tema que contenga cualquiera de estos atributos:

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

El color de los botones de acción también se deriva del atributo `android:colorAccent` del tema Material,
o del atributo `colorAccent` del tema Material de AppCompat, como se ve en el proyecto de ejemplo. Establecer
el color manualmente anulará ese comportamiento.

---

# Callbacks de show, cancel y dismiss

Puedes configurar directamente los listeners de show/cancel/dismiss desde el `Builder` en lugar de en la instancia
`MaterialDialog` resultante:

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

# Diálogos de entrada

Un diálogo de entrada es bastante autoexplicativo: obtiene datos del usuario de tu aplicación mediante un campo de entrada (EditText). También puedes mostrar contenido encima del EditText si lo deseas.

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

Ten en cuenta que el diálogo forzará que el botón de acción positiva sea visible; cuando se pulsa, la entrada se envía al callback.

El diálogo de entrada gestionará automáticamente el enfoque del EditText y mostrará el teclado para que el usuario pueda introducir datos de inmediato. Cuando el diálogo se cierre, el teclado se ocultará automáticamente.

## Colorear el EditText

Al igual que los botones de acción y muchos otros elementos del diálogo Material, puedes personalizar el color del `EditText` de un diálogo de entrada. La clase `Builder` contiene los métodos `widgetColor()`, `widgetColorRes()` y `widgetColorAttr()`. Sus nombres y las anotaciones de sus parámetros los hacen autoexplicativos. Ten en cuenta que, por defecto, los EditText se colorearán con el color definido en `colorAccent` (para AppCompat) o `android:colorAccent` (para el tema Material) en el tema de tu Activity.

También existe un atributo de tematización global, como se muestra en la sección Global Theming de este README: `md_widget_color`.

---

# Diálogos de progreso

Esta biblioteca te permite mostrar diálogos de progreso con diseño Material que incluso usan el color de acento de tu aplicación para colorear las barras de progreso (si usas AppCompat para aplicar temas a tu aplicación, o el tema Material en Lollipop).

## Diálogos de progreso indeterminado

Esto mostrará el clásico diálogo de progreso con un círculo giratorio; consulta el proyecto de ejemplo para verlo en acción:

```java
new MaterialDialog.Builder(this)
    .title(R.string.progress_dialog)
    .content(R.string.please_wait)
    .progress(true, 0)
    .show();
```

## Diálogos de progreso determinados (barra de búsqueda)

Si un diálogo no es indeterminado, muestra una barra de progreso horizontal que aumenta hasta un valor máximo.
Los comentarios en el código explican qué hace esto.

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

Consulta el proyecto de ejemplo para ver este diálogo en acción, con la adición de hilos (threading).

## Colorear la barra de progreso

Al igual que los botones de acción y muchos otros elementos del diálogo Material, puedes personalizar el color de la barra de progreso de un diálogo de progreso. La clase `Builder` contiene los métodos `widgetColor()`, `widgetColorRes()` y `widgetColorAttr()`. Sus nombres y las anotaciones de sus parámetros los hacen autoexplicativos. Ten en cuenta que, por defecto, las barras de progreso se colorearán con el color definido en `colorAccent` (para AppCompat) o `android:colorAccent` (para el tema Material) en el tema de tu Activity.

También existe un atributo de tematización global como se muestra en la sección Global Theming de este README: `md_widget_color`.

---

# Diálogos de preferencias

Los componentes `EditTextPreference`, `ListPreference` y `MultiSelectListPreference` de Android te permiten asociar la configuración de una actividad de preferencias con la entrada del usuario recibida mediante escritura o selección. Material Dialogs incluye las clases `MaterialEditTextPreference`, `MaterialListPreference` y `MaterialMultiSelectListPreference` que pueden usarse en tu XML de preferencias para utilizar automáticamente diálogos con tema Material. Consulta el proyecto de ejemplo para más detalles.

---

# Tint Helper

Puedes usar la clase `MDTintHelper` para colorear dinámicamente casillas de verificación, botones de radio, campos de texto y barras de progreso
(para sortear la imposibilidad de cambiar `styles.xml` en tiempo de ejecución). La biblioteca la usa para colorear dinámicamente
los elementos de la interfaz y que coincidan con tu `widgetColor` configurado.

---

# Misceláneo

Si no quieres que el diálogo se descarte automáticamente cuando se pulsa un botón de acción o cuando
el usuario selecciona un elemento de la lista:

```java
MaterialDialog dialog new MaterialDialog.Builder(this)
        // ... other initialization
        .autoDismiss(false)
        .show();
```