---
title: トレースの送信
type: apicontent
order: 34.1
external_redirect: '/api/#send-traces'
---
## トレースの送信

Datadog の APM を使用すると、コードをトレースしてパフォーマンスメトリクスを収集し、アプリケーションのどの部分が実行に時間がかかるか、あるいは非効率であるかを特定できます。

トレーシングデータは、HTTP API から Datadog Agent に送信されます。Datadog は、Datadog Agent へのメトリクスの送信を簡略化するために、いくつかの[公式ライブラリ][1]を提供しています。ただし、それらのライブラリを使用できないアプリケーションや、公式の Datadog トレーシングライブラリがまだ提供されていない言語で書かれたアプリケーションを計測するために、API を直接使用することもできます。

トレースは、次のような[トレース][2]の配列として送信されます。

```text
[ trace1, trace2, trace3 ]
```

それぞれのトレースは、次のような[スパン][3]の配列です。

```text
trace1 = [ span, span2, span3 ]
```

さらに、それぞれのスパンは、`trace_id`、`span_id`、`resource` などを含む辞書です。

[APM と分散型トレーシングの用語については、こちらを参照してください][4]。

**注**: 同じトレース内の各スパンは同じ `trace_id` を使用する必要があります。しかし、`trace_id` と `span_id` は異なる値でなければなりません。

**引数**:

*   **`trace_id`** - 必須 - このスパンが含まれるトレースの一意の整数 (64 ビット符号なし) ID。
*   **`span_id`** - 必須 - スパンの整数 (64 ビット符号なし) ID。
*   **`name`** - 必須 - スパン名。スパン名の長さは、最大 100 文字です。
*   **`resource`** - 必須 - トレース対象のリソース。リソース名の長さは、最大 5000 文字です。
*   **`service`** - 必須 - トレース対象のサービス。サービス名の長さは、最大 100 文字です。
*   **`type`** - オプション、デフォルト = **custom**、大文字と小文字の区別あり - リクエストのタイプ。設定可能なオプションは、`web`、`db`、`cache`、`custom` です。
*   **`start`** - 必須 - リクエストの開始時間を Unix Epoch からのナノ秒で指定します。
*   **`duration`** - 必須 - リクエストの処理時間 (ナノ秒単位)。
*   **`parent_id`** - オプション - 親スパンのスパン整数 ID。
*   **`error`** - オプション - エラーが発生したことを示すには、この値を 1 に設定します。エラーが発生した場合は、エラーメッセージ、タイプ、スタックなどの追加情報を `meta` プロパティで渡す必要があります。
*   **`meta`** - オプション - キー/値メタデータのセット。キーと値は文字列でなければなりません。
*   **`metrics`** - オプション - キー/値メタデータのセット。キーは文字列、値は 64 ビット浮動小数点数でなければなりません。

[1]: /ja/tracing/#instrument-your-application
[2]: /ja/tracing/visualization/trace
[3]: /ja/tracing/visualization/trace/#spans
[4]: /ja/tracing/visualization/services_list