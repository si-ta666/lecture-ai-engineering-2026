# 第5回 解答

## 演習

- `--list-tools` で見えたツール名と、第3回（`--mcp` なし）との違い：第3回では search_glossary と get_lecture_summary の2個が表示された。第5回では glossary_search_glossary の1個が表示された。MCP経由ではツール名にサーバー名のglossary_が付いており、get_lecture_summaryはまだ公開されていなかった。
- `--mcp --trace` で見えたツール呼び出し（ツール名と引数）と、`requests` の回数： MCPありでは `glossary_search_glossary` が呼び出され、引数は `{'query': '3レベル'}` だった。ツール呼び出しは1回、requestsは2回だった。MCPなしでは `search_glossary` が呼び出され、引数と回数は同じだった。回答内容にも大きな違いはなかった。
- MCP サーバーに公開したツールと、その説明文：tools.search_glossary と tools.get_lecture_summary の2つを公開した。get_lecture_summary の説明文は、docstringに記載された内容がツールの説明として表示された。また、docstringを変更してHTTPサーバーを再起動すると、変更後の説明文が --list-tools に反映された。
- fetch サーバーを足したときのツール呼び出し（ツール名と引数）：fetch_fetch が呼び出され、引数は {'url': 'https://modelcontextprotocol.io/'} だった。また、用語の確認のために glossary_search_glossary({'query': 'MCP'}) も呼び出された。ツール呼び出しは合計2回、requestsは3回だった。取得したWebページの内容を日本語で3行に要約できた。

## 宿題

- 取り組んだレベル：基礎
- やったこと：MCPサーバーの通信方式をstdioからStreamable HTTPに変更し、別のターミナルでHTTPサーバーを起動してエージェントから接続した。また、get_lecture_summary のdocstringを変更し、--list-tools と --trace でツールの説明文や呼び出し結果を確認した。
- 結果：Streamable HTTP方式でも glossary_get_lecture_summary が正常に呼び出され、第2回の講義内容を取得できた。docstringの変更後、サーバーを再起動すると、変更した説明文がツール一覧に反映された。また、uv run pytest -q を実行し、11件すべてのテストが成功した。
- 気づき・疑問：MCPにはstdioとStreamable HTTPの2つの通信方式があり、stdioではエージェント側からコマンドを実行してサーバーを起動する一方、Streamable HTTPではサーバーを独立して起動し、URLを指定して接続することを学んだ。また、ツールのdocstringや型ヒントがモデルに渡される説明や引数の情報になるため、モデルが適切なツールを選択できるように、分かりやすく記述することが重要だと感じた。
