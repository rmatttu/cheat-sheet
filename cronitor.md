# Cronitor

[Simple monitoring for any application | Cron Monitoring | Website Monitoring | & more | Cronitor](https://cronitor.io/index) こちらを使用してみたメモ

## cron設定

```bash
0  2  *  * wed cronitor exec exec-batch        /path/to/launcher.sh
0  0  *  * *   cronitor exec exec-daily-backup /path/to/backup.sh

# * * * * *    cronitor exec <job name>        <command>

# Cronitor alert rehearsal
# */10 *  *  * *   cronitor exec test false
```

`<job-name>`でエントリーが自動的に作られる。
通知に関してはカスタムがあまりできず、外部サービスとの連携推奨のようだ。
