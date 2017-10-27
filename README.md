# Synopsis

This plugin helps you internationalize you Flutter app by generating be needed boiler plate code. You just add and organize strings in files that are contained in the /res/values folder. This plugin is based on the [Internationalizing Flutter Apps](https://flutter.io/tutorials/internationalization/) tutorial and on the [Material Library Localizations](https://github.com/flutter/flutter/tree/master/packages/flutter_localizations/lib/src/l10n) package. Much of the instructions are taken from there.

# Usage

### 1. Setup you App

 Setup your localizationsDelegates and your supportedLocales witch will allow to access the strings.

<pre style="margin-left: 80px;">class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      <b>localizationsDelegates: [S.delegate],</b>
      <b>supportedLocales: S.delegate.supportedLocales,</b>
      title: 'Flutter Demo',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}</pre>

Optionally you can provide a fallback Locale for the unsupported languages. In case that the user changes the device language to an unsupported language the default resolution is:

1.  The first supported locale with the same Locale.languageCode.
2.  The first supported locale.

If you want to change the last step and to provided a default locale instead of choosing the first one in you supported list, you can specify that as fallows:

<pre style="margin-left: 60px;">class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      localizationsDelegates: [S.delegate],
      supportedLocales: S.delegate.supportedLocales,
      <b>localeResolutionCallback: S.delegate.resolution(fallback: new Locale("en", "")),</b>
      title: 'Flutter Demo',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}</pre>

<img src="/extras/arb_icon.png" width="150">
### 2.  Setup the arb files. 

ARB files extension stands for [Application Resource Bundle](https://github.com/googlei18n/app-resource-bundle) and is used by the Dart [intl](https://pub.dartlang.org/packages/intl) package, it is supported by the [Google Translators Toolkit](https://translate.google.com/toolkit) and it's supported by Google.

Flutter internalization only depends on a small subset of the ARB format. Each .arb file contains a single JSON table that maps from resource IDs to localized values. Filenames contain the locale that the values have been translated for. For example material_de.arb contains German translations, and material_ar.arb contains Arabic translations. Files that contain regional translations have names that include the locale's regional suffix. For example material_en_GB.arb contains additional English translations that are specific to Great Britain.

<b>The first English file is generated for you(/res/values/strings_en.arb).</b> Every arb file depends on this one. If you have an a string in the German arb file(/res/values/strings_de.arb) that has an ID that is <b>not found</b> in the English file, it would not be listed. So you must be sure to first have the strings in the English file and then add other translations.

Do not add white spaces or illegal characters in your IDs. They should match this RegEx <b>[a-zA-Z]</b> <span style="color: rgb(0, 0, 0); font-family: &quot;Source Sans Pro&quot;, &quot;Helvetica Neue&quot;, Helvetica, Arial, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: normal; letter-spacing: normal; orphans: 2; text-align: left; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-style: initial; text-decoration-color: initial; display: inline !important; float: none;">for now</span>. The IDE doesn't worn you if your ID doesn't match.

To add a new arb file right click on <b>values</b> folder and select <b>New</b> -><b> Arb </b><b>File</b>. Then pick your language from the list, and region if necessary.

#### 1. Referencing the values

The ARB table's keys, called resource IDs, are valid Dart variable names. They correspond to methods from the S class. For example:

<pre style="margin-left: 60px;">Widget build(BuildContext context) {
  return new FlatButton(
    child: new Text(
      <b>S.of(context).cancelButtonLabel,</b>
    ),
  );
}</pre>

#### 2. Parametrized strings

Some strings may contain <em>$variable</em> tokens witch are replaced with your value. For example:

    {   
        "aboutListTileTitle": "About $applicationName"  
    }

The value for this resource ID is retrieved with a parametrized method instead of a simple getter:  

    S.of(context).aboutListTileTitle(yourAppTitle)

#### 3. Plurals

Plural translations can be provided for several quantities: 0, 1, 2, "few", "many", "other". The variations are identified by a resource ID suffix which must be one of "Zero", "One", "Two", "Few", "Many", "Other". The "Other" variation is used when none of the other quantities apply. All plural resources must include a resource with the "Other" suffix. For example the English translations ('material_en.arb') for selectedRowCountTitle in the [Material Library Localizations](https://github.com/flutter/flutter/tree/master/packages/flutter_localizations/lib/src/l10n) are:

    {
        "selectedRowCountTitleZero": "No items selected",
        "selectedRowCountTitleMany": "to many items", //not actual real
        "selectedRowCountTitleOne": "1 item selected",
        "selectedRowCountTitleOther": "$selectedRowCount items selected",</pre>
    }

In your code you can use this methods

<pre>S.of(context).selectedRowCountTitle("many")</pre>

or

<pre>S.of(context).selectedRowCountTitle("1")</pre>

or

<pre>S.of(context).selectedRowCountTitle("$selectedRowCount")</pre>

# Issues

There are some performance improvements and bug fixes that this plugin could use, so feel free to PR.
