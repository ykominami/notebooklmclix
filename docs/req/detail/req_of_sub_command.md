# サブコマンド定義
## setup
### オプション
- `--user <username>`: ユーザ名。省略可。省略値は環境変数USERNAMEの値。
### 処理内容
- <コンフィグファイル>に user: <username> を出力する。

## login
### オプション
- なし
### 処理内容
- `<NLM_PYコマンド> login` を実行する。

## fetch
### オプション
- `--limit <N>`: 処理対象のSourceを、先頭 N 件（N は 1 以上の整数）に制限する。省略時は全件を対象とする。Notebookは、Sourceを指定件数取得するのに必要な最小限の件数を取得する。
### 処理内容
- 新規WorkspaceIDを採番する
- 必要最小限の件数分のNotebookを取得し、取得したNotebookに対応したSourceを制限の上限まで取得する。
- <Workspace作成記録ファイル>に記録する

## fix
### オプション
- なし
### 処理内容
- 現在のところ未定

## check
### オプション
- `--ws <WorkspaceID>`: 省略可。省略値は直近に生成したWorkspaceID
### 処理内容
- 指定された<WorkspaceID>に対応する<個別Workspaceディレクトリ>内の各<Notebook取得実行結果記録ファイル>で記録されたNotebookの取得結果（<Notebookのタイトル>, <Notebookの取得結果を記録する日時（<JSTタイムスタンプ形式>）>, <RESULT>）を出力する

