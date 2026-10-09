---
name: ec2-kg-api-deploy-path
description: EC2 kg_api の実体は /app/kg_api/app.py(WorkingDir)。/app/app.py へ入れても無効
metadata:
  type: project
---

EC2 コンテナ `jrtokai-v9-kg-api` は `WorkingDir=/app/kg_api`、CMD=`uvicorn app:app --port 8600`。
したがって**実際に読み込まれるのは `/app/kg_api/app.py` と `/app/kg_api/glossary.json`**。
`/app/app.py` にも同名ファイルが存在するため、そちらへ `docker cp` しても**変更は一切反映されない**(2026-08-12に半日を空費して判明)。

**How to apply**: 反映は必ず両方 or `/app/kg_api/` へ。
`scp kg_api/app.py kg_api/glossary.json kb-demo-ec2:/tmp/` →
`docker cp /tmp/app.py jrtokai-v9-kg-api:/app/kg_api/app.py`(glossary.json も同様) →
`docker restart` → `curl http://172.31.4.27:8600/docs` が200 → `docker commit jrtokai-v9-kg-api jrtokai-v9-kg-api:latest-persisted`。
**Why**: 反映確認は「回答が変わったか」ではなく `docker exec ... grep -c <新規シンボル> /app/kg_api/app.py` で行う。
プロンプト調整が効かないときは、まずこの反映先を疑う。[[eval-improvement-progress]]
