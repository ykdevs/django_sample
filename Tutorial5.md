# はじめての Django アプリ作成 その 4

[はじめての Django アプリ作成、その 4](https://docs.djangoproject.com/ja/4.0/intro/tutorial04/)

## generic.ListView

| 変数名              | 説明                                                                          |
| ------------------- | ----------------------------------------------------------------------------- |
| template_name       | テンプレート名                                                                |
| context_object_name | get_queryset メソッドで取得したリストを保存する変数名。デフォルト object_list |
| queryset            | get_queryset メソッドを使わず、１行で設定する場合に利用する変数。             |

- 【参考】[【Django】ListView の使い方と出来ること](https://qiita.com/aqmr-kino/items/d536e08a715a9fad5720)

### 複数のリストを扱う場合

get_context_data メソッドで context を取得する。

```python:view.py
    def get_context_data(self, *args, **kwargs):
        context = super().get_context_data(*args, **kwargs)
        context['tag_list'] = Tag.objects.all
        return context
```

テンプレートでは context に設定したキー名で取得できる。

```html:list.html
        <div class="text">
          <ul>
            {% for tag in tag_list %}
            <li>{{ tag.name }}</li>
            {% endfor %}
          </ul>
        </div>
```

- 【参考】[Django の Listview で複数モデルのデータを取り出す方法](https://qiita.com/yongjugithub/items/edd69e1ac6d4507f9ad1)

### Django でテーブルを結合する方法

Django でデータベース操作を行うには ORM という技法を利用します。

- 【参考】[【Django】データベース操作：テーブルを結合する方法](https://office54.net/python/django/database-combine-table)
- 【参考】[select_related と prefetch_related](https://just-python.com/document/django/orm-query/select_related-prefetch_related)
- 【参考】[Django で prefetch_related を使ってクエリ数を減らす](https://tokibito.hatenablog.com/entry/20140718/1405691738)

#### select_relatd()メソッド

```
book = Book.objects.select_related('author').get(pk=4)
# １回目のSQLを発行
author = book.author
# 前のクエリでbook.authorのデータは保持しているのでSQLは発行しません
```

モデルで related_name を設定。（デフォルトは choice_set）

```
class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE, related_name='choices')
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```
