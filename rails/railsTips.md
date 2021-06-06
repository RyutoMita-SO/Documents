# railsTips

## コマンド
参考：https://railsguides.jp/generators.html  
参考：https://railsdoc.com/page/rails_scaffold  

###  rails generate scafold...
rails generateというジェネレータ機能のうちのコマンドの一つ。  
アプリケーションの基本的な機能の一覧(index)、詳細(show)、新規作成(new/create)、編集(edit/update)、削除(destroy)するために必要なコントローラ、モデル、ビューをまとめて生成してくれるめっちゃ便利なコマンド。  
`rails generate scaffold page name:string title:string`  
と打てば、ストリング型のnameカラムとストリング型のtitleカラムを持ったpageというモデルとそれを操作するためのコントローラ、描写するビューが作られる。えぐい。  
