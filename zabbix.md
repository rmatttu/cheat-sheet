# zabbix

## server

### 新規ホスト作成

下記設定で新規作成

* ホスト名: `<HOST>`
* テンプレート: [`Linux by Zabbix agent`, `Zabbix server health`]
* グループ: [`Zabbix servers`]
* インターフェース > 追加
  * タイプ: エージェント
  * IPアドレス: 対象ホストのIPアドレスを入力（e.g. 123.123.123.123）
  * ポート: agentの設定ファイル（`zabbix_agentd.conf`）の`ListenPort`

homeのルータもポートマッピングを調整すること


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

`zabbix_agentd.conf`を設定

* Server: zabbixサーバのIPアドレス
* ListenPort: このホストがzabbixサーバと疎通するためのポート
* DebugLevel: デフォルト: 3
* UserParameter: オプション、カスタムパラメータ定義（サーバ側から取得設定をすること）

例

```conf
Server=123.123.123.123
ListenPort=10050

UserParameter=gpu.temp,nvidia-smi --query-gpu=temperature.gpu --format=csv,noheader,nounits -i 0
```

