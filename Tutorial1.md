# はじめての Django アプリ作成 その 1

[はじめての Django アプリ作成、その 1](https://docs.djangoproject.com/ja/4.0/intro/tutorial01/)

## アプリケーションの作成

sample という名称のアプリケーションができる。

```
$ django-admin startproject sample

sample ・・・ manage.py が置かれる
│
└─ sample ・・・ sampleプロジェクトの設定
　　 │
　　 ├─ asgi.py     ・・・ プロジェクトを提供する ASGI 互換 Web サーバーのエントリポイント
　　 ├─ settings.py ・・・ Django プロジェクトの設定ファイル
　　 ├─ urls.py     ・・・ Django プロジェクトの URL 宣言
　　 └─ wsgi.py     ・・・ プロジェクトをサーブするためのWSGI互換Webサーバーとのエントリーポイント
```

- ASGI(Asynchronous Server Gateway Interface) ・・・ WSGI の精神的な後継仕様であり、asyncio を介して非同期で動作するように設計されていて、また WebSocket など複数のプロトコルをサポートしています。
- WSGI(Web Server Gateway Interface) ・・・ Python において、Web サーバと Web アプリケーションが通信するための、標準化されたインタフェース定義

- 【参考】[サクッと WSGI・ASGI に触れてみる](https://okiyasi.hatenablog.com/entry/2020/08/10/211804)

## 開発用サーバの起動

以下のコマンドで、開発用のサーバを起動し確認ができる。

```
$ python manage.py runserver
```

http://localhost:8000/

## Polls アプリケーションを作る

```
$ python manage.py startapp polls

sample ・・・ manage.py が置かれる
│
└─ polls ・・・ polls アプリケーション
　　│
　　├─ admin.py   ・・・
　　├─ apps.py    ・・・
　　├─ models.py  ・・・ Modelクラスを記述する
　　├─ tests.py   ・・・ Testクラスを記述する
　　├─ urls.py    ・・・ アプリケーション内のURL定義
　　├─ views.py   ・・・ View（表示）クラスを記述する
　　├─ migrations ・・・ Model定義に従って自動でDBをマイグレーションするスクリプト
　　│　　│
　　│　　└─ 0001_initial.py ・・・ マイグレーションするスクリプト
　　└─ templates  ・・・ Viewから呼び出されるHTMLテンプレート
```

### URL 定義

#### プロジェクトの URL 定義

include でアプリケーション urls に処理を委譲する。

```
urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

#### アプリケーションの URL 定義

app_name は名前空間の定義。
urlpatterns に path を設定する。

```
app_name = 'polls'
urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    path('<int:pk>/', views.DetailView.as_view(), name='detail'),
    path('<int:pk>/results/', views.ResultsView.as_view(), name='results'),
    path('<int:question_id>/vote/', views.vote, name='vote'),
]
```

```
path(route, view, kwargs=None, name=None)

route ・・・ルート。パス変数での設定も可能
view  ・・・ディスパッチするViewクラス
kwargs・・・任意のキーワードをViewに渡す
name  ・・・ルートの名前。
```
