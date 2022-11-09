#herokuでのエラー
マイグレーションファイルを誤って2つ作成しており、重複のエラーが出ていた。rollbackしてファイル削除後にマイグレーションを行えば良い。
今回誤ってファイルを削除してからロールバックしようとしてしまった。解決策として、もう一度IDの同じファイルを作成してから行う。

https://qiita.com/KKDDD/items/b1158caebe3788eb39c8
https://qiita.com/ITmanbow/items/4e331fde26b6809b4cc7
