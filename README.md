

https://user-images.githubusercontent.com/40127279/125183017-dba03180-e24d-11eb-80c6-f29cb6cb19b4.mp4

## 　　Lesson8.　RSSニュースを検索する  
#### 開発メモ
### 1.％エンコードしてURLを生成する
　早速、Googleニュース検索のRSSを手打ちしてみたら日本語の検索がうまくないようです
<br>　試しにUTF-8で％エンコードしたら問題なく取得できました
<br>　価格.comキーワード検索で実装したnkfを利用しました
<br>　あとはURLの文字列に結合するだけです
### 2.RSSを取得する
　Lesson7と同様にcURLでRSSを取得します
<br>　今回は、一旦RSSを変数で受けるようにしてみました
<br>　特に問題なく、後続のタグ抽出ができました
<br>　Lesson7はなにが悪かったのかな、まあいいか
### 3.RSSからタグの内容を取得する
　こちらもLesson7と同様にsedとgrepでタグの中身をとりだしています
<br>　違っている点は、取り出し元は変数になっていることです
<br>　実装したらdescriptionがうまくとりだせません
<br>　色々いじったのですがわかりませんでした
<br>　結局、タイトルが、『記事のタイトル ー ニュース発信元』という構成だったので
<br>　ハイフンの前後で分割してAlfredのタイトルとサブタイトルにセットしました
<br>　下記の部分で取り出したtitleタグの中身を『-』で分割しています
```
　arry=(${title[i]//-/ })
```
### 4.JSON出力フォーマットのXMLを作成する
　こちらもLesson7と同様です
<br>　上記の分離を受けてtitleに記事のタイトルをsubtitleにニュース発信元をセットしています
#### 背景
 Lesson7のRSSツールを改造してRSSを検索できるようにしました
<br> googleニュースが検索用のRSSを発見して作ってみました
#### 取扱説明
### 機能:
　Alfred Json Formatをシェルスクリプトで動かしてみる
<br>　※本家Alfredのサンプルの簡易版です
### インストール:
　1.[alfredworkflow](https://github.com/KitanoTamotsu/searchnews/releases/download/1.3/news.alfredworkflow.zip)をダウンロード 
<br>　2.ファイルをダブルクリックしてワークフローに登録
### 使い方:
　Alfredから『ニュース』+検索ワード
#### 修正履歴
### ver1.1(2021-03-28)：
・ScriptFilterのJSONフォーマットの編集をワンライナーから1行1プロパティーに変更
<br>　修正前
```
　json=$json'{"title":"'${arry[0]::24}'","subtitle":"'${arry[1]}' - '${pdate[i]:9:4}.${pdate[i]:6:3}.${pdate[i]:4:2}'","arg":"'${link[i]}'"}'
``` 
<br>　修正後
```
  json=$json'{"title":"'${arry[0]::24}'",'
  json=$json'"subtitle":"'${arry[1]}' - '${pdate[i]:9:4}.${pdate[i]:6:3}.${pdate[i]:4:2}'",'
  json=$json'"arg":"'${link[i]}'"}'
```
### ver1.2(2021-04-04)：
   シェルスクリプトをbashからzshに変更

### ver1.3(2021-05-16)：
   zshで変数内置換後の配列格納の際に空白で分割されない不具合に対応
<br>  変更前
```
   arry=(${title[i]//-/ })
```
<br> 変更後
```
   arry=(`echo ${title[i]/-/ }`) 
```   
<br>　『//』を『/』に変更した部分は、初めの『-』だけで分割するようにしたものです
<br>　ニュース発信元の文字列に『-』があるものがあったので、合わせて修正しました
<br>    
<br>　ちなみにzshのスクリプトに以下のように、オプションを指定をする方法もあるそうです
```
setopt SH_WORD_SPLIT
```

<br>
<br>
[トップページに戻る](https://kitanotamotsu.github.io/)

