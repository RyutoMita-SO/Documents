# Laravel学習のためのTodoアプリ開発ログ
題材：[入門Laravelチュートリアル ](https://www.hypertextcandy.com/laravel-tutorial-introduction)

## 2020/07/23

まず環境を構築にはWinだとXAMPPを使ってLaravelを作っていく。  
その前にGit上でブランチを切らなくては。  
features/environment_laravelで環境構築用のブランチを作成。  
その後下記サイトをもとに諸々インストール。  
参考サイト：[WindowsでXAMPPを使えばLaravel環境構築は簡単](https://reffect.co.jp/laravel/windows-xampp-laravel-install)  
XAMPPとComposerとインストールが無事に終わり、ブランチ上で`composer create-project --prefer-dist laravel/laravel laravel5.8`を実行。  
laravel5.8フォルダに移動して、php artisanコマンドで開発サーバを起動に成功。  
ようやく開発に入れるかと思ったが問題発生artisanファイルが消失
