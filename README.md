# テーマ授業【Gitを用いたチーム開発】

## 開発をスタートするまでの準備

#### cloneする

cloneとは現時点でGithubのリポジトリに上がっているファイルやディレクトリを
そっくりそのままローカルに「コピー」することを指します。

`$ git clone https://github.com/DiveintoCode-corp/DIC_thema_lesson.git`

とすると、クローンできます。
なお、クローンすると自動的にリモートリポジトリが追加されるので

`$ git remote add origin ○○○○○`

は必要ありません。
さらにクローンは「ブランチ毎」にすることができます。


#### .gitignoreにvendor/bundleを追加

.gitignoreはバージョン管理対象外のものを指定するファイルです。


#### `bundle install --path vendor/bundle`

一般的にチーム開発においてGemをインストールするコマンドは「vendor/bundle」とつけます。
これはチーム内でGemのバージョンを合わせるために、
コンピュータの裏側ではなくあえてアプリケーションの中にGemをインストールします。


#### `sudo service postgresql start`（cloud9のみ）

postgresqlを起動させます。


#### ローカル環境にデータベースを作成

`$ rake db:create`


#### ローカル環境にテーブルを作成

`$ rake db:migrate`


#### Githubのアカウント情報を聞いてCollaboratorに追加

これをしないとリモートリポジトリにプッシュすることができません。



## 開発スタートしてからの流れ

#### 現在のブランチを確認

これは特に重要なことです。
issueごとのブランチは必ず「develop」ブランチから派生します。
ここで絶対やってはいけないのは、issue#○○○というブランチからissue#△△△というブランチを派生させることです。

`$ git branch`

で必ずdevelopブランチにいることを確認しましょう。


#### issueごとにブランチを切る

`$ git checkout -b issue#○○`


#### 開発

もし自分が開発途中で誰かがリモートのdevelopブランチにpushした時は、

`$ git pull origin develop`

をしてローカルのdevelopブランチを最新にしなければなりません。

もし途中で誰かがマイグレーションファイルをプッシュしたらpullした後に

`$ rake db:migrate`

を実行しなければなりません。

#### コミット

`$ git add .``
`$ git commit -m '（コミット内容）'`


#### push

開発していたブランチ名がissue#○○○であれば、
送り先のリモートブランチの名前もissue#○○○にすべきです。

`$ git push origin issue#○○○`

と打ってpushしましょう。


#### プルリク作成

いきなりdevelopブランチに直接pushする方法は、チーム開発だとほぼ0％でしょう。
開発したら誰かにレビューをもらうのが原則です。
送り先のリモートブランチの名前もissue#○○○にしたのもそのような理由だからです。
そこで「プルリクエスト」略してプルリクを作成して誰かにレビューを受けます。


#### マージ

プルリクを作成してレビューでOKをもらったらdevelopブランチへマージします。

#### ブランチ削除

issueがマージまで完了したらそのブランチは不要となるのでローカル・リモートともに
ブランチを削除します。
リモートのブランチはマージした時に「delete」のボタンが出現します。
ローカルのブランチは

`$ git branch -d issue#○○○`

で消すことができます。

#### developブランチに切り替えてpull

次のissueに取り掛かる前にローカルのdevelopブランチを最新にしておきます。
最新ではない状態からブランチを切ってしまうとコンフリクトのリスクが高まります。

`$ git checkout develop`
`$ git pull origin develop`

を実行しましょう。
