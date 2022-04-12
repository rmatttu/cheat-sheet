# zabbix

## server

### Check command

サーバからzabbix agentへのデータ取得確認コマンド

```bash
zabbix_get -s <IP> -p <PORT> -k <ITEM_KEY>
```

sample

```bash
zabbix_get -s 123.123.123.123 -p 10050 -k "system.cpu.load[all]"
zabbix_get -s 123.123.123.124 -p 1058 -k "gpu.temp"
```

## agent

