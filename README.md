Collecting workspace information```markdown
# Misskey Kubernetes Helm Chart

Kubernetes上でMisskeyを実行するためのHelm chartです。

## 機能

- PostgreSQLクラスター (CrunchyData Operator使用)
- Redis (複数インスタンス対応)
  - メインRedis
  - Pub/Sub用Redis (オプション)
  - ジョブキュー用Redis (オプション) 
  - タイムライン用Redis (オプション)
  - リアクション用Redis (オプション)
- Misskey Webサーバー
- 永続化ボリューム対応

## 必要要件

- Kubernetes 1.19+
- Helm 3.0+
- [CrunchyData PostgreSQL Operator](https://github.com/CrunchyData/postgres-operator)

## インストール方法

1. PostgreSQL Operatorをインストール:

```bash
kubectl apply -k https://github.com/CrunchyData/postgres-operator-examples/kustomize/install
```

2. values.yamlの編集

基本的にバニラMisskeyの設定と同様です。

3. このHelmチャートをインストール:

```bash
helm install misskey ./helm -f helm/values.yaml
```

## 設定

### 主な設定項目

- `misskey.web.replicaCount` - Misskeyのレプリカ数
- `misskey.web.image` - Misskeyのイメージ設定
- `misskey.persistence` - ファイルストレージの永続化設定
- `misskey.config.*` - Misskey本体の設定

### Redis分散設定

以下のRedisインスタンスを個別に有効化できます:

```yaml
redisPubsub:
  enabled: true 
redisJobQueue:
  enabled: true
redisTimelines:
  enabled: true
redisReactions:
  enabled: true
```

### データベース設定

PostgreSQLクラスターは自動的に作成され、必要な認証情報はSecretとして保存されます。

## 補足

- すべての設定は values.yaml で上書き可能です
- デフォルトでは1つのRedisインスタンスが使用されます
- PostgreSQLのバックアップは PGBackRestを使用して設定されます
