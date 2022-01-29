# はじめての Django アプリ作成 その 2

[はじめての Django アプリ作成、その 2](https://docs.djangoproject.com/ja/4.0/intro/tutorial02/)

## DB の設定

デフォルトでは sqlite3 を利用するように settings.py で設定されている。
変更する場合は設定を見直す。
今回はそのまま sqlite3 を利用する。

## マイグレーション

settings.py の INSTALL_APPS の設定に従って migration を実行する。

```
$ python manage.py migrate
```

## モデルの作成

models.py に DB テーブルと一致させる Model クラスを作成します。

```python:polls/models.py
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

Question クラスには VARCHAR(200)の question_text と DATETIME の pub_date が存在します。

Choice クラスには Queston の外部キーの question、VARCHAR(200)の choice_text、INTEGER の votes が存在します。

以下のコマンドを実行すると、DB にテーブルを作成、変更する SQL が作成されます。

```
$ python manage.py makemigrations polls
```

以下のコマンドを実行すると、DB にテーブルを作成する SQL を確認することができます。

001 は今回作成された SQL ファイルの番号

```
$ python manage.py sqlmigrate polls 001
```

以下のコマンドを実行することで、実際に DB にテーブルを作成できます。

```
$ python manage.py migrate
```

## API であそんでみる

```
$ python manage.py shell
```

## 管理ユーザーを作成する

```
$ python manage.py createsuperuser

Username: admin

Email address: yukanda_corp@yahoo.co.jp

Password: *******
Password（again）: ******
```

http://localhost:8000/admin/

## Poll アプリを admin 上で編集できるようにする

```python:admin.py
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```
