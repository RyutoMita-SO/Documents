# LaravelTips

[戻る](https://github.com/RyutoMita-SO/Documents)  

### オブジェクト指向について（Laravel）  
参考：[オブジェクト指向について](https://laraweb.net/surrounding/466/)  
* クラスとは?  
いわゆる**設計図**といわれるもの。  
設計図（=クラス）からインスタンス（オブジェクト）を作ることを**オブジェクト指向**と呼ぶ。  
書き方は以下。  
```
class クラス名 {
  実装したい処理
}
```

* プロパティとは?  
クラス内に定義された**変数**。  
public、private、protected (アクセス修飾子)のいずれかで定義する。  
public：どこからでもアクセスできる  
private：そのクラスと、サブクラスからしかアクセスできない  
protected：そのクラスからしかアクセスできない  

* メソッドとは?  
クラス内に定義された**関数**。  

* インスタンスとは?  
クラスの**実体**。  
```
$インスタンス名 = new クラス名
```
クラス内に定義されている変数や関数などに数値や文字などを渡すなどして実行結果を得るもの。  
（設計図だけでは結果は得られないため、結果を得るためのインスタンスを作る必要がある？）  

▼例  

```
class Math {
  //プロパティを定義
  public $number1;
  public $number2;
 
  //メソッドを定義
  function add() {
  return $this->number1 + $this->number2;
  }
  function minus() {
  return $this->number1 - $this->number2;
  }
}
```

```
//Mathクラスのオブジェクトを作成
$math = new Math;
 
//プロパティに値を代入
$math->number1 = 7;
$math->number2 = 5;
 
//メソッドを実行して結果を取得
$result1 = $math->add();
$result2 = $math->minus();
 
echo "足し算の結果は{$result1}、引き算の結果は{$result2}です。";
```

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
1. `->unsigned();`で符号なし属性を紐づける。
foldersテーブル内の`iD`はincrementで自動作成されており、裏でunsined（符号無し）属性が付与されている。（採番項目なので正の値しか登録できない）  
そのため、「->unsigned();」で符号なし属性を付与しないと形式の不一致が起きてエラーが起きてしまう。  

1. `folder_id`を宣言後、foreign('folder_id')->references('id')->on('folders');で外部制約キーを設定する。  
符号なし属性を紐づけたあと、  
`foreign('このテーブルで設定したカラム')->references('外部制約で紐づけたいカラム')->on('外部制約で紐づけたいカラムを保持するテーブル');`  
を宣言する。  

### Carbonとは？  
参考：[【Laravel】Carbonを初めて使ってみた](https://qiita.com/mackeyTA/items/e8b5e47a9f020a1902c0)  
参考：[全217件！Carbonで時間操作する実例](https://blog.capilano-fw.com/?p=867#createFromFormat)  
PHPに標準実装されているDateTimeクラスを継承した日時を扱うクラス。
Laravelでも実装されている。  
日付は表示形式が色々と違うため、これを使うと便利。  
使うには、使うファイル内で`use Carbon\Carbon;`と宣言。  
  
日時フォーマットからインスタンスを作成する：`Carbon::createFromFormat('フォーマット（Y-m-dなど）','データ（2020-08-02など）');`  
※あくまでインスタンスを作成するので、データとフォーマットの型は一致していないとエラーが起こる  
指定したフォーマットで出力する：`->format('フォーマット');`  
▼例
```
return Carbon::createFromFormat('Y-m-d', $this->attributes['due_date'])->format('Y/m/d');
```
createFromFormatでインスタンスを作成し、Y/m/dというフォーマットで出力してそれをreturnで返している。  

### Tinkerとは？  
参考：[Laravelのtinkerを使えるとちょっと幸せになれる](https://qiita.com/juve_534/items/96dc6e7e0652dced1428)  
artisanコマンドが使えるディレクトリ配下で`php artisan tinker`を実行することで、  
Laravelを対話的に動かすことができるコマンド。  
動作確認の際に便利。  

### find()、get()、first()、where()、all()の返り値早見表  
参考：[【Laravel】DB登録値取得時のfind()、get()、first()の返り値早見表](https://qiita.com/sola-msr/items/fac931c72e1c46ae5f0f)  
* find()  
`App\Model::find('id') `の返り値はModelのオブジェクト。  
idと一致したデータを見つけて返してくれる。  

* get()  
`App\Model::where('カラム名', カラム内の値)->get()`の返り値はCollectionクラス  
（中身はModelのオブジェクト。ゆえにforeach()で回せば各々の値を取得できる。） 
whereで一致した値を全て取得している。  
※whereは下部参照  

* first()  
`App\Model::where('カラム名', カラム内の値)->first()`の返り値はModelのオブジェクト  
上記のwhere条件に一致した一番最初のデータを取得してくる。  

* where()   
参考：[WHERE句について](https://www.ritolab.com/entry/93#aj_11)
`App\Model::where('カラム名', カラム内の値)`の返り値はBuilderクラス  
カラム名とカラム内の値に一致したデータのインスタンスを作成されている（SQLを作っただけ？）。  
つまりwhereだけだと値を取得しているわけではないので  
QueryBuilderのgetメソッドなどを使用して値を取得しないと結果の入ったインスタンスが作られる。

* all()  
App\Model::all() の返り値はCollectionクラス  
全てのデータを取得してくる。  
