### メモ

OkHttpのエラーが出る場合、異なるアプリケーションを起動していることがあるのでそちらを切る。



#### ViewModelのインスタンスの生成

https://qiita.com/sudo5in5k/items/1d70ec65fd264eed5fc



#### リサイクラービューでローカルに保存されている画像の使用について

https://akira-watson.com/android/recyclerview.html



#### MVVCモデルの設計

今回やりたいことはこんな感じ?

https://qiita.com/Tsutou/items/69a28ebbd69b69e51703













勉強メモ(6/6)

やりたいこと
「REST APIを叩いて取得した結果をもとにUIを更新する。」

- HTTP通信はメインスレッド以外
- jsonのパース
- UIの更新はメインスレッド(async awaitなど?)