---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

<!-- Acrolinx: 2018-05-31 -->

# チュートリアルの概説
{: #getting-started-with-cloudant}

この {{site.data.keyword.cloudantfull}} チュートリアルの概説では、Python を使用して、{{site.data.keyword.cloudant_short_notm}} データベースを作成し、そのデータベースにシンプルなデータ・コレクションを取り込みます。
{:shortdesc}

このチュートリアルに加えて、{{site.data.keyword.cloudant_short_notm}} について詳しく知りたい場合に役立つ実践チュートリアルを参照してください。 または、特定の言語に焦点を当てたチュートリアルの 1 つを試してみてください。

- [Liberty for Java および {{site.data.keyword.cloudant_short_notm}} ![外部リンク・アイコン](images/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/runtimes/liberty/getting-started.html#getting-started-tutorial){:new_window}
- [Node.js および {{site.data.keyword.cloudant_short_notm}} ![外部リンク・アイコン](images/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/runtimes/nodejs/getting-started.html#getting-started-tutorial){:new_window}
- [Swift および {{site.data.keyword.cloudant_short_notm}} ![外部リンク・アイコン](images/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/runtimes/swift/getting-started.html#getting-started-tutorial){:new_window}

その他の言語固有のチュートリアルについては、『[Get started by deploying your first app ![外部リンク・アイコン](images/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/){:new_window}』を参照してください。 

<div id="prerequisites"></div>

## 始める前に
{: #prereqs}

[{{site.data.keyword.cloud}} アカウント ![外部リンク・アイコン](images/launch-glyph.svg "外部リンク・アイコン")](https://console.ng.bluemix.net/registration/){:new_window} および {{site.data.keyword.cloudant_short_notm}} サービスのインスタンスが必要であり、また以下の Python 要件が満たされている必要があります。

*	最新バージョンの
	[Python プログラミング言語 ![外部リンク・アイコン](images/launch-glyph.svg "外部リンク・アイコン")](https://www.python.org/){:new_window} をシステムにインストールします。
	
	確認するには、プロンプトで以下のコマンドを実行します。
	```sh
	python --version
	```
	{:pre}
	
	次のような結果が表示されます。

	```
	Python 2.7.12
	```
	{:screen}

*	[Python ライブラリー](libraries/supported.html#python)をインストールして、Python アプリケーションで {{site.data.keyword.cloudant_short_notm}} 上の {{site.data.keyword.cloud_notm}} を処理できるようにします。
	
	クライアント・ライブラリーが正常にインストールされたことを確認するには、
プロンプトで以下のコマンドを実行します。
	```sh
	pip freeze
	```
	{:pre}
	
	システムにインストールされているすべての Python モジュールのリストが表示されます。リストを調べ、以下のような {{site.data.keyword.cloudant_short_notm}} 項目を探します。

	```
	cloudant==2.3.1
	```
	{:screen}
	
	`cloudant` モジュールがインストールされていない場合は、以下のようなコマンドを使用してインストールします。
	
	```
	pip install cloudant==2.3.1
	```
	{:pre}

## ステップ 1: {{site.data.keyword.cloudant_short_notm}} 上の {{site.data.keyword.cloud_notm}} サービス・インスタンスに接続する
{: #step-1-connect-to-your-cloudant-nosql-db-service-instance-on-ibm-cloud}

1.	{{site.data.keyword.cloudant_short_notm}} クライアント・ライブラリー・コンポーネントの `import` ステートメントを実行して、Python アプリケーションが {{site.data.keyword.cloudant_short_notm}} サービス・インスタンスに接続できるようにします。
	```python
	from cloudant.client import Cloudant
	from cloudant.error import CloudantException
	from cloudant.result import Result, ResultByKey
	```
	{: codeblock}

2.  以下のようにして、新規の {{site.data.keyword.cloudant_short_notm}} サービス資格情報を作成します。
  <br>{{site.data.keyword.cloud_notm}} コンソールで、サービス・インスタンスのダッシュボードを開きます。
  <br>左ナビゲーションで、`「サービス資格情報」`をクリックします。
  <br>a. `「新規資格情報」`ボタンをクリックします。
  <br>![新規サービス資格情報の作成](tutorials/images/img0050.png)
  <br>b. 次のスクリーン・ショットに示されているように、「新規資格情報の追加」ウィンドウで新規資格情報の名前を入力します。
  <br>c. (オプション) インライン構成パラメーターを追加します。
  <br>d. `「追加」`ボタンをクリックします。 
  <br>![新規サービス資格情報の追加](tutorials/images/img0051.png)
  <br>資格情報が「サービス資格情報」テーブルに追加されます。
  <br>e.「アクション」で`「資格情報の表示」`をクリックします。
  <br>![すべてのサービス資格情報の表示](tutorials/images/img0052.png)
  <br>以下のようにサービス資格情報の詳細が表示されます。<br>![{{site.data.keyword.cloudant_short_notm}} サービス資格情報](tutorials/images/img0009.png)
   
3.	以下のコマンドを実行して、サービス・インスタンスへの接続を確立します。
	前のステップで確認したサービス資格情報に置き換えてください。
	```python
	client = Cloudant("<username>", "<password>", url="<url>")
	client.connect()
	```
	{: codeblock}


## ステップ 2: データベースを作成する
{: #step-2-create-a-database}

1. 以下のように、Python アプリケーションで変数を定義します。
  ```python
  databaseName = "<yourDatabaseName>"
  ```
  {: codeblock}
  ... ここで、`<yourDatabaseName>` は、データベースに付ける名前です。 

  データベース名は、文字で始まる必要があり、小文字 (a から z)、数字 (0 から 9)、記号 `_`、`$`、`(`、 `)`、`+`、`-`、および `/` のみを含むことができます。
  {: tip}

2. 以下のように、データベースを作成します。
  ```python
  myDatabase = client.create_database(databaseName)
  ```
  {: codeblock}

3. 以下のように、データベースが正常に作成されたことを確認します。
  ```python
  if myDatabase.exists():
      print "'{0}' successfully created.\n".format(databaseName)
  ```
  {: codeblock}

## ステップ 3: データベース内に文書として小規模なデータ・コレクションを保管する
{: #step-3-store-a-small-collection-of-data-as-documents-within-the-database}

1. 以下のように、データ・コレクションを定義します。
  ```python
  sampleData = [
      [1, "one", "boiling", 100],
      [2, "two", "hot", 40],
      [3, "three", "warm", 20],
      [4, "four", "cold", 10],
      [5, "five", "freezing", 0]
    ]
  ```
  {: codeblock}

2. Python コードを使用して、データを順に処理し、JSON 文書に変換します。
  各文書は、以下のようにデータベースに保管されます。

  ```python
  # Create documents by using the sample data.
  # Go through each row in the array
  for document in sampleData:
    # Retrieve the fields in each row.
    number = document[0]
    name = document[1]
    description = document[2]
    temperature = document[3]

    # Create a JSON document that represents
    # all the data in the row.
    jsonDocument = {
        "numberField": number,
        "nameField": name,
        "descriptionField": description,
        "temperatureField": temperature
    }

    # Create a document by using the database API.
    newDocument = myDatabase.create_document(jsonDocument)

    # Check that the document exists in the database.
    if newDocument.exists():
        print "Document '{0}' successfully created.".format(number)
  ```
  {: codeblock}

  各文書が正常に作成されたかを確認することに注意してください。
  {: tip}

## ステップ 4: 照会を介してデータを取得する
{: #step-4-retrieving-data-through-queries}

小規模なデータ・コレクションがデータベース内に文書として保管されました。
データベースからのそのデータの最小限の取得または完全な取得を実行します。
最小限の取得では、文書_に関する_ 基本データを取得します。
完全な取得では、文書_内の_ データも含まれます。

* 最小限に取得を実行するには、以下のようにします。
  1. まず、データベース内のすべての文書のリストを要求します。
    ```python
    result_collection = Result(myDatabase.all_docs)
    ```      
    {: codeblock}

    このリストは、配列として返されます。

  2. 配列内の要素の内容を表示します。
    ```python
    print "Retrieved minimal document:\n{0}\n".format(result_collection[0])
    ```
    {: codeblock}

    結果は、以下の例のようになります。
    
    ```
    [{u'value': {u'rev': u'1-106e76a2612ea13468b2f243ea75c9b1'}, u'id': u'14be111aac74534cf8d390eaa57db888', u'key': u'14be111aac74534cf8d390eaa57db888'}]
    ```
    {:screen}
    
    `u` 接頭部は、Python が Unicode ストリングを表示していることを示す標識です。 
    {: tip}

    外観を少々整えると、返された最小限の文書明細が以下の例に相当することが分かります。
    
    ```json
    [
        {
            "value": {
                "rev": "1-106e76a2612ea13468b2f243ea75c9b1"
            },
            "id": "14be111aac74534cf8d390eaa57db888",
            "key": "14be111aac74534cf8d390eaa57db888"
        }
    ]
    ```
    {: codeblock}

    {{site.data.keyword.cloudant_short_notm}} などの NoSQL データベースでは、データベースに保管されている最初の文書が、結果のリストでは最初に返されない場合もあるため注意が必要です。
    {: tip}

* 完全な取得を実行するには、データベース内のすべての文書のリストを要求し、`include_docs` オプションを指定することで、文書の内容も返す必要があることを指定します。
  ```python
  result_collection = Result(myDatabase.all_docs, include_docs=True)
  print "Retrieved full document:\n{0}\n".format(result_collection[0])
  ```
  {: codeblock}
  
  結果は、以下の例のようになります。
  ```
  [{u'value': {u'rev': u'1-7130413a8c7c5f1de5528fe4d373045c'}, u'id': u'0cfc7d902f613d5fdb7b7818e262353b', u'key': u'0cfc7d902f613d5fdb7b7818e262353b', u'doc': {u'temperatureField': 40, u'descriptionField': u'hot', u'numberField': 2, u'nameField': u'two', u'_id': u'0cfc7d902f613d5fdb7b7818e262353b', u'_rev': u'1-7130413a8c7c5f1de5528fe4d373045c'}}]
  ```
  {: screen}
  
  外観を少々整えると、返された完全な文書明細が以下の例に相当することが分かります。
  
  ```json
  [
    {
      "value": {
        "rev": "1-7130413a8c7c5f1de5528fe4d373045c
      },
      "id": "0cfc7d902f613d5fdb7b7818e262353b",
      "key": "0cfc7d902f613d5fdb7b7818e262353b",
      "doc": {
        "temperatureField": 40,
        "descriptionField": "hot",
        "numberField": 2,
        "nameField": "two",
        "_id": "0cfc7d902f613d5fdb7b7818e262353b",
        "_rev": "1-7130413a8c7c5f1de5528fe4d373045c"
      }
    }
  ]
  ```
  {: codeblock}

## ステップ 5: {{site.data.keyword.cloudant_short_notm}} API エンドポイントを介してデータを取得する
{: #step-5-retrieving-data-through-the-cloudant-nosql-db-api-endpoint}

{{site.data.keyword.cloudant_short_notm}} [`/_all_docs` エンドポイント](api/database.html#get-documents)を呼び出すことで、すべての文書のリストとその内容を要求することもできます。

1. 以下のように、接続するエンドポイント、および呼び出しで渡すパラメーターを指定します。
  ```python
  end_point = '{0}/{1}'.format("<url>", databaseName + "/_all_docs")
  params = {'include_docs': 'true'}
  ```
  {: codeblock}
  ... ここで、`<url>` は、ステップ 1 で確認したサービス資格情報の URL 値です。

2. 以下のように、要求をサービス・インスタンスに送信し、結果を表示します。
  ```python
  response = client.r_session.get(end_point, params=params)
  print "{0}\n".format(response.json())
  ```
  {: codeblock}

  結果は、以下の例 (_一部省略_) のようになります。
  
  ```
  {u'rows': [{u'value': {u'rev': u'1-6d8cb5905316bf3dbe4075f30daa9f59'}, u'id': u'0532feb6fd6180d79b842d871316c444', u'key': u'0532feb6fd6180d79b842d871316c444', u'doc': {u'temperatureField': 20, u'descriptionField': u'warm', u'numberField': 3, u'nameField': u'three', u'_id': u'0532feb6fd6180d79b842d871316c444', u'_rev': u'1-6d8cb5905316bf3dbe4075f30daa9f59'}}, ... , {u'value': {u'rev': u'1-3f61736fa96473d358365ce1665e3d97'}, u'id': u'db396f77bbe12a567b09177b4accbdbc', u'key': u'db396f77bbe12a567b09177b4accbdbc', u'doc': {u'temperatureField': 0, u'descriptionField': u'freezing', u'numberField': 5, u'nameField': u'five', u'_id': u'db396f77bbe12a567b09177b4accbdbc', u'_rev': u'1-3f61736fa96473d358365ce1665e3d97'}}], u'total_rows': 5, u'offset': 0}
  ```
  {:screen}
  
  外観を少々整えると、返された明細 (_一部省略_) が、以下の例のようなものであることが分かります。
  
  ```json
  {
      "rows": [
          {
              "value": {
                "rev": "1-6d8cb5905316bf3dbe4075f30daa9f59"
              },
              "id": "0532feb6fd6180d79b842d871316c444",
              "key": "0532feb6fd6180d79b842d871316c444",
              "doc": {
                  "temperatureField": 20,
                  "descriptionField": "warm",
                  "numberField": 3,
                  "nameField": "three",
                  "_id": "0532feb6fd6180d79b842d871316c444",
                  "_rev": "1-6d8cb5905316bf3dbe4075f30daa9f59"
              }
          },
          ...
          {
              "value": {
                "rev": "1-6d8cb5905316bf3dbe4075f30daa9f59"
              },
              "id": "db396f77bbe12a567b09177b4accbdbc",
              "key": "db396f77bbe12a567b09177b4accbdbc",
              "doc": {
                  "temperatureField": 0,
                  "descriptionField": "freezing",
                  "numberField": 5,
                  "nameField": "five",
                  "_id": "db396f77bbe12a567b09177b4accbdbc",
                  "_rev": "1-6d8cb5905316bf3dbe4075f30daa9f59"
              }
          }
      ],
      "total_rows": 5,
      "offset": 0
  }
  ```
  {: codeblock}

## ステップ 6: データベースを削除する
{: #step-6-delete-the-database}

データベースの処理が終わったら、データベースを削除できます。

```python
try :
    client.delete_database(databaseName)
except CloudantException:
    print "There was a problem deleting '{0}'.\n".format(databaseName)
else:
    print "'{0}' successfully deleted.\n".format(databaseName)
```
{: codeblock}

潜在的な問題のトラブルシューティング方法および対処方法を示すために、
基本的なエラー処理を組み込みました。

## ステップ 7: サービス・インスタンスへの接続を閉じる
{: #step-7-close-the-connection-to-the-service-instance}

最終ステップとして、以下のように、Python クライアント・アプリケーションをサービス・インスタンスから切断します。

```python
client.disconnect()
```
{: codeblock}

## 次のステップ
{: #next-steps}

すべての {{site.data.keyword.cloudant_short_notm}} オファリングについて詳しくは、
メインの [{{site.data.keyword.cloudant_short_notm}}![外部リンク・アイコン](images/launch-glyph.svg "外部リンク・アイコン")](http://www.ibm.com/analytics/us/en/technology/cloud-data-services/cloudant/){:new_window} サイトを参照してください。

{{site.data.keyword.cloudant_short_notm}} の概念、タスク、および技法に関する詳細およびチュートリアルについては、[{{site.data.keyword.cloudant_short_notm}} 資料](cloudant.html)を参照してください。

## 付録: 完全な Python コード・リスト
{: #appendix-complete-python-code-listing}

完全な Python コード・リストは、以下のとおりです。 
必ず、`<username>`、`<password>`、および `<url>` の値をご使用のサービス資格情報に置き換えてください。
同様に、`<yourDatabaseName>` の値をご使用のデータベースの名前に置き換えてください。

```python
from cloudant.client import Cloudant
from cloudant.error import CloudantException
from cloudant.result import Result, ResultByKey

client = Cloudant("<username>", "<password>", url="<url>")
client.connect()

databaseName = "<yourDatabaseName>"

myDatabase = client.create_database(databaseName)

if myDatabase.exists():
    print "'{0}' successfully created.\n".format(databaseName)

sampleData = [
    [1, "one", "boiling", 100],
    [2, "two", "hot", 40],
    [3, "three", "warm", 20],
    [4, "four", "cold", 10],
    [5, "five", "freezing", 0]
]

# Create documents using the sample data.
# Go through each row in the array
for document in sampleData:
    # Retrieve the fields in each row.
    number = document[0]
    name = document[1]
    description = document[2]
    temperature = document[3]

    # Create a JSON document that represents
    # all the data in the row.
    jsonDocument = {
        "numberField": number,
        "nameField": name,
        "descriptionField": description,
        "temperatureField": temperature
    }

    # Create a document using the Database API.
    newDocument = myDatabase.create_document(jsonDocument)

    # Check that the document exists in the database.
    if newDocument.exists():
        print "Document '{0}' successfully created.".format(number)

result_collection = Result(myDatabase.all_docs)

print "Retrieved minimal document:\n{0}\n".format(result_collection[0])

result_collection = Result(myDatabase.all_docs, include_docs=True)
print "Retrieved full document:\n{0}\n".format(result_collection[0])

end_point = '{0}/{1}'.format("<url>", databaseName + "/_all_docs")
params = {'include_docs': 'true'}
response = client.r_session.get(end_point, params=params)
print "{0}\n".format(response.json())


try :
    client.delete_database(databaseName)
except CloudantException:
    print "There was a problem deleting '{0}'.\n".format(databaseName)
else:
    print "'{0}' successfully deleted.\n".format(databaseName)

client.disconnect()

```
{: codeblock}
