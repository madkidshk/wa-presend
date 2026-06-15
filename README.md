# WA 預SEND站

寫定 msg + 揀日子時間 + 重複（每 N 日/週/月/年）→ GitHub 雲端 cron 每 5 分鐘經 CallMeBot 自動 send 去 WhatsApp（24/7，唔使開頁）。

- 站：index.html（Pages）
- 引擎：send.py（cron 行）
- queue/<id>.json 一檔一 msg；sent/ 為已送（一次性）
- Secrets：CALLMEBOT_PHONE / CALLMEBOT_APIKEY
