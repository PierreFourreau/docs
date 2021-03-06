CakePHP の典型的なリクエスト
############################

CakePHP の基本的な構成要素について既に考えました。ここからは、基本的なリクエストがあった場合、
個々のオブジェクトがそれをどう処理して完結させるのかということを調べてみましょう。
リクエストの例を元に続けます。例えば、リカルドさんが CakePHP アプリケーションの
「特注ケーキを今すぐ購入！」というリンクをクリックしたとしましょう。

.. figure:: /_static/img/typical-cake-request.png
   :align: center
   :alt: Flow diagram showing a typical CakePHP request

   典型的な CakePHP のリクエストを示すフローダイアグラム

図２ 典型的な CakePHP リクエスト

黒 = 必要な要素、灰色 = 任意の要素、青 = コールバック

#. リカルドさんが http://www.example.com/cakes/buy を指しているリンクをクリックすると、
   ブラウザは、ウェブサーバにリクエストを送信します。
#. Router が、URL を解析（parse）し、このリクエストのパラメータを取り出します。
   このリクエストにおいてビジネスロジックに影響するコントローラ、アクション、その他の引数などです。
#. リクエストされた URL は、Router によって、コントローラのアクション（コントローラクラスの
   中にあるメソッド）にマップされます。この場合は、CakesController の buy() メソッドです。
   コントローラのアクションロジックが実行される前には、コントローラの beforeFilter()
   コールバックが呼ばれます。
#. コントローラは、アプリケーションのデータを取り出すためにモデルを使用することができます。
   この例では、リカルドさんの最新の購入履歴をデータベースから取得するため、コントローラが
   モデルを使用します。この操作で、該当するモデルのコールバック、ビヘイビア、データソースが動作します。
   モデルは使用しなくてもかまいませんが、CakePHP のすべてのコントローラは、初期状態では
   少なくともひとつのモデルを使用する設定になっています。
#. モデルがデータを取得すると、それはコントローラに送られます。
   モデルのコールバックが設定されていれば、それが動作します。
#. コントローラはコンポーネントを使用して、データをさらに調整したり、その他の操作
   （セッション操作、認証、Eメールの送信など）ができます。
#. コントローラがモデルとコンポーネントを使用してデータを準備し終えると、コントローラの
   set() メソッドを使用して、ビューにテータを渡します。
   データが送信される前に、コントローラのコールバックがあれば実行されます。
   ビューのロジックが動作すると、エレメントやヘルパーなどがあれば使用されます。
   デフォルトでは、ビューは、レイアウトの内側に表示されます。
#. コントローラのその他のコールバック（:php:meth:`~Controller::afterFilter` など）があれば、
   実行されます。完全に描画されたビューコードは、リカルドさんのブラウザに送信されます。

.. meta::
    :title lang=ja: A Typical CakePHP Request
    :keywords lang=ja: optional element,model usage,controller class,custom cake,business logic,request example,request url,flow diagram,basic ingredients,datasources,sending emails,callback,cakes,manipulation,authentication,router,web server,parameters,cakephp,models
