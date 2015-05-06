[[目次](README.md)]
# 実用的なアプリケーション開発 4 ビジネスロジック

## ビジネスロジックの記述

フォーム値の検証が完了したら、いよいよロジック部分(アプリケーションの本質的な仕事をする部分)を記述します。

注意点としては、アクションクラス(perform()メソッド)にはアプリケーションの核となる処理をベタベタと記述してはいけません。

基本的にはほぼ全ての処理はアプリケーションの核となるクラス(app/manager/, app/model/以下に置かれるクラス)に記述し、アクションクラスはそれらを単純に呼び出すのみ、というイメージです。

例えば

app/action/Login/Do.php:

```php
public function perform()
{
    // メールアドレスをキーにしてユーザオブジェクトを生成
    $user = new User($this->backend, $this->af->get('mailaddress'));
    // 認証処理
    $result = $user->auth($this->af->get('password');
    // 以降結果によってビューを変更、等...
}
```

のようにするか、またはMangerを使って

```php
public function perform()
{
    $um = $this->backend->getManager('user');

    // 認証処理
    $result = $um->auth($this->af->get('mailaddress'), $this->af->get('password'));
    // 以降結果によってビューを変更、等...
}
```

というようにするのがよいでしょう。

なぜこのようにするのかというと、

* 各アクション/ビュークラス間でのコードの重複を防ぐ
* ビジネスロジックの再利用性を高める
* 単体テストを書きやすくする

などが目的です。