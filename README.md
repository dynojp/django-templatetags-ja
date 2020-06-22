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
    - [`default`](#default)
    - [`default_if_none`](#default_if_none)
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
ページ上で確認したいときは `{% filter force_escape %}{% endfilter %}` 等をあわせて使うと便利です。

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

| アトリビュート | 内容 |
| --- | --- |
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

| アトリビュート | 内容 |
| --- | --- |
| `lang_info.code` | 言語コード |
| `lang_info.name_local` | ローカルな名前 |
| `lang_info.name` | 英語での名前 |
| `lang_info.bidi` | 右から左に読む言語かどうか |
| `lang_info.name_translated` | アクティブな言語での名前 |

### `get_language_info_list`

`for` で指定された言語コードに対応する言語情報の一覧を返します。

```django
{% load i18n %}
{% get_language_info_list for lang_codes as lang_infos %}
{% for lang_info in lang_infos %}
    {{ ... }}
{% endfor %}
```

戻り値は `get_language_info` の戻り値と同等のオブジェクトを要素に持つ `list` です。

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

`if` の後に渡された変数が存在してなおかつ truthy なら（＝ `bool()` に渡したときの戻り値が `False` なら）、 `endif` までのブロックの中身を出力します。

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

共通のアトリビュートを持ったオブジェクトからなる `list` をアトリビュートの値でグルーピングします。

文章での説明よりも例の方がわかりやすいです。
次のような `list` 変数 `cities` があるときに

```django
cities = [
    {'name': 'Mumbai', 'population': '19,000,000', 'country': 'India'},
    {'name': 'Calcutta', 'population': '15,000,000', 'country': 'India'},
    {'name': 'New York', 'population': '20,000,000', 'country': 'USA'},
    {'name': 'Chicago', 'population': '7,000,000', 'country': 'USA'},
    {'name': 'Tokyo', 'population': '33,000,000', 'country': 'Japan'},
]
```

`regroup` タグを使えば次のようなリストを表す HTML を比較的かんたんに生成することができます。

- India
    - Mumbai: 19,000,000
    - Calcutta: 15,000,000
- USA
    - New York: 20,000,000
    - Chicago: 7,000,000
- Japan
    - Tokyo: 33,000,000

具体的なコードは次のとおりです。

```django
{% regroup cities by country as country_list %}

<ul>
{% for country in country_list %}
    <li>{{ country.grouper }}
    <ul>
        {% for city in country.list %}
          <li>{{ city.name }}: {{ city.population }}</li>
        {% endfor %}
    </ul>
    </li>
{% endfor %}
</ul>
```

`regroup` の戻り値 `country_list` は、各要素が `GroupedResult` オブジェクトの `list` です。
`GroupedResult` オブジェクトは `grouper` と `list` という 2 つのフィールドを持つ `namedtuple` です。

`GroupedResult` オブジェクトは `namedtuple` なので、同じ処理を次のような形でも記述できます。

```django
{% regroup cities by country as country_list %}

<ul>
{% for country, local_cities in country_list %}
    <li>{{ country }}
    <ul>
        {% for city in local_cities %}
          <li>{{ city.name }}: {{ city.population }}</li>
        {% endfor %}
    </ul>
    </li>
{% endfor %}
</ul>
```

グルーピングのキーには、アトリビュートだけでなく、参照先のオブジェクトのアトリビュートや、引数を持たないメソッド等を使用することも可能です。

`regroup` は元の値がグルーピングのキーに使われるアトリビュートでソート済みでないと正しく動かないので注意が必要です（自動的に対象のアトリビュートでソートしてからグルーピングしてくれるわけではありません）。

### `resetcycle`

`cycle` タグのループを初期状態にリセットします。

例えば次のようにすると、各 `coach` の最初の `<p>` タグのクラスが必ず `odd` になります。

```django
{% for coach in coach_list %}
    <h1>{{ coach.name }}</h1>
    {% for athlete in coach.athlete_set.all %}
        <p class="{% cycle 'odd' 'even' %}">{{ athlete.name }}</p>
    {% endfor %}
    {% resetcycle %}
{% endfor %}
```

複数の `cycle` のループが回っている場合は、 `cycle` に `as` を付けて名前を付けることにより、特定の `cycle` だけをリセットすることができます。

```django
{% for item in list %}
    <p class="{% cycle 'odd' 'even' as stripe %} {% cycle 'major' 'minor' 'minor' 'minor' 'minor' as tick %}">
        {{ item.data }}
    </p>
    {% ifchanged item.category %}
        <h1>{{ item.category }}</h1>
        {% if not forloop.first %}{% resetcycle tick %}{% endif %}
    {% endifchanged %}
{% endfor %}
```

### `spaceless`

範囲内の空白文字を除去します。

```django
{% spaceless %}
    <p>
        <a href="foo/">あ い　う </a>
    </p>
{% endspaceless %}
```

上のコードは次の HTML を出力します。

```html
<p><a href="foo/">あ い　う </a></p>
```

タブ文字や改行も除去対象に含まれます。

ただし、除去されるのはタグとタグの間の空白文字だけです。
タグとテキストの間、テキストとテキストの間の空白文字は除去されません。

### `static`

設定項目 `STATIC_ROOT` 以下のスタティックファイルに対応する URL を生成します。

```django
{% load static %}
<img src="{% static "images/hi.jpg" %}" alt="Hi!">
```

その場で出力したくない場合は `as` を使えば出力せずに変数に格納できます。

```django
{% static "images/hi.jpg" as myphoto %}
<img src="{{ myphoto }}">
```

### `templatetag`

Django テンプレートにおける特殊な文字列を出力します。

```django
{% templatetag openblock %} url 'entry_list' {% templatetag closeblock %}
```

上のコードは次の HTML を出力します。

```html
{% url 'entry_list' %}
```

| 引数 | 出力 |
| --- | --- |
| `openblock` | `{%` |
| `closeblock` | `%}` |
| `openvariable` | `{{` |
| `closevariable` | `}}` |
| `openbrace` | `{` |
| `closebrace` | `}` |
| `opencomment` | `{#` |
| `closecomment` | `#}` |

### `trans`

対象の文字列の翻訳を出力します。

```django
{% load i18n %}
<title>{% trans "This is the title." %}</title>
```

内部的には `django.utils.translation.gettext()` を使用します。

`trans` はテンプレート変数を含むテキストの翻訳には使えません。
テンプレート変数を含むテキストの翻訳には `blocktrans` を使用してください。

翻訳後の文字列を複数箇所で使いたい場合は `as` を使って変数に格納できます。

```django
{% load i18n %}
{% trans "This is the title" as the_title %}

<title>{{ the_title }}</title>
<meta name="description" content="{{ the_title }}">
```

コンテキスチュアルマーカーを付与したい場合は `context` キーワードが使用できます。

```django
{% trans "May" context "month name" %}
```

尚、タグやフィルタに文字列リテラルの翻訳を渡したいときは `_()` シンタックスが使えます。

```django
{% some_tag _("Page not found") value|yesno:_("yes,no") %}
```

### `url`

特定のビューに対応した絶対パスを生成します。

```django
{% url 'articles:list' %}
```

第 1 引数には URL パターン名を渡します。
URL パターン名に名前空間を含む場合は、上の例のように `:` で区切ります。

URL パラメータを渡したい場合は第 2 引数以降で指定できます。

```django
{% url 'some-url-name' v1 v2 %}
```

URL パラメータは positional 引数でも keyword 引数でも指定することが可能です。

```django
{% url 'some-url-name' arg1=v1 arg2=v2 %}
```

必須の URL パラメータはすべて渡さないとエラーが出ます。
このエラーがよく起こるのは、引数として渡した変数が存在しない場合です。

```django
{% for article in article_list %}
  {# タイポ `artcle` で存在しない変数を指定した結果エラーが起こる #}
  <a href="{% url "articles:detail" artcle.id %}">{{ article.title }}</a>
{% endfor %}
```

### `verbatim`

Django テンプレートエンジンによる評価を無効化します。

```django
{% verbatim %}
    {{if dying}}Still alive.{{/if}}
{% endverbatim %}
```

範囲内の文字列をテンプレートとして評価せずそのまま出力します。
典型的な使いどころは、 Django と似たテンプレートエンジンを持つ JavaScript を出力する場合です。

### `widthratio`

高さに対応した幅を出力します。

```django
<img src="bar.png" alt="Bar"
     height="10" width="{% widthratio this_value max_value max_width %}">
```

各変数の値が `this_value` が 175 、 `max_value` が 200 、 `max_width` が 100 のとき、この `widthratio` の出力は 88 になります（ `88 = round(175 / 200 * 100)` ）。

### `with`

範囲内でのみ有効な一時的な変数を定義します。

```django
{% with total=business.employees.count %}
    {{ total }} employee{{ total|pluralize }}
{% endwith %}
```

複数の変数をまとめて定義することもできます。

```django
{% with alpha=1 beta=2 %}
    ...
{% endwith %}
```

古いシンタックス `{% with business.employeees.count as total %}` もまだ使用できます。

## テンプレートフィルタ

### `add`

対象の値に引数で渡された値を追加します。

```django
{{ value|add:"2" }}
```

`add` フィルタは最初に各値を `int` に変換して追加しようとします。
それに失敗したらそのまま追加しようとします。
それも失敗した場合は空文字列を返します。

上の例で `value` の値が 10 のとき、出力は 12 になります。

`+` 演算子が使える要素同士であればどのような値にも使えるので、例えば、 `values1` と `values2` がともに `list` であれば次のようなこともできます。

```django
{# values1 と values2 の値を順番に出力する #}
{% for value in values1|add:values2 %}
  <li>{{ value }}</li>
{% endfor %}
```

### `addslashes`

文字列内の引用符 `'` と `"` の前にバックスラッシュを追加します。

```django
{{ value|addslashes }}
```

`value` が `I'm using Django.` の場合、出力は `I\'m using Django` になります。

CSV 内のセルのエスケープ等に有用とされています。

### `apnumber`

1 から 9 までの数字をアルファベット表現に変換します。
1 未満と 10 以上の数字はそのまま返します。

```django
{% load humanize %}
<ul>
  {% for number in numbers %}
    <li>{{ number|apnumber }}</li>
  {% endfor %}
</ul>
```

例えばこのコードは以下のリストに相当する HTML を生成します。

- 0
- one
- two
- three
- four
- five
- six
- seven
- eight
- nine
- 10
- 11
- 12

公式サイトではこれは「 Associated Press スタイル」（ AP 通信のスタイルブックのスタイル？）に従うと説明されています。

尚このタグを利用するには、設定項目 `INSTALLED_APPS` に `django.contrib.humanize` を追加する必要があります。

### `capfirst`

先頭文字を大文字に変換します。
先頭文字がアルファベットではない場合は効果はありません。

```django
{{ value|capfirst }}
```

### `center`

指定された字数の中で文字列を中央揃えにして出力します。

```django
<pre>
"{{ value|center:"15" }}"
</pre>
```

上のコードで `value` が `Django` のとき、出力は `"     Django    "` になります。

### `cut`

文字列内の特定の文字を除去します。

```django
{{ value|cut:"ん" }}
```

上のコードで `value` が `にんじん` のとき、出力は `にじ` になります。

除去できる文字の種類は 1 文字だけです。
内部では `str.replace()` メソッドが使われています。

### `date`

日付オブジェクトを指定されたフォーマットで出力します。

```django
{{ value|date:"Y/m/d (D) H:i:s" }}
```

上のコードで `value` が `datetime.datetime(2018, 5, 27, 3, 32, 30)` の場合、出力は `2018/05/27 (Sun) 03:32:30` になります。

フォーマット指定には PHP の `date()` 関数と似たフォーマット指定子が利用できます（ PHP からの移行をかんたんにするために意図的に PHP 互換で作られたとのことです）。

以下のいずれかの設定項目で定義されたフォーマットを使うこともできます。

- `DATE_FORMAT`
- `DATETIME_FORMAT`
- `SHORT_DATE_FORMAT`
- `SHORT_DATETIME_FORMAT`

例:

```django
投稿日: {% value|date:"DATE_FORMAT" %}
```

フォーマットが省略されたときは `DATE_FORMAT` のフォーマットが使用されます。

```django
{{ value|date }}
```

### `default`

値が falsy （＝ `bool()` に渡されたときの戻り値が `False` ）の場合に、引数で指定された文字列を出力します。値が truthy の場合はそのまま出力します。

```django
{{ value|default:"nothing" }}
```

### `default_if_none`

値が `None` の場合に引数で指定された文字列を出力します。値が `None` 以外の場合はそのまま出力します。

```django
{{ value|default_if_none:"nothing" }}
```

変数が存在しない場合は何も出力しません。

### `dictsort`

各要素が `dict` の `list` を引数で指定されたキーでソートします。
順序は昇順です。

```django
{{ value|dictsort:"age" }}
```

上のコードで `value` の値が次の `list` のとき

```python
[
    {'name': 'ポチ', 'age': 3},
    {'name': 'タマ', 'age': 1},
    {'name': 'タロウ', 'age': 10},
]
```

次の出力になります。

```html
[{'name': 'タマ', 'age': 1}, {'name': 'ポチ', 'age': 3}, {'name': 'タロウ', 'age': 10}]
```

`for` タグや `with` タグと組み合わせて使用すると有用です。

```django
<ul>
  {% for item in d|dictsort:"age" %}
    <li>{{ item.name }}: {{ item.age }}</li>
  {% endfor %}
</ul>
```

出力のイメージ:

- タマ: 1
- ポチ: 3
- タロウ: 10

ネストされた　`dict` に対してキーを指定することもできます。

```django
{% for book in books|dictsort:"author.age" %}
    * {{ book.title }} ({{ book.author.name }})
{% endfor %}
```

`dictsort` は内部で `__getitem__` を使用するので、各要素が `dict` の `list` だけでなく、各要素が `list` や `tuple` の `list` にも使えます。

```django
{{ value|dictsort:0 }}
```

### `dictsortreversed`

各要素が `dict` の `list` を引数で指定されたキーでソートします。
ただし、順序が `dictsort` とは異なり降順です。

### `divisibleby`

値が指定された引数で割り切れる場合に `True` を返します。

```django
{{ value|divisibleby:"3" }}
```

### `escape`

HTML の特殊文字をエスケープします。

変換される文字は以下のとおりです。

| 変換前 | 変換後 |
| --- | --- |
| `<` | `&lt;` |
| `>` | `&gt;` |
| `'` | `&#x27;` |
| `"` | `&quot;` |
| `&` | `&amp;` |

```django
{{ value|escape }}
```

このエスケープ処理はデフォルトで有効になっているため、通常のコンテキストでは効果はありません（エスケープが二重に走ったりはしません）。

`escape` フィルタは `autoescape` タグで自動エスケープを無効にしたときに選択的にエスケープをしたいときに有用です。

```django
{% autoescape off %}
    {{ title|escape }}
{% endautoescape %}
```

### `escapejs`

JavaScript の文字列として使われる値をエスケープします。

```django
<script>
var value = "{{ value|escapejs }}";
</script>
```

上のコードで `value` が文字列 `testing\r\njavascript 'string" <b>escaping</b>` のとき、出力は `testing\u000D\u000Ajavascript \u0027string\u0022 \u003Cb\u003Eescaping\u003C/b\u003E` になります。

文字列の中で使うと HTML シンタックスエラーを防いでくれますが、 JavaScript のテンプレートリテラルといっしょに使うと XSS の危険があるためテンプレートリテラルと併用すべきではありません。
例えば、次のコードで `value` に `${alert(100)}` という文字列が渡されると ブラウザが `alert(100)` を実行してしまいます。

```django
<script>
var value = `{{ value|escapejs }}`;
</script>
```

Python から JavaScript に値を受け渡ししたい場合は後から追加された `json_script` フィルタを使うと便利です。

### `filesizeformat`

ファイルサイズの数字を人間が読みやすい形に変換します。

```django
{{ value|filesizeformat }}
```

例:

- `13 KB`
- `4.1 MB`
- `102 bytes`

### `first`

`list` または `tuple` の最初の要素を返します。

```django
{{ value|first }}
```

最初の要素が取得できないときは何も出力しません。
内部では `value[0]` で最初の要素を取得するので、インデックスアクセスのできない iterator はサポートしていません。

### `floatformat`

指定された桁数で浮動小数点をフォーマットします。

```django
{{ value|floatformat:3 }}
```

引数には正の整数と負の整数が使えます。
引数が正の数の場合は、小数点以下指定された桁を残してフォーマットします。

| value | Template | Output |
| --- | --- | --- |
| `34.23234` | `{{ value|floatformat:3 }}` | 34.232 |
| `34.00000` | `{{ value|floatformat:3 }}` | `34.000` |
| `34.26000` | `{{ value|floatformat:3 }}` | `34.260` |

引数が負の数の場合は、基本的には正の数の場合と同じですが、値が整数とみなせる場合は小数点以下を出力しません。

| value | Template | Output |
| --- | --- | --- |
34.23234    {{ value|floatformat:"-3" }}    34.232
34.00000    {{ value|floatformat:"-3" }}    34
34.26000    {{ value|floatformat:"-3" }}    34.260

引数を省略することもでき、そのときの挙動は引数が `-1` のときの挙動になります。

| value | Template | Output |
| --- | --- | --- |
| `34.23234` | `{{ value|floatformat }}` | `34.2` |
| `34.00000` | `{{ value|floatformat }}` | `34` |
| `34.26000` | `{{ value|floatformat }}` | `34.3` |

### `force_escape`

HTML の特殊文字をエスケープします。
処理内容は `escape` と同じですが、 `escape` がエスケープ処理が重複して起こらないようにするのに対し、 `force_escape` はその場でエスケープ処理を必ず行います。

```django
{% autoescape off %}
    {{ body|linebreaks|force_escape }}
{% endautoescape %}
```

`filter` タグと組み合わせて使うとタグの出力を強制的にエスケープすることができます。

```django
<pre>
  {% filter force_escape %}
  {% debug %}
  {% endfilter %}
</pre>
```

レアケースを除き通常は `escape` を使います。

### `get_current_timezone`

現在アクティブなタイムゾーンを返します。

```django
{% load tz %}
{% get_current_timezone as TIME_ZONE %}
{{ TIME_ZONE }}
```

必ず `as` を用いて変数に格納する必要があります。

同じ値を取得する別の方法として、コンテキストプロセッサ `django.template.context_processors.tz` とコンテキスト変数 `TIME_ZONE` を使う方法もあります。

### `get_digit`

渡された値を `int` とみなして、引数で指定された桁目の数字を返します。

```django
{{ value|get_digit:"2" }}
```

上のコードで `value` が `50134` の場合、出力は `3` になります。

`int` とみなせない値には何も処理を加えずそのまま返します。
渡された値の桁数が引数で指定された桁よりも少ない場合は `0` を返します。

### `intcomma`

`int` （または `float` ）にみなせる値を 3 桁ごとに `,` を付けた表現に変換します。

```django
{% load humanize %}
{{ value|intcomma }}
```

例:

| 変換前 | 変換後 |
| --- | --- |
| `4500` | `4,500` |
| `4500.2` | `4,500.2` |
| `45000` | `45,000` |
| `450000` | `450,000` |
| `4500000` | `4,500,000` |

尚このタグを利用するには、設定項目 `INSTALLED_APPS` に `django.contrib.humanize` を追加する必要があります。

設定項目 `USE_L10N` と `USE_THOUSAND_SEPARATOR` が共に `True` の場合、 `intcomma` と同様の処理が自動で行われます。

### `intword`

`int` とみなせる値を人間が読みやすい表現に変換します。

```django
{% load humanize %}
{{ value|intword }}
```

```django
{% load humanize %}
<ul>
  <li>{{ 10000|intword }}</li>
  <li>{{ 1000000|intword }}</li>
  <li>{{ 10000000000|intword }}</li>
  <li>{{ 10000000000000|intword }}</li>
  <li>{{ 10000000000000000|intword }}</li>
  <li>{{ 10000000000000000000|intword }}</li>
</ul>
```

上のコードは次のリストに対応する HTML を出力します。

- 10000
- 1.0 million
- 10.0 billion
- 10.0 trillion
- 10.0 quadrillion
- 10.0 quintillion

`10^100` （ Googol ）までがサポートされているとのことです。

言語を日本語にする場合は使うシーンがあまり無さそうです。

尚このタグを利用するには、設定項目 `INSTALLED_APPS` に `django.contrib.humanize` を追加する必要があります。

### `iriencode`

IRI 文字列を ASCII 文字のみからなる URI にエンコードします。

```django
{{ value|iriencode }}
```

特定の文字列を URL に含めるときに使用します。

例:

| 変換前 | 変換後 |
| --- | --- |
| `?test=1&me=2` | `?test=1&amp;me=2` |
| `/news/犬/` | `/news/%E7%8A%AC/` |
| `/news/🐶/` | `/news/%F0%9F%90%B6/` |

IRI は Internationalized Resource Identifier の、 URI は Universal Resource Identifier の略語です。

参考:

- [Internationalized Resource Identifiers (IRIs)](https://www.w3.org/International/O-URL-and-ident.html)

内部では `django.utils.encoding.iri_to_uri()` が使用されます。

### `join`

`list` 等の iterable を引数で指定された文字で結合します。
ちょうど Python の `str.join()` と似た挙動をします。

```django
{{ value|join:' / ' }}
```

上のコードで `value` が `['<a href="/">ホームへ</a>', '<strong>強調</strong>']` の場合の出力は次のとおりです。

```
&lt;a href="/"&gt;リンク&lt;/a&gt; / &lt;strong&gt;強調&lt;/strong&gt;
```

iterable の各要素は `join` の時点でエスケープされます。

### `json_script`

Python オブジェクトを JSON として安全に出力し `<script>` タグで囲います。
Python から JavaScript に値を渡すときに使用します。

```django
{{ value|json_script:"hello-data" }}
```

上のコードで `value` が `{'hello': 'world'}` という `dict` の場合、次の内容が出力されます。

```html
<script id="hello-data" type="application/json">{"hello": "world"}</script>
```

引数には `<script>` タグの `id` アトリビュートの値を指定します。

XSS 攻撃対策として `<` と `>` と `&` をエスケープします。

この値は次の形で JavaScript でアクセスできます。

```js
var value = JSON.parse(document.getElementById('hello-data').textContent);
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
{% load humanize %}
```

尚このタグを利用するには、設定項目 `INSTALLED_APPS` に `django.contrib.humanize` を追加する必要があります。

### `naturaltime`

```django
{% load humanize %}
```

尚このタグを利用するには、設定項目 `INSTALLED_APPS` に `django.contrib.humanize` を追加する必要があります。

### `ordinal`

```django
{% load humanize %}
```

尚このタグを利用するには、設定項目 `INSTALLED_APPS` に `django.contrib.humanize` を追加する必要があります。

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
