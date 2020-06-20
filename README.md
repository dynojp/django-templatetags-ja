# Django テンプレートタグまとめ

Python ベースのウェブアプリケーションフレームワーク Django のコアが提供するテンプレートタグ・テンプレートフィルタの一覧です。

対象の Django のバージョンは 3.0 です。

（サンプルコードのほとんどは公式ドキュメントのものです）

- [テンプレートタグ](#テンプレートタグ)
    - [`autoescape`](#autoescape)
    - [`block`](#block)
    - [`blocktrans`](#blocktrans)
    - [`comment`](#comment)
    - [`csrf_token`](#csrf_token)
    - [`cycle`](#cycle)
    - [`debug`](#debug)
    - [`extends`](#extends)
    - [`filter`](#filter)
    - [`firstof`](#firstof)
    - [`for`](#for)
    - [`for` … `empty`](`#for--empty`)
    - [`get_available_languages`](#get_available_languages)
    - [`get_current_language`](#get_current_language)
    - [`get_current_language_bidi`](#get_current_language_bidi)
    - [`get_language_info`](#get_language_info)
    - [`get_language_info_list`](#get_language_info_list)
    - [`get_media_prefix`](#get_media_prefix)
    - [`get_static_prefix`](#get_static_prefix)
    - [`if`](#if)
    - [`ifchanged`](#ifchanged)
    - [`ifequal` / `ifnotequal`](#ifequal--ifnotequal)
    - [`include`](#include)
    - [`load`](#load)
    - [`localize`](#localize)
    - [`lorem`](#lorem)
    - [`now`](#now)
    - [`regroup`](#regroup)
    - [`resetcycle`](#resetcycle)
    - [`spaceless`](#spaceless)
    - [`static`](#static)
    - [`templatetag`](#templatetag)
    - [`trans`](#trans)
    - [`url`](#url)
    - [`verbatim`](#verbatim)
    - [`widthratio`](#widthratio)
    - [`with`](#with)
- [テンプレートフィルタ](#テンプレートフィルタ)
    - [`add`](#add)
    - [`addslashes`](#addslashes)
    - [`apnumber`](#apnumber)
    - [`capfirst`](#capfirst)
    - [`center`](#center)
    - [`cut`](#cut)
    - [`date`](#date)
    - [`default_if_none`](#default_if_none)
    - [`default`](#default)
    - [`dictsort`](#dictsort)
    - [`dictsortreversed`](#dictsortreversed)
    - [`divisibleby`](#divisibleby)
    - [`escape`](#escape)
    - [`escapejs`](#escapejs)
    - [`filesizeformat`](#filesizeformat)
    - [`first`](#first)
    - [`floatformat`](#floatformat)
    - [`force_escape`](#force_escape)
    - [`get_current_timezone`](#get_current_timezone)
    - [`get_digit`](#get_digit)
    - [`intcomma`](#intcomma)
    - [`intword`](#intword)
    - [`iriencode`](#iriencode)
    - [`join`](#join)
    - [`json_script`](#json_script)
    - [`language_bidi`](#language_bidi)
    - [`language_name_local`](#language_name_local)
    - [`language_name_translated`](#language_name_translated)
    - [`language_name`](#language_name)
    - [`last`](#last)
    - [`length_is`](#length_is)
    - [`length`](#length)
    - [`linebreaks`](#linebreaks)
    - [`linebreaksbr`](#linebreaksbr)
    - [`linenumbers`](#linenumbers)
    - [`ljust`](#ljust)
    - [`localize`](#localize-1)
    - [`localtime`](#localtime)
    - [`lower`](#lower)
    - [`make_list`](#make_list)
    - [`naturalday`](#naturalday)
    - [`naturaltime`](#naturaltime)
    - [`ordinal`](#ordinal)
    - [`phone2numeric`](#phone2numeric)
    - [`pluralize`](#pluralize)
    - [`pprint`](#pprint)
    - [`random`](#random)
    - [`rjust`](#rjust)
    - [`safe`](#safe)
    - [`safeseq`](#safeseq)
    - [`slice`](#slice)
    - [`slugify`](#slugify)
    - [`stringformat`](#stringformat)
    - [`striptags`](#striptags)
    - [`time`](#time)
    - [`timesince`](#timesince)
    - [`timeuntil`](#timeuntil)
    - [`timezone`](#timezone)
    - [`title`](#title)
    - [`truncatechars_html`](#truncatechars_html)
    - [`truncatechars`](#truncatechars)
    - [`truncatewords_html`](#truncatewords_html)
    - [`truncatewords`](#truncatewords)
    - [`unlocalize`](#unlocalize)
    - [`unordered_list`](#unordered_list)
    - [`upper`](#upper)
    - [`urlencode`](#urlencode)
    - [`urlize`](#urlize)
    - [`urlizetrunc`](#urlizetrunc)
    - [`utc`](#utc)
    - [`wordcount`](#wordcount)
    - [`wordwrap`](#wordwrap)
    - [`yesno`](#yesno)

## テンプレートタグ

### `autoescape`

HTML のエスケープの有効・無効を切り替えます。

```django
{% autoescape on %}
    {{ body }}
{% endautoescape %}
```

デフォルトではエスケープは有効になっています。

### `block`

`extends` タグを使って他のテンプレートを継承したときに特定のブロックの中身を書き換えます。

```django
{% extends "base.html" %}

{% block main_content %}
{% endblock main_content %}
```

`endblock` の後の名前は付けても付けなくても問題ありません。

継承元の内容を上書きするのではなく追加したいときは `block.super` 変数を使用します。

```django
{% extends "base.html" %}

{% block breadcrumbs %}
{{ block.super }} / {{ title }}
{% endblock breadcrumbs %}
```

### `blocktrans`

範囲内の文字列を翻訳対象にします。

```django
{% blocktrans with book_t=book|title author_t=author|title %}
This is {{ book_t }} by {{ author_t }}
{% endblocktrans %}
```

変数は `with` の後に渡します。

### `comment`

範囲内のコードをコメント扱いにして無視します（出力しません）。

```django
{% comment "Optional note" %}
    <p>Commented out text with {{ create_date|date:"c" }}</p>
{% endcomment %}
```

オプションでメモ用の文字列を渡すことができます。

単一行のコメントは `{# これはコメントです #}` で、複数行のコメントは `comment` タグでと使い分けるとよいと思います。

### `csrf_token`

CSRF （クロスサイトリクエストフォージェリ）保護のための `<input type="hidden" />` タグを出力します。

```django
<form method="post">{% csrf_token %}
</form>
```

Django の CSRF 保護機能について詳しくは次のページをご覧ください。

- [Cross Site Request Forgery protection | Django documentation | Django](https://docs.djangoproject.com/en/3.0/ref/csrf/)

### `cycle`

渡された文字列を順番に出力します。

```django
{% for item in item_list %}
    <tr class="{% cycle "odd" "even" %}">
        ...
    </tr>
{% endfor %}
```

主にループの中で使用します。

### `debug`

デバッグ情報を出力します。

```django
<textarea> {% filter force_escape %} {% debug %} {% endfilter %} </textarea>
```

そのままでは `<` や `>` がエスケープされずに出力されるためページ上にうまく表示されません。
ページ上で確認したいときは `filter force_escape` 等をあわせて使うと便利です。

ただし、デバッグ情報の出力にはこの `debug` タグではなく [`django-debug-toolbar`](https://github.com/jazzband/django-debug-toolbar) がよく使われます。

### `extends`

他のテンプレートを継承します。

```django
{% extends "base.html" %}
```

```django
{% extends "blog/article_list.html" %}
```

テンプレート名はテンプレートローダーのルートディレクトリからのパスで指定します。
`./` または `../` から始めることで、現在のテンプレートからの相対パスでも指定できます。

通常は `block` タグと併用してブロックの内容を上書きして使用します。

### `filter`

範囲内の文字列にフィルタをかけます。

```django
{% filter force_escape|lower %}
    This text will be HTML-escaped, and will appear in all lowercase.
{% endfilter %}
```

`|` でつないで複数のフィルタをかけることができます。

### `firstof`

渡された引数の中から `bool()` に渡したときの戻り値が `False` にならない最初の要素を返します。

```django
{% firstof var1 var2 var3 %}
```

結果をそのまま出力するのではなく後で利用したいときは `as` を使って変数に格納します。

```django
{% firstof var1 var2 var3 as value %}
```

### `for`

iterable なオブジェクトに対してループを回します。

```django
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>
```

末尾の方から要素を取り出したいときは `{% for obj in list reversed %}` と `reversed` を付けます。

例えば `dict` に対してループを回したいときは次のようにします。

```django
{% for key, value in data.items %}
    {{ key }}: {{ value }}
{% endfor %}
```

`for` から `endfor` の間では以下の変数を格納した `forloop` オブジェクトが利用できます。

| `forloop.counter` | 1 始まりのカウンタ |
| `forloop.counter0` | 0 始まりのカウンタ |
| `forloop.revcounter` | 1 で終わる逆順のカウンタ |
| `forloop.revcounter0` | 0 で終わる逆順のカウンタ |
| `forloop.first` | 最初の要素では `True` 、その他は `False` |
| `forloop.last` | 最後の要素では `True` 、その他は `False` |
| `forloop.parentloop` | ネストされたループにおける親の `forloop` オブジェクト |

例:

```django
{# 1 から順番にカウントアップする #}
<ul>
{% for athlete in athlete_list %}
    <li>{{ forloop.counter }}: {{ athlete.name }}</li>
{% endfor %}
</ul>
```

### `for` … `empty`

`for` タグには `empty` タグを付けて、 iterable が空の場合のみ出力する文字列を指定することができます。

```django
<ul>
{% for gu in gu_list %}
    <li>{{ gu.name }}</li>
{% empty %}
    <li>具はありません。</li>
{% endfor %}
</ul>
```

### `get_available_languages`

利用可能な言語の一覧を返します。

```django
{% load i18n %}
{% get_available_languages as languages %}
{{ languages }}
```

各要素はひとつめが言語コード、ふたつめが言語名の 2 要素の `tuple` です。
`as` を使って変数名を指定する必要があります。

### `get_current_language`

現在アクティブになっている言語の言語コードを返します。

```django
{% load i18n %}
{% get_current_language as language %}
{{ language }}
{# "ja" #}
```

言語コードは例えば日本語の場合は `ja` です。
`as` を使って変数名を指定する必要があります。

### `get_current_language_bidi`

現在アクティブになっている言語が右から左に読む言語（アラビア語等）の場合のみ `True` を返します。

```django
{% load i18n %}
{% get_current_language_bidi as language %}
{{ language }}
{# False #}
```

### `get_language_info`

`for` で指定された言語コードの言語情報を返します。

```django
{% block content_main_body %}
{% load i18n %}
{% get_language_info for "tr" as lang_info %}
```

戻り値は以下の情報を持つオブジェクトです。

<table>
    <tr>
        <th><code>lang_info.code</code></th>
        <td>言語コード</td>
    </tr>
    <tr>
        <th><code>lang_info.name_local</code></th>
        <td>ローカルな名前</td>
    </tr>
    <tr>
        <th><code>lang_info.name</code></th>
        <td>英語での名前</td>
    </tr>
    <tr>
        <th><code>lang_info.bidi</code></th>
        <td>右から左に読む言語かどうか</td>
    </tr>
    <tr>
        <th><code>lang_info.name_translated</code></th>
        <td>アクティブな言語での名前</td>
    </tr>
</table>

### `get_language_info_list`

`for` で指定された言語コードに対応する言語情報の一覧を返します。

```django
{% load i18n %}
{% get_language_info_list for lang_codes as lang_infos %}
{% for lang_info in lang_infos %}
    {{ ... }}
{% endfor %}
```

戻り値は `get_language_info` の戻り値と同等のオブジェクトを要素に持つリストです。

### `get_media_prefix`

設定項目 `MEDIA_URL` で設定されたメディアプリフィクスを返します。

```django
{% load static %}
<body data-media-url="{% get_media_prefix %}">
{% "/media/" 等 %}
```

### `get_static_prefix`

設定項目 `STATIC_URL` で設定されたスタティックファイルのプリフィクスを返します。

```django
{% load static %}
{% get_static_prefix as STATIC_PREFIX %}

<img src="{{ STATIC_PREFIX }}images/hi.jpg" alt="Hi!">
<img src="{{ STATIC_PREFIX }}images/hi2.jpg" alt="Hello!">
```

通常スタティックファイルの URL を出力したいときはテンプレートタグ `static` を使用するべきです。

### `if`

変数の値によって出力する文字列を切り替えます。

```django
{% if athlete_list %}
    Number of athletes: {{ athlete_list|length }}
{% elif athlete_in_locker_room_list %}
    Athletes should be out of the locker room soon!
{% else %}
    No athletes.
{% endif %}
```

`if` の後に渡された変数が存在してなおかつ truthy なら（＝ `bool()` に渡したときに `False` を返すなら）、 `endif` までのブロックの中身を出力します。

Python と同じように `elif` `else` を使うこともできます。

### `ifchanged`

値がループのひとつ前の周から変更されたかどうかをチェックします。

引数を渡すかどうかで挙動が異なります。

引数が渡されなかった場合は、 `ifchanged` から `endifchanged` までの内容がループのひとつ前の周と異なる場合のみその内容を出力します。
次の例では、日付の一覧を出力する中で、月が変わったときにのみ月の見出し（ `{{ date|date:"F" }}` ）を出力します。

```django
<h1>Archive for {{ year }}</h1>

{% for date in days %}
    {% ifchanged %}<h3>{{ date|date:"F" }}</h3>{% endifchanged %}
    <a href="{{ date|date:"M/d"|lower }}/">{{ date|date:"j" }}</a>
{% endfor %}
```

引数が渡された場合は、渡された引数がループのひとつ前の周と変わった場合のみ `ifchanged` から `endifchanged` までの内容を出力します。
次の例では、日付と時刻の一覧を出力する中で、日付が変わったときにのみ日付を出力します。

```django
{% for date in days %}
    {% ifchanged date.date %} {{ date.date }} {% endifchanged %}
    {% ifchanged date.hour date.date %}
        {{ date.hour }}
    {% endifchanged %}
{% endfor %}
```

### `ifequal` / `ifnotequal`

`ifequal` と `ifnotequal` は 2 つの変数の値が等しいかどうかで出力内容を変更するためのタグですが、どちらも `if` で等価なコードが書けるため deprecated となっています。

```django
{% if a == b %}
    ...
{% else %}
    ...
{% endif %}
```

### `include`

他のテンプレートを読み込みます。

```django
{% include "sidebar.html" %}
```

読み込まれたテンプレートでは読み込み側のコンテキストがそのまま利用できます。

`with` を使うとコンテキストを追加（または上書き）することができます。

```django
{% include "sidebar.html" with page_type="detail" %}
```

`with` の末尾に `only` をつけると、読み込まれた方のテンプレートでは明示的に渡されたコンテキストのみが利用できます。

```django
{% include "sidebar.html" with page_type="detail" only %}
```

### `load`

カスタムテンプレートタグのセットを読み込みます。

```django
{% load static %}

{% get_media_prefix %}
```

次のようにネストされたテンプレートタグのセットを読み込むには `.` を使用します。

```text
blogs/
    templatetags/
        articles/
            __init__.py
            utils.py
        __init__.py
    ...
```

```django
{% load articles.utils %}
```

`from` を使えば特定のタグやフィルタだけを個別に読み込むこともできます。

```django
{% load get_media_prefix get_static_prefix from static %}

{% get_media_prefix %}
{% get_static_prefix %}
```

複数のタグセットをまとめて読み込むこともできます。

```django
{% load static i18n %}
```

### `localize`

範囲内の変数のローカライズの有効・無効を切り替えます。

```django
{% load l10n %}

{% localize on %}
    {{ value }}
{% endlocalize %}

{% localize off %}
    {{ value }}
{% endlocalize %}
```

設定項目 `USE_L10N` よりも細かい粒度での制御ができます。

### `lorem`

ランダムな "lorem ipsum" ラテン語テキストを表示します。

```django
{% lorem [count] [method] [random] %}
```

オプション引数の意味合いは次のとおりです。

- `count`: 要素数。デフォルトは 1 。
- `method`: `w` は単語を、 `p` は HTML パラグラフ（ `<p>` ）を、 `b` はプレーンテキストのパラグラフブロックを出力する。
- `random`: `random` が渡されたときはランダムな文字列を、渡されなかったときは固定の文字列を出力する。

使用例:

```django
{# プレーンテキストのパラグラフを 1 つ出力する #}
{% lorem %}

{# HTML のパラグラフを 10 個出力する #}
{% lorem 10 p %}

{# 単語を 20 個ランダムで出力する #}
{% lorem 20 w random %}
```

### `now`

現在の日時を指定されたフォーマットで出力します。

```django
現在の日時は {% now "Y/m/d H:i:s" %} です。
```

以下のいずれかの設定項目で定義されたフォーマットを使うこともできます。

- `DATE_FORMAT`
- `DATETIME_FORMAT`
- `SHORT_DATE_FORMAT`
- `SHORT_DATETIME_FORMAT`

例:

```django
本日の日付は {% now "DATE_FORMAT" %} です。
```

`as` を使うことで結果を変数に格納し別の場所で利用することもできます。

```django
{% now "Y" as current_year %}
{% blocktrans %}Copyright {{ current_year }}{% endblocktrans %}
```

### `regroup`

```django
```

### `resetcycle`

```django
```

### `spaceless`

```django
```

### `static`

```django
```

### `templatetag`

```django
```

### `trans`

```django
```

### `url`

```django
```

### `verbatim`

```django
```

### `widthratio`

```django
```

### `with`

```django
```


## テンプレートフィルタ

### `add`

```django
```

### `addslashes`

```django
```

### `apnumber`

```django
```

### `capfirst`

```django
```

### `center`

```django
```

### `cut`

```django
```

### `date`

```django
```

### `default_if_none`

```django
```

### `default`

```django
```

### `dictsort`

```django
```

### `dictsortreversed`

```django
```

### `divisibleby`

```django
```

### `escape`

```django
```

### `escapejs`

```django
```

### `filesizeformat`

```django
```

### `first`

```django
```

### `floatformat`

```django
```

### `force_escape`

```django
```

### `get_current_timezone`

```django
```

### `get_digit`

```django
```

### `intcomma`

```django
```

### `intword`

```django
```

### `iriencode`

```django
```

### `join`

```django
```

### `json_script`

```django
```

### `language_bidi`

```django
```

### `language_name_local`

```django
```

### `language_name_translated`

```django
```

### `language_name`

```django
```

### `last`

```django
```

### `length_is`

```django
```

### `length`

```django
```

### `linebreaks`

```django
```

### `linebreaksbr`

```django
```

### `linenumbers`

```django
```

### `ljust`

```django
```

### `localize`

```django
```

### `localtime`

```django
```

### `lower`

```django
```

### `make_list`

```django
```

### `naturalday`

```django
```

### `naturaltime`

```django
```

### `ordinal`

```django
```

### `phone2numeric`

```django
```

### `pluralize`

```django
```

### `pprint`

```django
```

### `random`

```django
```

### `rjust`

```django
```

### `safe`

```django
```

### `safeseq`

```django
```

### `slice`

```django
```

### `slugify`

```django
```

### `stringformat`

```django
```

### `striptags`

```django
```

### `time`

```django
```

### `timesince`

```django
```

### `timeuntil`

```django
```

### `timezone`

```django
```

### `title`

```django
```

### `truncatechars_html`

```django
```

### `truncatechars`

```django
```

### `truncatewords_html`

```django
```

### `truncatewords`

```django
```

### `unlocalize`

```django
```

### `unordered_list`

```django
```

### `upper`

```django
```

### `urlencode`

```django
```

### `urlize`

```django
```

### `urlizetrunc`

```django
```

### `utc`

```django
```

### `wordcount`

```django
```

### `wordwrap`

```django
```

### `yesno`

```django
```

## 参考

- [Built-in template tags and filters | Django documentation | Django](https://docs.djangoproject.com/en/3.0/ref/templates/builtins/)
- [`django.contrib.humanize` | Django documentation | Django](https://docs.djangoproject.com/en/3.0/ref/contrib/humanize/)
- [Translation | Django documentation | Django](https://docs.djangoproject.com/en/3.0/topics/i18n/translation/#specifying-translation-strings-in-template-code)
- [Format localization | Django documentation | Django](https://docs.djangoproject.com/en/3.0/topics/i18n/formatting/#topic-l10n-templates)
- [Time zones | Django documentation | Django](https://docs.djangoproject.com/en/3.0/topics/i18n/timezones/#time-zones-in-templates)
- [Custom template tags and filters | Django documentation | Django](https://docs.djangoproject.com/en/3.0/howto/custom-template-tags/)
