| カラム名 | データ型 | 制約 | 説明 |
|:----------|:--------|:-----|:-----|
| id | BIGINT | PRAIMARY KEY.AUTO_INCREMENT | 商品を1件ずつ区別する番号 |
| name | VARCHAR(50) | NOT NULL | 商品の名前 |
| praice | INT | NOT NULL | 値段（円） |
|description | VARCHAR(100) | - | 短い説明 |
| category | ENUM | NOT NULL | ドリンクかフードかの区分 |
| is_available | BOOLEAN | NOT NULL.DEAFALT true | その日に売れるかどうか。売れ切れたらfalseにする |
| created_at | TIMESTAMP | - | 登録した日時 |
| updated_at | TIMESTAMP | - | 最後に書き換えた日時 |


| カラム名 | データ型 | 制約 | 説明 |
|:---------|:---------|:-----|:----|
| id | BIGINT | PRAIMARY KEY.AUTO_INCREMENT | 注文を1件ずつ区別する番号 |
| order_number | VARCHAR(6) | NOT NULL.UNIQUE | お客様にお伝えする６桁の注文番号 |
| total_amount | INT | NOT NULL | 合計金額（円） |
| status | VARCHAR(20) | NOT NULL | 受け取りの状態。取りうる値はchapter 6で決めます |
| ordered_at | DATETIME | NOT NULL | 注文を受けた日時 |
| created_at | TIMESTAMP | - | 登録した日時 |
| updated_at | TIMESTAMP | - | 最後に書き換えた日時 |


| 機能 | 商品 | 注文 | 注文明細 |
|:------|:----|:-----|---------|
| F-01メニューの閲覧 | R | - | - |
| F-02商品の注文 | R | C | C |
| F-03注文状況の確認 | R | R | R |
| F-04注文の取り消し | - | U | - |
| F-05注文の受け渡し | R | R・U | R |
