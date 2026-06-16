# WA 預SEND站 — 交俾主腦管理

衛星機（madkidshk gh = OAuth，冇 workflow scope）已推晒所有非-workflow 嘢。
站已 live：https://madkidshk.github.io/wa-presend/（Pages = branch main，.nojekyll）。
**淨低要主腦做（有 workflow scope + runtime owner）：**

## 1. 啟動自動 send（cron）
```
git mv _setup/send.yml .github/workflows/send.yml
git add -A && git commit -m "activate WA presend cron" && git push
```
→ 每 5 分鐘掃 `queue/` 夠鐘嘅 → CallMeBot send → 一次性移去 `sent/`、重複就自動排下一次。

## 2. Secrets（已設，verify 下）
`gh secret list -R madkidshk/wa-presend` → 應有 `CALLMEBOT_PHONE`、`CALLMEBOT_APIKEY`
（由 `~/.config/madkids/callmebot.phone` + `.key` set，同主機一直用嗰對）。

## 3. 站寫入權限（token，per-device）
靜態頁要 fine-grained PAT（**wa-presend, Contents R/W**）先寫到 `queue/`。
- 存 browser localStorage key `idealab_pat`，或
- 用書籤網址 `https://<domain>/#t=<TOKEN>`（站已支援 `#t=`，免每次貼、抗 app 內置瀏覽器）。
（電話/CallMeBot key 唔使落 browser —— 喺 Secrets，server 送。）

## 4. 自訂域名 wa.madkids.hk（可選）
- DNS：`CNAME  wa → madkidshk.github.io`
- repo：`gh api -X PUT repos/madkidshk/wa-presend/pages -f cname=wa.madkids.hk`（或 Settings→Pages→Custom domain），GitHub 自動發 HTTPS。
- 換 domain 後，書籤改 `https://wa.madkids.hk/#t=<TOKEN>` 即可，token 唔使重貼。

## 架構
`index.html`(站) → GitHub API 寫 `queue/<id>.json` `{id, send_at(UTC ISO), text, created, repeat:{freq,interval}|null}` → `send.py`(cron) 讀夠鐘嘅 → CallMeBot。時間 UTC 存 / HKT 顯示。重複：freq=daily/weekly/monthly/yearly、interval=任意 N（cron 做唔到「每 N 月/年」，所以邏輯喺 send.py）。
