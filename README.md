# ReFramework Template Excel Transaction

Excelファイルのデータをトランザクションとして処理するワークフローテンプレート

## 内容

Excelファイルで管理されているデータを自動処理するロボットを作成するためのテンプレートです。

トランザクションデータを格納したxlsxファイルには以下の形式でデータが格納されていることを想定しています。

| TransactionID | TransactionField1 | TransactionField2 |
| ------------- | ----------------- | ----------------- |
| ID-1          | Field1-1          | Field2-1          |

* Transaction ID: トランザクションID
* Transaction Field1: トランザクションの情報１
* Transaction Field2: トランザクションの情報２

上記３項目は列番号で参照しているため、違う名称でも問題ありません。これら項目は、トランザクションを識別するデータとしてログに出力されるものです。また、列がこれより多い場合も、Process.xamlのパラメータ（in_TransactionItem）にすべての列データが渡されます。

## 設定

Data¥Condig.xlsxで以下の設定ができます。

Settingタブ

| Name                     | Default                  | Description                              |
| ------------------------ | ------------------------ | ---------------------------------------- |
| logF_BusinessProcessName | Framework Excel Template | ワークフローの名称（ログに出力されます） |
| TransactionData          | -                        | トランザクションデータのファイル名       |
| TransactionDataSheet     | -                        | トランザクションデータのシート名         |

Constantsタブ

| Name           | Default | Description  |
| -------------- | ------- | ------------ |
| MaxRetryNumber | 3       | リトライ回数 |

## 利用方法

1. テンプレート一式をダウンロードします。

2. project.jsonの"name","description"を修正します。

3. Data¥Config.xmlxにトランザクションのファイル名、シート名を設定します。

4. Process.xamlを実装します。

  in_TransactionItem（DataRow型）にトランザクションデータが渡されます。データの不備等で処理を再実行できない場合は、BRE(Business Rule Exception)をThrowするように実装してください。

5. トランザクションテストデータを準備します。

  Test_Framework\_TransactionTest.xsmlにテストデータを準備します。形式は、以下の通り。
  TransactionID, TransactionField1, TransactionField2は、トランザクションデータと同様。Exceptionは、期待する実行結果で以下の3つから選択します。

  * Success: 正常終了
  * BRE: Business Rule Exception
  * AppEx: System Exception

| TransactionID | TransactionField1 | TransactionField2 | Exception   |
| ------------- | ----------------- | ----------------- | ----------- |
| ID-1          | Field1-1          | Field2-1          | Exception-1 |


6. トランザクションテストを実施します。

  _TransactionTest.xamlを実行することにより_TransactionTest.xsmlの各行のデータでProcess.xamlをテストします。テスト結果は、Test_Framework\_TransactionTest.xsmlのResultタブと、Test_Framework\TransactionTestLog.txtに出力されます。
