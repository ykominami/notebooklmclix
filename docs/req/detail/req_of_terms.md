# 用語の定義

- **ユーザ**: `setup` サブコマンドで指定された名前（`user`）で内部的に識別されるアカウント。
- **プロジェクト名**: `notebooklmclix`
- **コンフィグディレクトリ**:
  - Windows: `%APPDATA%/<プロジェクト名>/`
  - Unix 系: `~/.config/<プロジェクト名>/`
- **コンフィグファイル**: `config.yml`
  （キー: `user`）
- **ユーザディレクトリ**:
  - Windows: `%LOCALAPPDATA%/<プロジェクト名>/<user>/`
  - Unix 系: `~/.local/share/<プロジェクト名>/<user>/`
- **NLM_PYコマンド**: `uv tool run --from notebooklm-py notebooklm`
- **Workspace**: NotebookLM からNotebook、Sourceを取得して格納する場所。<個別Workspace>を用意する。
- **個別Workspace**: リモートに存在するNotebookLMの状態（Notebook、Sourceなどの変更）に対応する<Workspace>。ファイルシステム上の1 つのディレクトリ（<個別Workspaceディレクトリ>）に紐づけられる。特定の状態のNotebookLMから、すべてのNotebook、Sourceを取得して格納する。
- **Notebookインデックス**: <Notebook生成一覧ファイル>内で、<notebook_id>に一対一に割り当てられた、1 から始まる整数。
- **Notebook生成一覧ファイル**: `notebook_creation_list.tsv`
  - 生成元: <Notebook取得実行結果記録ファイル>内の<RESULT>が`OK`の<notebook_id>で、<Notebook生成一覧ファイル>に未登録のもの。
  - フォーマット: `<notebook_id>\t<Notebookインデックス>\t<WorkspaceID>`
- **Notebook状態**:
  `M`: 変更
  `D`: 削除
- **Source状態**:
  `M`: 変更
  `D`: 削除
- **Notebook存在比較元ファイル**: <Notebook存在比較先ファイル>の属する<個別Workspace>の1 つ前の<個別Workspace>に含まれる<Notebook一覧ファイル>。
- **Notebook存在比較先ファイル**: 当該の<個別Workspace>に含まれる<Notebook一覧一時ファイル>。
- **Notebook変更一覧ファイル**: `notebook_change_list.tsv`
  - 生成元: <Notebook取得実行結果記録ファイル>内の<RESULT>が`OK`の<notebook_id>で、<Notebook生成一覧ファイル>に登録済のものは、<Notebook状態>を`M`とする。<Notebook存在比較元ファイル>に含まれるが、<Notebook存在比較先ファイル>に含まれない<notebook_id>は、<Notebook状態>を`D`とする。
  - フォーマット: `<Notebookインデックス>\t<Notebook状態>\t<WorkspaceID>`
- **Sourceインデックス**: <Source生成一覧ファイル>内で、<source_id>に一対一に割り当てられた、1 から始まる整数。
- **Source生成一覧ファイル**: `source_creation_list.tsv`
  - 生成元: <Source取得実行結果記録ファイル>内の<RESULT>が`OK`の<source_id>で、<Source生成一覧ファイル>に未登録のもの。
  - フォーマット: `<source_id>\t<Sourceインデックス>\t<WorkspaceID>`
- **Source存在比較元ファイル**: <Source存在比較先ファイル>の属する<個別Workspace>の1 つ前の<個別Workspace>に含まれる<Source一覧ファイル>。
- **Source存在比較先ファイル**: 当該の<個別Workspace>に含まれる<Source一覧一時ファイル>。
- **Source変更一覧ファイル**: `source_change_list.tsv`
  - 生成元: <Source取得実行結果記録ファイル>内の<RESULT>が`OK`の<source_id>で、<Source生成一覧ファイル>に登録済のものは、<Source状態>を`M`とする。<Source存在比較元ファイル>に含まれるが、<Source存在比較先ファイル>に含まれない<source_id>は、<Source状態>を`D`とする。
  - フォーマット: `<Sourceインデックス>\t<Source状態>\t<WorkspaceID>`

- **取得計画トップディレクトリ**: `plans/`
- **個別取得計画ディレクトリ**: <PlanID>
  - **PlanID**: <個別Workspaceディレクトリ>毎の取得計画を作成する回数であり、1 から始まる整数。
- **Notebook取得候補ファイル**: `notebook_candidates.yml`
  - 生成元: <Notebook存在比較元ファイル>の`notebooks`フィールドには存在しないが、<Notebook存在比較先ファイル>の`notebooks`フィールドには存在する要素の`id`。また、どちらにも存在するが、`modified_at`フィールドの値が異なる要素の`id`。
  - フォーマット:
    `notebooks:
      - <notebook_id>`
- **Notebook取得候補更新済ファイル**: `notebook_candidates_updated.yml`
  - 生成元: <Notebook取得候補ファイル>から、自身の属する<個別Workspaceディレクトリ>に含まれる、既存の<Notebook取得実行結果記録ファイル>で<RESULT>が`OK`だった<notebook_id>を削除したもの。
  - フォーマット:
    `notebooks:
      - <notebook_id>`
- **Notebook取得計画ファイル**: `notebook_plan.yml`
  - 生成元: <Notebook取得候補更新済ファイル>（YAML 形式）。そのままコピーする。
  - フォーマット:
    `notebooks:
      - <notebook_id>`
- **Source取得候補ファイル**: `source_candidates.yml`
  - 生成元: <Source存在比較元ファイル>の`sources`フィールドには存在しないが、<Source存在比較先ファイル>の`sources`フィールドには存在する要素の`id`。また、どちらにも存在するが、`modified_at`フィールドの値が異なる要素の`id`。
  - フォーマット:
    `sources:
      - <source_id>`
- **Source取得候補更新済ファイル**: `source_candidates_updated.yml`
  - 生成元: <Source取得候補ファイル>から、自身の属する<個別Workspaceディレクトリ>に含まれる、既存の<Source取得実行結果記録ファイル>で<RESULT>が`OK`だった<source_id>を削除したもの。
  - フォーマット:
    `sources:
      - <source_id>`
- **Source取得計画ファイル**: `source_plan.yml`
  - 生成元: <Source取得候補更新済ファイル>（YAML 形式）。そのままコピーする。
  - フォーマット:
    `sources:
      - <source_id>`
- **個別Workspaceディレクトリの作成日時**: <個別Workspaceディレクトリ>の作成日時を<JSTタイムスタンプ形式>で表現したもの。
- **Workspace作成記録ファイル**: `workspaces.yml`
  - フォーマット: `<WorkspaceID>: <個別Workspaceディレクトリの作成日時>`
- **Workspaceトップディレクトリ**: `workspaces/`
- **個別Workspaceディレクトリ**: <WorkspaceID>
  - **WorkspaceID**: <Workspace>を作成する回数であり、1 から始まる整数。
- **Notebookトップディレクトリ**: `notebooks/`
- **JSTタイムスタンプ形式**: `YYYY-MM-DD HH:MM:SS`
- **notebook_id**: <Notebook一覧ファイル>の生成元のフィールド`id`の値。
- **Notebook一覧ファイル**: `notebook_list.yml`
  - 生成元: `<NLM_PYコマンド> list --json --no-truncate` の JSON 形式の出力
  - 例:
`{
  "notebooks": [
    {
      "index": 1,
      "id": "90f8b892-0b21-4813-8a58-77a06bc611ce",
      "title": "AI-AIによる開発-エージェント管理",
      "is_owner": true,
      "role": "owner",
      "created_at": "2026-04-23T07:31:31+00:00",
      "last_viewed_at": "2026-09-04T22:10:29+00:00",
      "modified_at": "2026-09-04T22:10:29+00:00"
    }
  ],
  "count": 1
}`
- **Notebook一覧一時ファイル**: `notebook_list_temp.yml`
  - 生成元: `<NLM_PYコマンド> list --json --no-truncate` の JSON 形式の出力
  - 例: <Notebook一覧ファイル>と同一。
- **RESULT**: `OK` / `NG`
- **Notebook取得実行結果記録ファイル**: `notebook_progress.yml`
  - 生成元: 同一の<notebook_id>に対応する<Notebook要約ファイル>と<Notebookメタデータファイル>のどちらも生成できた場合に `OK`、そうでない場合に `NG` とする。
  - 制約: トップレベルキーの<Notebookインデックス>は<Notebook生成一覧ファイル>で定義された、<notebook_id>に対応する値と一致すること
  - フォーマット:
    - <Notebookインデックス>:
      - title: <Notebookのタイトル>
      - timestamp: <Notebookの取得結果を記録する日時（<JSTタイムスタンプ形式>）>
      - result: <RESULT>
- **個別Notebookディレクトリ**: <Notebookインデックス>
- **Sourceインデックス配列**: Notebookに含まれる<Sourceインデックス>の配列。空白文字で区切って並べたもの。
- **Notebookのタイトル**: <Notebook一覧ファイル>の生成元の`title`フィールドの値。
- **Notebookリレーションファイル**: `notebook_relation.yml`
  - 制約:
    - 各エントリの<Notebookインデックス>と<Sourceインデックス配列>が、<Notebook一覧ファイル>・<Source一覧ファイル>・各<Notebookメタデータファイル>から導かれる対応関係と一致すること
  - フォーマット:
    - notebook_index: <Notebookインデックス>
    - array_of_source_index: <Sourceインデックス配列>
- **Notebookメタデータファイル**: `notebook_metadata.yml`
  - 生成元: `<NLM_PYコマンド> metadata --notebook <notebook_id> --json` の JSON 形式の出力
  - 例:
`{
  "id": "90f8b892-0b21-4813-8a58-77a06bc611ce",
  "title": "AI-AIによる開発-エージェント管理",
  "created_at": "2026-04-23T07:31:31+00:00",
  "last_viewed_at": "2026-09-05T21:55:57+00:00",
  "modified_at": "2026-09-05T21:55:57+00:00",
  "is_owner": true,
  "role": "owner",
  "sources": [
    {
      "type": "youtube",
      "title": "Driving agents from Basecamp (experimental connector)",
      "url": "https://www.youtube.com/watch?v=jq-Z1Y631f8"
    }
  ]
}`

- **Notebook要約**: <Notebook要約ファイル>の生成元の`summary`フィールドの値
- **Notebook要約ファイル**: `notebook_summary.yml`
  - 生成元: `<NLM_PYコマンド> notebook summary --notebook <notebook_id> --json` の JSON 形式の出力から `summary` の値を抽出し、<notebook_id>に対応する<Notebookインデックス>と組み合わせる。`<NLM_PYコマンド> notebook summary --notebook <notebook_id> --json` コマンドの実行に失敗した場合、または `summary` を抽出できない場合は、<Notebook要約ファイル>を生成しない。
  - フォーマット:
    - notebook_index: <Notebookインデックス>
    - summary: <Notebook要約>
- **Sourceタイプ**: Sourceのタイプを表す次のいずれかの値

┌────────────────┬────────────────────────┐
│       値       │          意味          │
├────────────────┼────────────────────────┤
│ youtube        │ YouTubeリンク          │
├────────────────┼────────────────────────┤
│ pdf            │ PDFファイル            │
├────────────────┼────────────────────────┤
│ markdown       │ Markdownファイル          │
├────────────────┼────────────────────────┤
│ web_page       │ Webページ              │
├────────────────┼────────────────────────┤
│ google_docs    │ Google Docs            │
├────────────────┼────────────────────────┤
│ pasted_text    │ 貼り付けテキスト       │
├────────────────┼────────────────────────┤
│ generated_text │ NotebookLM生成テキスト │
├────────────────┼────────────────────────┤
│ Unknown        │ 不明                   │
└────────────────┴────────────────────────┘
- **URL値**:
  URLを用いてSourceを指定した場合（`type` が `youtube` / `web_page` / `pdf` / `google_docs` の場合）はURLを表す文字列。該当しない場合は、空文字列。
- **Sourceトップディレクトリ**: `sources/`
- **Sourceのタイトル**: <Source一覧ファイル>の生成元に含まれる`title`フィールドの値。
- **source_id**: <Source一覧ファイル>の生成元のフィールド`id`の値。
- **Source一覧ファイル**: `source_list.yml`
  - 生成元: 処理対象となった各Notebookについて `<NLM_PYコマンド> source list --notebook <notebook_id> --json` の JSON 形式の出力（オブジェクトの配列）から、各要素の `id`, `title`, `type`, `url` を取得し、`id` の値ごとに採番した<Sourceインデックス>と組み合わせる（`source_id` として用いる `id` が同一の場合は<Sourceインデックス>を再利用し、重複したエントリを作らない）。`url` が得られない場合は空文字列とする。
  - フォーマット:
    - <Sourceインデックス>:
      - source_id: <source_id>
      - title: <Sourceのタイトル>
      - type: <Sourceタイプ>
      - url: <URL値>
- **Source一覧一時ファイル**: `source_list_temp.yml`
  - 生成元: <Source一覧ファイル>と同じ。
  - 例: <Source一覧ファイル>と同一。
- **Sourceの取得日時-JST**: <Sourceフルテキストファイル>の生成日時と<Source内容ファイル>の生成日時のうち、新しい方の日時を<JSTタイムスタンプ形式>で表現したもの。
- **Source取得実行結果記録ファイル**: `source_progress.yml`
  - 生成元:
    - 同一の<source_id>に対応する<Sourceフルテキストファイル>と<Source内容ファイル>のどちらも生成できた場合に `OK`、そうでない場合に `NG` とする。
  - 制約:
    - <Sourceインデックス>は<Source一覧ファイル>で定義された、<source_id>に対応する値と一致すること
    - <Sourceのタイトル>は、<Sourceインデックス>に対応する、<Source一覧ファイル>の<Sourceのタイトル>と一致すること
  - フォーマット:
    - <Sourceインデックス>:
      - title: <Sourceのタイトル>
      - timestamp: <Sourceの取得日時-JST>
      - result: <RESULT>
- **個別Sourceディレクトリ**: <Sourceインデックス>
- **Sourceフルテキストファイル**: `source_fulltext.yml`
  `<NLM_PYコマンド> source fulltext <source_id> --notebook <notebook_id> --json` で取得した JSON 形式の出力
  - 例:
`{
  "source_id": "6c49a0f7-98c9-4a5a-9639-1d87bf9b58bb",
  "title": "【会見ノーカット】閣議後  金子国交相 記者会見「辺野古沖転覆事故  死亡した船長を刑事告発へ」 ──政治ニュース（日テレNEWS）",
  "kind": "youtube",
  "content": "はい  え  おはよう ござい ます  あの  本日 の 核撃 案件 で 特に 私 から 報告 する もの は ござい ませ ん  ",
  "url": "https://www.youtube.com/watch?v=JDITTcmSHR4",
  "char_count": 5408
}`

- **Source内容ファイル**: `source_content.yml`
  `<NLM_PYコマンド> source get <source_id> --notebook <notebook_id> --json` で取得した JSON 形式の出力
  - 例:
`{
  "source": {
    "id": "6c49a0f7-98c9-4a5a-9639-1d87bf9b58bb",
    "title": "【会見ノーカット】閣議後  金子国交相 記者会見「辺野古沖転覆事故  死亡した船長を刑事告発へ」 ──政治ニュース（日テレNEWS）",
    "type": "youtube",
    "url": "https://www.youtube.com/watch?v=JDITTcmSHR4",
    "status": "ready",
    "status_id": 2,
    "created_at": "2026-05-23T03:32:58+00:00",
    "drive_document_id": null,
    "drive_status": null,
    "is_drive_degraded": false
  },
  "found": true
}`
