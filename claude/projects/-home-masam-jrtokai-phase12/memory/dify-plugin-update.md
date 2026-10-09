---
name: dify-plugin-update
description: Dify のプラグイン更新はローカル pkg だと uninstall→install が必須(upgrade API は marketplace/github のみ)
metadata:
  type: reference
---

Dify CE 1.16 系には、ローカル `.difypkg` から既存プラグインを更新する API が無い。`upgrade` は `/workspaces/current/plugin/upgrade/marketplace` と `.../upgrade/github` の 2 つだけ。

- `install/pkg` は**新規インストール用**。同じ `plugin_id` が既にあると、タスクは `success('installed')` を返し実体も `/app/storage/cwd/<author>/<plugin>-<ver>@<hash>/` に展開されるが、**有効版(`plugin/list` の version)は差し替わらない**。
- DSL の `dependencies` に新しい識別子を書いてインポートしても、**取り込み時にインストール済みの版へ解決し直されて戻る**。DSL だけでは版を上げられない。
- **したがって uninstall → install。** 実績(2026-09-22・jrtokai_kb 0.3.5→0.3.6): uninstall は `{"success": true}`、install 後 3 秒で有効版が変わり、**資格情報(`kg_api`)は消えず再登録不要**(`add` は「名前が既に使われている」で拒否される=残っている証拠)、**DSL も自動で新しい識別子へ解決された**(再インポート不要)。停止は数秒。
- **旧版の `.difypkg` を先に作ってから uninstall する**。リポジトリの旧コミットから `git archive <sha> deploy/dify/plugin/jrtokai_kb | tar x` → `./dify-plugin plugin package ./<dir> -o <out>.difypkg`。`dify-plugin` CLI は EC2 に常設されていないので、GitHub release(`langgenius/dify-plugin-daemon` 0.6.10 の `dify-plugin-linux-amd64`)から取る。
- console API の認証は `~/dify/ADMIN_CREDENTIALS.txt`(URL / Email / Password)。**login はパスワードを Base64 で送り、トークンは Cookie(`access_token` / `csrf_token`)で返る**。`http.cookiejar` が要る。`plugin/tasks/<id>` は 500 を返すことがあるので、`plugin/tasks?page=1&page_size=5` の一覧か `plugin/list` の version で確認する。
- チャットフローのスモークは**公開 API**(`/v1/chat-messages`・`response_mode: streaming`)で叩ける。アプリの API キーは console の `/apps/<app_id>/api-keys` から取れる。app_id は EC2 の `~/dkg_app_id.txt` / `~/dkg_guided_app_id.txt`。

**道具(2026-09-25・決92 で 0.3.8→0.3.9 に使った)**: `deploy/dify/tools/dify_plugin_swap.py`(`status` / `swap <pkg> <version>` = upload → uninstall → install → version を待つ / `dsl <app_id>` / `apikey <app_id>`)と `dify_smoke.py`(公開 API で 1 往復)。EC2 の `~/` にも同じもの。**pkg は `git archive <sha> deploy/dify/plugin/jrtokai_kb` を EC2 の `~/dify-plugin`(0.6.10)で梱包**(旧版も同じ手順で先に作る)。uninstall → install → version が変わるまで約 3 秒・DSL 2 本(dkg・guided)は自動で新識別子に解決した(2 回目の実績)。画面(Streamlit)は `docker run --env-file`(前のコンテナの `KG_*` を写す)+ `docker network connect jrtokai-v9_default`。

**Why:** install が成功を返すのに版が上がらず、原因の特定に時間を使った。
**How to apply:** 旧版 pkg を作る → uninstall → install → `plugin/list` の version で確認 → スモーク。関連: [[dify-demo-ec2]] [[safety-mark-display]] [[ec2-nginx-conf-canonical]]
