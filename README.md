## docker 起動

```
docker compose up
```

## 環境変数設定

```
export SPANNER_EMULATOR_HOST=localhost:9010
```

## spanner-cli

インストール

```
go install github.com/cloudspannerecosystem/spanner-cli@latest
```

ログイン

```
spanner-cli -p test-project -i test-instance -d test-database
```

## wrench

インストール

```
go install github.com:cloudspannerecosystem/wrench@latest
```

wrench用に環境変数設定

```
export SPANNER_PROJECT_ID=test-project
export SPANNER_INSTANCE_ID=test-instance
export SPANNER_DATABASE_ID=test-database
```

### マイグレーションファイル作成

```
wrench migrate create --directory ./migrations/ddl
```

### マイグレーション実行

```
wrench migrate up --directory ./migrations/ddl
```

## golang-migrate

インストール

```
brew-h install golang-migrate
```

マイグレーションファイル作成

```
migrate create -ext sql -dir migrations/ddl/golang-migrations -seq create_photos_table
```

マイグレート実行

```
migrate -source file://./migrations/ddl/golang-migrations/ -database 'spanner://projects/test-project/instances/test-instance/databases/test-database?x-clean-statements=True' up
```

テーブル確認

```
spanner> show tables;
+-------------------------+
| Tables_in_test-database |
+-------------------------+
| Photos                  |
| SchemaMigrations        |
| Albums                  |
| Singers                 |
+-------------------------+
4 rows in set (0.02 sec)

spanner> select * from SchemaMigrations;
+---------+-------+
| Version | Dirty |
+---------+-------+
| 1       | false |
+---------+-------+
1 rows in set (3.26175ms)
```
