# aws_dynamodb_tutorial
[AWS SDK for Python (Boto) を使用した DynamoDB のサンプルアプリケーション: Tic-tac-toe](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/TicTacToe.html) を実施します。

# install  
```
# 仮想環境作成
python3 -m venv venv
source venv/bin/activate

# パッケージインストール
pip install -r requirements.txt

# 8000port を使っているものがないか確認
docker ps 
# db 起動 
docker-compose up -d
# 起動確認
docker ps
```

# サーバーを立てる
```shell
cd ./dynamodb-tictactoe-example-app
python application.py --mode local --serverPort=5111 --port=8000
```

## エラー回避
```shell
ImportError: cannot import name 'Mapping' from 'collections'
```
と出てしまった時はエラーメッセージに出ている `<どこかへのパス>/python<version>/collections/__init__.py` に `from _collections_abc import Mapping` を1行追加すると治る。   
[参考](https://coronene.hatenablog.com/entry/python/importerror)


# 操作方法
[1.2: ゲームアプリケーションをテストします](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/TicTacToe.Phase1.html)

# データの確認

## テーブルの確認
```shell
aws dynamodb list-tables --endpoint-url http://localhost:8000
```
<details>
<summary>リザルト</summary>
<div>

```
{
    "TableNames": [
        "Games"
    ]
}
```

</div>
</details>

## データ一覧
```shell
aws dynamodb scan --table-name Games --endpoint-url http://localhost:8000
```

<details>
<summary>リザルト</summary>
<div>

```
{
    "Items": [
        {
            "BottomMiddle": {
                "S": "O"
            },
            "TopMiddle": {
                "S": "X"
            },
            "HostId": {
                "S": " user1"
            },
            "TopRight": {
                "S": "X"
            },
            "MiddleRight": {
                "S": "O"
            },
            "Result": {
                "S": "Tie"
            },
            "BottomRight": {
                "S": "X"
            },
            "OpponentId": {
                "S": "user2"
            },
            "StatusDate": {
                "S": "FINISHED_2023-07-31 20:00:27.620755"
            },
            "BottomLeft": {
                "S": "O"
            },
            "MiddleLeft": {
                "S": "X"
            },
            "Turn": {
                "S": "N/A"
            },
            "OUser": {
                "S": " user1"
            },
            "MiddleMiddle": {
                "S": "X"
            },
            "GameId": {
                "S": "e2a14aa4-8252-4178-955d-ab2f4b1dd8f3"
            },
            "TopLeft": {
                "S": "O"
            }
        },
        {
            "OpponentId": {
                "S": "user2"
            },
            "StatusDate": {
                "S": "IN_PROGRESS_2023-07-31 20:37:06.806542"
            },
            "BottomMiddle": {
                "S": "O"
            },
            "TopMiddle": {
                "S": "X"
            },
            "Turn": {
                "S": "user1"
            },
            "OUser": {
                "S": "user1"
            },
            "HostId": {
                "S": "user1"
            },
            "MiddleMiddle": {
                "S": "X"
            },
            "GameId": {
                "S": "9bd862a9-3015-4b11-a5c5-02b03101d45d"
            }
        }
    ],
    "Count": 2,
    "ScannedCount": 2,
    "ConsumedCapacity": null
}
```

</div>
</details>

## テーブルの詳細
```shell
aws dynamodb describe-table --table-name Games --endpoint-url http://localhost:8000
```

<details>
<summary>リザルト</summary>
<div>

```
{
    "Table": {
        "AttributeDefinitions": [
            {
                "AttributeName": "GameId",
                "AttributeType": "S"
            },
            {
                "AttributeName": "HostId",
                "AttributeType": "S"
            },
            {
                "AttributeName": "StatusDate",
                "AttributeType": "S"
            },
            {
                "AttributeName": "OpponentId",
                "AttributeType": "S"
            }
        ],
        "TableName": "Games",
        "KeySchema": [
            {
                "AttributeName": "GameId",
                "KeyType": "HASH"
            }
        ],
        "TableStatus": "ACTIVE",
        "CreationDateTime": "2023-07-31T19:57:43.701000+09:00",
        "ProvisionedThroughput": {
            "LastIncreaseDateTime": "1970-01-01T09:00:00+09:00",
            "LastDecreaseDateTime": "1970-01-01T09:00:00+09:00",
            "NumberOfDecreasesToday": 0,
            "ReadCapacityUnits": 1,
            "WriteCapacityUnits": 1
        },
        "TableSizeBytes": 557,
        "ItemCount": 3,
        "TableArn": "arn:aws:dynamodb:ddblocal:000000000000:table/Games",
        "GlobalSecondaryIndexes": [
            {
                "IndexName": "HostId-StatusDate-index",
                "KeySchema": [
                    {
                        "AttributeName": "HostId",
                        "KeyType": "HASH"
                    },
                    {
                        "AttributeName": "StatusDate",
                        "KeyType": "RANGE"
                    }
                ],
                "Projection": {
                    "ProjectionType": "ALL"
                },
                "IndexStatus": "ACTIVE",
                "ProvisionedThroughput": {
                    "ReadCapacityUnits": 1,
                    "WriteCapacityUnits": 1
                },
                "IndexSizeBytes": 557,
                "ItemCount": 3,
                "IndexArn": "arn:aws:dynamodb:ddblocal:000000000000:table/Games/index/HostId-StatusDate-index"
            },
            {
                "IndexName": "OpponentId-StatusDate-index",
                "KeySchema": [
                    {
                        "AttributeName": "OpponentId",
                        "KeyType": "HASH"
                    },
                    {
                        "AttributeName": "StatusDate",
                        "KeyType": "RANGE"
                    }
                ],
                "Projection": {
                    "ProjectionType": "ALL"
                },
                "IndexStatus": "ACTIVE",
                "ProvisionedThroughput": {
                    "ReadCapacityUnits": 1,
                    "WriteCapacityUnits": 1
                },
                "IndexSizeBytes": 557,
                "ItemCount": 3,
                "IndexArn": "arn:aws:dynamodb:ddblocal:000000000000:table/Games/index/OpponentId-StatusDate-index"
            }
        ]
    }
}
```

</div>
</details>


※ 参考  
[aws cli で DynamoDB を使う](https://qiita.com/ekzemplaro/items/93c0aef433a2b633ab4a)

## AWS CLI が入っていない場合
### Mac
```shell
$ brew install awscli
$ aws configure
AWS Access Key ID [None]: <Access Key ID>
AWS Secret Access Key [None]:  <Secret Access Key>
Default region name [None]: ap-northeast-1
Default output format [None]: <無し>
```
