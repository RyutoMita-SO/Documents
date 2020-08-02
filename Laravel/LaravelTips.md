# LaravelTips

[戻る](https://github.com/RyutoMita-SO/Documents)  

### 外部キー制約を付けるとき  
参考：[【Laravel】外部キー制約があるテーブルのmigrateができません](https://teratail.com/questions/187718)  
参考：[【Laravel】 5.8系にアップデートしてマイグレーションファイルを自動生成する時に注意する事](https://blog.websandbag.com/entry/2019/06/09/111933)  
参考：[【メモ】【Laravel】外部キー制約付きMigrateがさっぱり動かないときのチェック・ポイント（Mysql）](https://qiita.com/0w0/items/4a9cb7d27794bfb93d46)  
参考：[入門Laravelチュートリアル (4) ToDoアプリのタスク一覧表示機能を作る](https://www.hypertextcandy.com/laravel-tutorial-todo-app-list-tasks)  　　

例：foldersというテーブルの`iD`とTasksというテーブルの`floder_id`を紐づけたいとき  　　
マイグレーションファイルには以下のように記述する  
```
public function up()
    {
        Schema::create('tasks', function (Blueprint $table) {
            //ポイント1
            $table->integer('folder_id')->unsigned();
            
            //省略

            //ポイント2
            $table->foreign('folder_id')->references('id')->on('folders');
        });
    }
```
1. 「->unsigned();」で符号なし属性を紐づける。
foldersテーブル内の`iD`はincrementで自動作成されており、裏でunsined（符号無し）属性が付与されている。（採番項目なので正の値しか登録できない）  
そのため、「->unsigned();」で符号なし属性を付与しないと形式の不一致が起きてエラーが起きてしまう。  

1. `folder_id`を宣言後、foreign('folder_id')->references('id')->on('folders');で外部制約キーを設定する。  
符号なし属性を紐づけたあと、  
`foreign('このテーブルで設定したカラム')->references('外部制約で紐づけたいカラム')->on('外部制約で紐づけたいカラムを保持するテーブル');`  
を宣言する。  


