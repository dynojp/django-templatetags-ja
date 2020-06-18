# Django テンプレートタグまとめ

Python ベースのウェブアプリケーションフレームワーク Django のコアが提供するテンプレートタグ・テンプレートフィルタの一覧です。

（サンプルコードのほとんどは公式ドキュメントのものです）

- テンプレートタグ
    - `autoescape`
    - `block`
    - `blocktrans`
    - `comment`
    - `csrf_token`
    - `cycle`
    - `debug`
    - `extends`
    - `filter`
    - `firstof`
    - `for`
    - `for` … `empty`
    - `get_available_languages`
    - `get_current_language`
    - `get_current_language_bidi`
    - `get_language_info`
    - `get_language_info_list`
    - `get_media_prefix`
    - `get_static_prefix`
    - `if`
    - `ifchanged`
    - `ifequal` / `ifnotequal`
    - `include`
    - `load`
    - `localize`
    - `lorem`
    - `now`
    - `regroup`
    - `resetcycle`
    - `spaceless`
    - `static`
    - `templatetag`
    - `trans`
    - `url`
    - `verbatim`
    - `widthratio`
    - `with`
- テンプレートフィルタ
    - `add`
    - `addslashes`
    - `apnumber`
    - `capfirst`
    - `center`
    - `cut`
    - `date`
    - `default_if_none`
    - `default`
    - `dictsort`
    - `dictsortreversed`
    - `divisibleby`
    - `escape`
    - `escapejs`
    - `filesizeformat`
    - `first`
    - `floatformat`
    - `force_escape`
    - `get_current_timezone`
    - `get_digit`
    - `intcomma`
    - `intword`
    - `iriencode`
    - `join`
    - `json_script`
    - `language_bidi`
    - `language_name_local`
    - `language_name_translated`
    - `language_name`
    - `last`
    - `length_is`
    - `length`
    - `linebreaks`
    - `linebreaksbr`
    - `linenumbers`
    - `ljust`
    - `localize`
    - `localtime`
    - `localtime`
    - `lower`
    - `make_list`
    - `naturalday`
    - `naturaltime`
    - `ordinal`
    - `phone2numeric`
    - `pluralize`
    - `pprint`
    - `random`
    - `rjust`
    - `safe`
    - `safeseq`
    - `slice`
    - `slugify`
    - `stringformat`
    - `striptags`
    - `time`
    - `timesince`
    - `timeuntil`
    - `timezone`
    - `title`
    - `truncatechars_html`
    - `truncatechars`
    - `truncatewords_html`
    - `truncatewords`
    - `unlocalize`
    - `unordered_list`
    - `upper`
    - `urlencode`
    - `urlize`
    - `urlizetrunc`
    - `utc`
    - `wordcount`
    - `wordwrap`
    - `yesno`

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
{% extends 'base.html' %}

{% block main_content %}
{% endblock main_content %}
```

`endblock` の後の名前は付けても付けなくても問題ありません。

継承元の内容を上書きするのではなく追加したいときは `block.super` 変数を使用します。

```django
{% extends 'base.html' %}

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
    <tr class="{% cycle 'odd' 'even' %}">
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
{% extends 'base.html' %}
```

```django
{% extends 'blog.html' %}
```

いっしょに `block` タグを使ってブロックを上書きして使用します。

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
{# 'ja' #}
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

戻り値は `get_language_info` の戻り値と同等のオブジェクトを要素に持つリストです。

### `get_media_prefix`

```django
```

### `get_static_prefix`

```django
```

### `if`

```django
```

### `ifchanged`

```django
```

### `ifequal` / `ifnotequal`

```django
```

### `include`

```django
```

### `load`

```django
```

### `localize`

```django
```

### `lorem`

```django
```

### `now`

```django
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
