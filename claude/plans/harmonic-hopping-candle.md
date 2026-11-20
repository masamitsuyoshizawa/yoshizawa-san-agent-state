# 計画: AI セッション間の自動の知らせ(ai-hub)の汎用版・3 プロジェクトへの配布・自動立ち上げへの組み込み(2026-11-20)

(この計画ファイルの前の中身 = 決352 の一括追加の計画(11-06)は、承認済みで、決352 の dec に記録が在る。今回の計画で置き換える)

## 1. Context

利用者の依頼(2026-11-20)は 2 つ。
- phase12 で動かしている、Claude Code / Codex / Antigravity の間の「メッセージが届いた」の自動の知らせ(本文は、今までどおり連絡箱 `docs/comms/`)を、**ほかのプロジェクトでも使える汎用版**にして、phase12・mvp2・diagram-dynfw-phase0 へ、実装の指示と配布を行う。
- Windows の更新による再起動のたびに、多数の窓を自動で立ち上げ直す仕組み(`~/ai_agent_environment`・Antigravity が作った)に、**Codex と agy の再開の時に、上の設定を自動で行う立ち上げ**を含めるよう、**ai_agent_environment への指示文**を作る。

### 1-1. 分かった事(確定・コードを読んで確かめた)

| 対象 | 今の状態 |
|---|---|
| 道具 `~/ai-msg-lab`(git・c2d946b・`ai-hub` と `ai-msg-watcher`) | プロジェクトと外部エージェントの見つけ方は、**もう汎用**。3 プロジェクトとも `ai-hub agents` が通る(phase12 = rev・pla(codex)・agy / mvp2 = rev(codex)・agy / dynfw = rev・icx(codex)・iag(agy))。3 プロジェクトとも `docs/comms`・`INBOX.md`・`scripts/comms_dispatch.py` が在る |
| 足りない物 | (1) 冪等な立ち上げの入口(今は、tmux の窓を手で作り、その中でエージェントを起こす)(2) **再起動の後の追い打ち**(見張りは、起動の時に在る依頼を「既存」と記録して打たない → 止まっていた間の依頼が、手の打ち込み待ちになる。今日の rev の 2 件が実例)(3) 見張りの 2 重起動の守り(4) プロジェクトごとの有効・無効(5) 版と手順書 |
| 自動立ち上げ `~/ai_agent_environment/ai-workspace.ps1`(647 行・git 管理なし・Windows 側へ `install-to-windows.sh` で配備) | 窓ごとに `Build-AgentBashLines` の行を base64 にして `wsl.exe -- bash -lic 'source <(…)'` で流す。窓は、題名 `[AI:NN]` で見つける(`--suppressApplicationTitle` なので、tmux の中で題名が変わっても影響なし)。Restart = 窓を閉じて(WM_CLOSE)起こし直す |
| 対象の窓(codex・agy) | 8 窓: phase12 = 06 pla・07 rev・08 agy / mvp2 = 15 rev・16 agy / dynfw = 19 icx・20 iag・21 rev。23 番(ai_agent_environment 自身の agy)は、連絡箱が無いので対象外 |
| 決まりの文書 | mvp2 の外部エージェントの木(rev・agy)に `AGENTS.md` が無い。phase12 と dynfw には在る |

### 1-2. 既定として決めた事(違えば、承認の時に指示を)

| 点 | 既定 | 理由 |
|---|---|---|
| 道具の置き場 | **正本は `~/ai-msg-lab` の 1 か所**(`~/.local/bin` の symlink は今のまま)。各プロジェクトに、道具の写しを配らない | 写しは、版がずれる(前例: 写した道具の決め打ちが、他 PJ を上書きした)|
| 有効化 | プロジェクトの本体ツリーの `.ai-hub.json` に `"enabled": true`。**無ければ、今までどおり(何も変わらない)** | プロジェクトごとに、入れる時期を選べる |
| tmux に入れる窓 | **外部エージェント(codex・agy)だけ**。coord(Claude)の窓は、設定 1 行で後から足せる形にして、**段 2(任意)** にする | coord の窓は、利用者が毎日打つ窓。先に、外部の側を固める |
| 窓を閉じた時 | **エージェントも止まる(今と同じ)** | 立ち上げの仕組みの Restart / Close を、変えなくて済む。驚きが無い |
| 立ち上げの仕組みとの分担 | 立ち上げは、道具に「この窓は対象か」を尋ねるだけ。**再開の引き方(resume・agy の会話の番号)は、ps1 が正本のまま**。道具が無い・失敗 → 今までどおり起こす | 判断を 1 か所に。窓が開かない事故を作らない |

## 2. 作る物

### 2-1. 道具の版 1.0(`~/ai-msg-lab`・私が実装)

| 足す物 | 中身 |
|---|---|
| `ai-hub wrap --check DIR` | その作業場所の窓を、tmux の中で起こす対象か。rc 0 = 対象 / それ以外 = 対象外(プロジェクトが有効でない・外部エージェントでない・tmux が無い) |
| `ai-hub wrap --dir DIR --script FILE` | (1) プロジェクトの tmux と、見張りの窓を、無ければ作る(同時に何本呼ばれても 1 つ = ロック)(2) 略号の窓で `bash -lic 'source FILE'` を起こす(題名・EDITOR などの環境を渡す)(3) その窓だけを映して attach (4) **映している端末が閉じたら、その窓(エージェント)を止める**(tmux の側の hook と、道具の後始末の 2 重)(5) エージェントの窓が 0 になったら、見張りも止める。rc 0 = 起こして、映し終えた / 11 = 起こす前に失敗(呼び手が、今までどおり起こす)/ 20 以上 = 起こした後の誤り(2 重に起こさない) |
| `ai-hub stop [略号]`・`ai-hub version`・`ai-hub status`(全プロジェクトを 1 表に) | 運用の口 |
| 見張り: **再起動の後の追い打ち** | 2 回目以降の起動では、既存の記録を取り直さない。止まっていた間に INBOX に載った依頼に、エージェントが起きた後、**1 度だけ**打つ(INBOX に今も載っている物だけ = 済んだ依頼には打たない) |
| 見張り: 守りの追加 | エージェントの起動から 20 秒は打たない・**人が、その窓で打っている最中(最後の打鍵から 20 秒)は打たない**・見張りは 1 プロジェクト 1 本(ロック)・`"dry_run": true` の設定 |
| 設定 `.ai-hub.json` | `enabled`・`tmux_agents`(既定 = 外部エージェント。`["external","coord"]` で coord も)・`exclude`・既存の `watcher`・`prompts` |
| 文書 | `README.md` の更新・`docs/導入の手順.md`(プロジェクトの coord 向け)・`docs/運用の手順.md`(利用者向け: 状態の見方・止め方・困った時)・`docs/依頼_ai_agent_environment_立上げの組み込み.md`(付録 A の正本)・`VERSION` |

- 使い回す物(新しく書かない): `lib_aimsg.py` の `main_tree`・`discover`・`project_id`・`agent_running`・`pane_dialog`・`send_keys`、`ai-hub` の `cmd_open`(その窓だけを映す作り)、`watcher_core.py` の `_guard`・`flush`(守りは、ここに足す)。
- 変えない物: 打つ文は、固定の 2 文だけ。連絡の本文を、窓に入れない。3 往復の上限・間隔 60 秒・1 日 30 回・一時停止。

### 2-2. ai_agent_environment への指示文

- 全文は **付録 A**。承認の後に、`~/ai_agent_environment/docs/REQUEST_FROM_PHASE12_COORD_AI_HUB_LAUNCH.md` に置く(正本は道具の `docs/`)。**ai_agent_environment のほかのファイルには、私は触れない**(改造は、そこの Antigravity が行う)。
- 改造は、道具の版 1.0 より先に入っても、害が無い作り(道具が「対象外」と答える間は、今までどおり)。

### 2-3. 各プロジェクトへの配布

| プロジェクト | 送り方 | 入れる物(入れるのは、そのプロジェクトの coord) |
|---|---|---|
| phase12(私が coord) | 新しい決め(dec・番号は実物を見て採る)と、担当への info | `.ai-hub.json`(`enabled: true`)・`CLAUDE.md` の「セッション間連絡」に 2 項(下)・rev / agy / pla の `AGENTS.md` に、終端の書き方の節。**演習が動いている間は入れない**(docs の外の差のため) |
| mvp2 | mvp2 の coord へ SendMessage + 依頼の文書(道具の `docs/導入の手順.md` を指す) | `.ai-hub.json`・決まりの文・**rev と agy の木に `AGENTS.md` を作る**(今、無い)。最初は `"dry_run": true` で 1 日 |
| dynfw | diagram-coord へ、同じ | `.ai-hub.json`・決まりの文(`AGENTS.md` は在る)。最初は `"dry_run": true` で 1 日 |

- 私は、mvp2 と dynfw のリポジトリに、書かない。各 coord が、自分のプロジェクトの承認の決まりで入れる。
- phase12 の `CLAUDE.md` に足す 2 項(案・この計画の承認で入れる):
  - 「外部エージェント(rev・pla・agy)への知らせは、自動(ai-hub)。coord が `INBOX.md` を作ると、見張りが、その窓に『新しいメッセージを読んで処理してください』と打つ。利用者に、打ち込みを頼まない。状態は `ai-hub status`。3 往復で止まった時は、利用者に諮ってから `ai-hub reset`。」
  - 「相手に何も求めない連絡は、type を `ack` か `info` にし、`status: closed` で書く(自動の知らせは、これには打たない)。」

## 3. 段取り

| 段 | 中身 | 止まる所 |
|---|---|---|
| 1 | 道具 版 1.0 の実装と試験(`~/ai-msg-lab`・phase12 の本体には触れない・tmux は試験用の別ソケット) | — |
| 2 | 指示文を ai_agent_environment/docs に置く → **利用者が、Antigravity に渡す** | 改造は Antigravity |
| 3 | phase12 に入れる(演習の後)。手で `ai-hub wrap` を、rev の窓で 1 度試す | 実の rev に打つので、**利用者の合図の後** |
| 4 | Antigravity の改造の後: phase12 の 3 窓(06・07・08)で、付録 A の受入の表を回す | 結果を報告 |
| 5 | mvp2・dynfw へ依頼 → 各 1 日のドライラン → 本番 | 各 coord の報告 |
| 6(任意) | coord(Claude)の窓も tmux に(rev の返事が、自動で coord に届く)。phase12 で先に試す | **利用者の指示の後** |

- 今日お伝えした、手の立ち上げ(tmux の 5 行 + `ai-hub open`)は、段 4 が済むまで、そのまま使える。

## 4. 守る事

- 道具は、連絡のヘッダだけを読む。本文・顧客の資料・個人情報を、窓にも、記録にも、入れない。
- 打つのは、(a) そのエージェントの処理が動いている (b) 選択の確認が出ていない (c) 起動から 20 秒を過ぎた (d) 人が打っていない、の全部がそろった窓だけ。
- 段 3 と段 4 で、実のエージェントに打つ前に、利用者の合図をもらう。
- phase12 の main に、コード・設定(`.ai-hub.json`・`CLAUDE.md`)を入れるのは、演習(dkb の段 3 と回し直し)が動いていない時。

## 5. 確かめ方

| 段 | 確かめ | 合格の形 |
|---|---|---|
| V1 単体 | `tests/test_core.py` に足す: 対象かの判定(有効でない・対象外・除外)/ 追い打ち(止まっていた間の依頼に 1 度だけ・済んだ依頼に 0)/ 起動の猶予 / 打鍵の守り / ロック | 全部合格。足した判定を壊す変異(`tests/mutate.py`)を、全部検出 |
| V3 偽物の窓 | `tests/tmux_e2e.py`(別ソケット・偽のエージェント)に足す: `wrap` で、窓と見張りができる / 同時に 3 本呼んで、見張りが 1 本 / 映す端末を閉じると、窓が消える / rc 11 の時に、呼び手が起こす形 / 2 度目の `wrap` が、2 重に起こさない | 全部合格。守りを外した変異で、落ちる事を確かめる |
| V4 実物(段 3) | phase12 の rev の窓を `wrap` で起こし、依頼 1 件 → 自動で打たれる → 返事。見張りを止めて INBOX を更新 → 起こす → 1 度だけ打たれる | 打ち込みの記録と、rev の返事 |
| 立ち上げ(段 4) | 付録 A の §7 の表(a〜i) | Antigravity と私の、両方の結果 |
| 他プロジェクト(段 5) | `ai-msg-watcher --replay-since`(読むだけ)で、今までの連絡の判定を見る → ドライラン 1 日 → 本番 | 余計な打ち込み 0・取りこぼし 0 |

---

## 付録 A: ai_agent_environment への指示文(全文・承認の後に、そのまま置く)

# 依頼: 自動立ち上げ(ai-workspace.ps1)に、AI セッション間の自動の知らせ(ai-hub)の立ち上げを組み込む

- 宛先: `~/ai_agent_environment` の担当(Antigravity)
- 差出人: jrtokai-phase12 の coord(利用者の指示・2026-11-20)
- 返事の置き場: この文書の隣に `REPLY_TO_PHASE12_COORD_AI_HUB_LAUNCH.md`

## 1. 目的

Windows の再起動の後に `start-ai.bat` で窓を立ち上げ直した時、**Codex と agy の窓が、何もしなくても「自動の知らせ」を受けられる状態で起きる**ようにする。今は、人が tmux の窓を手で作り、その中でエージェントを起こし直している。

## 2. 背景(仕組み)

- 各プロジェクトのセッションは、連絡箱(`docs/comms/`)でやり取りする。Claude 同士は、相手に直に知らせられる。**Codex と agy は、知らせを受ける口が無い**ので、今は、人が「新しいメッセージを読んで処理してください」と打っている。
- 道具 `ai-hub`(`~/ai-msg-lab`・`~/.local/bin/ai-hub`)は、この打ち込みを代わりに行う。打てるのは **tmux の中で動いているエージェントだけ**。プロジェクトごとに tmux のセッションを 1 つ持ち、エージェントごとの窓と、見張りの窓を置く。
- そこで、**対象の窓だけ、エージェントを tmux の中で起こし、Windows Terminal の窓は、それを映す**形に変える。対象かどうかは、道具が答える(プロジェクトが有効にしているか・Codex か agy か)。

## 3. 前提

- 道具の版 1.0 以上(`ai-hub version` が `1.` で始まる値を返す)で、`ai-hub wrap` が使える。**版が古い・道具が無い時は、今までどおりに起こす作りにする**(§4 の形なら、そうなる)。だから、この改造は、道具の版 1.0 より先に入れても、害が無い。
- 道具の中身(`~/ai-msg-lab`)は、変えない。要望は、coord へ。

## 4. 変える所(`ai-workspace.ps1` の `Build-AgentBashLines` だけ)

今は、窓ごとに、次の順の行を作っている。

1. 前置き(`AI_WIN_TITLE`・題名・EDITOR・`target_dir`)
2. エージェントの起動(`if cd "$target_dir"; then … resume のやり方 … fi`)
3. 終わり(題名・`exec bash --rcfile …`)

これを、**エージェントが空でない窓の全部**について、次の順に変える(エージェントが空の窓は、今のまま)。

1. 前置き(今のまま)
2. **「エージェントの起動」の行(今の 2 と、それが使う `target_dir` の 2 行)を、一時ファイルに書く**(引用つきの here-document で。中の `$` を、ここで展開しない)
3. 道具に尋ねて、分ける:

```bash
__ai_inner="$(mktemp "${TMPDIR:-/tmp}/ai-inner.XXXXXX")"
cat > "$__ai_inner" <<'__AI_HUB_INNER__'
(今の「target_dir の 2 行」と「エージェントの起動」の行を、そのまま)
__AI_HUB_INNER__
if command -v ai-hub >/dev/null 2>&1 && ai-hub wrap --check "$target_dir" >/dev/null 2>&1; then
    ai-hub wrap --dir "$target_dir" --script "$__ai_inner"
    __ai_rc=$?
    if [ "$__ai_rc" = "11" ]; then
        source "$__ai_inner"        # 道具が、起こす前に失敗した → 今までどおり起こす
    elif [ "$__ai_rc" != "0" ]; then
        echo "[WARN] ai-hub wrap が rc=$__ai_rc で終わった(エージェントは、2 重に起こさない)"
    fi
    cd "$target_dir" 2>/dev/null || true   # エージェントが終わった後の bash を、今と同じ場所に
else
    source "$__ai_inner"            # 対象外・道具が無い → 今までどおり
fi
rm -f "$__ai_inner"
```

4. 終わり(今のまま)

決まり:

| 点 | 中身 |
|---|---|
| 一時ファイルの中身 | **今の行を、そのまま**。resume のやり方(Claude・Codex・agy の会話の番号の引き方・見つからない時の新規)を、変えない・道具の側に写さない。正本は ps1 のまま |
| 入れ子の base64 にしない | 行は、1 度だけ書く(here-document)。`-EncodedCommand` の長さを、今の 1.3 倍以内に(agy の窓が、いちばん長い) |
| `ai-hub wrap --check` | rc 0 の時だけ、`wrap` を使う。それ以外(対象外・道具が古い・tmux が無い)は、今までどおり |
| `ai-hub wrap --dir … --script …` の rc | 0 = エージェントを tmux の中で起こし、窓を閉じる(か、エージェントが終わる)まで映した / 11 = 起こす前に失敗(エージェントは、起きていない)/ それ以外 = 起こした後の誤り |
| 環境 | `AI_WIN_TITLE`・`EDITOR`・`VISUAL`・`CLAUDE_CODE_DISABLE_TERMINAL_TITLE` は、道具が、tmux の中の窓へ渡す。ps1 の側で、足す物は無い |
| 窓を閉じた時 | **エージェントも止まる(今と同じ)**。道具が、映している端末が無くなった窓を、止める |

## 5. 変えない所

- 窓の題名(`[AI:NN] …`)・配置・位置の保存・`-Restart`・`-Close`・`-Watch`・`-Group`・`-Id`。
- `workspaces.json` の形(新しい欄は、要らない)。
- Claude の窓と、23 番(ai_agent_environment の agy)の起こし方(道具が「対象外」と答えるので、今のままになる)。

## 6. 配備と、戻し方

- 変える前に、`ai-workspace.ps1` の写しを、日付つきで残す(このディレクトリは、git 管理でないため)。
- 変えた後に、`./install-to-windows.sh` で、Windows 側へ配る。
- 戻す時は、写しを戻して、配り直す。道具の側で止めるだけなら、プロジェクトの `.ai-hub.json` の `"enabled"` を `false` にする(立ち上げは、今までどおりになる)。

## 7. 受入の確かめ(結果を、返事に書く)

| # | 確かめ | 合格の形 |
|---|---|---|
| a | 道具を、PATH から外して(名を一時的に変えて)、`start-ai.bat -Id 07` | 今までどおりに、Codex が起きる |
| b | 対象外の窓(Claude の 01・agy の 23) | 今までどおり。tmux の中でない |
| c | 対象の窓(phase12 の 07 rev。プロジェクトが有効な時) | Codex が tmux の中で起き、会話が再開される。`ai-hub status` に、窓 rev と、見張りが出る。題名と配置は、今までどおり |
| d | agy の窓(08) | 今と同じ引き方で、同じ会話が再開される |
| e | `restart-ai.bat -Id 07` | 閉じて、起こし直される。Codex の過程は 1 つだけ(2 重に動いていない) |
| f | `-Close -Id 07` | 窓が閉じ、Codex も止まる(tmux に、窓 rev が残らない) |
| g | `start-ai.bat` を 2 度 | 開いている窓は、飛ばされる(今と同じ) |
| h | 24 窓の一括 | 全部が開く。かかる時間が、今と大きく変わらない |
| i | `-EncodedCommand` の長さ | agy の窓で、今の 1.3 倍以内 |

- c〜f は、**実のエージェントを起こすので、coord に、行う時刻を知らせてから**(phase12 の rev と agy には、処理待ちの依頼が在る事がある)。c の前提(プロジェクトの有効化)は、coord が行う。

## 8. 返してほしい物

- 変えた行の範囲と、変える前の写しの名。
- §7 の表の結果(a〜i)。
- `README.md` への追記(「Codex と agy の窓は、プロジェクトが有効にしていれば、tmux の中で起きる」「止めたい時」「戻し方」)。
- Windows 側へ配ったか。

## 9. しない事

- 道具(`~/ai-msg-lab`)と、各プロジェクトのリポジトリを、変えない。
- 連絡箱の本文を、読まない(この改造には、要らない)。
- resume のやり方を、変えない。
