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

# 入门教程
{: #getting-started-with-cloudant}

在本 {{site.data.keyword.cloudantfull}} 入门教程中，我们将使用 Python 来创建一个 {{site.data.keyword.cloudant_short_notm}} 数据库，并使用简单的数据集合来填充该数据库。
{:shortdesc}

除本教程外，还可以查看我们的实践教程，以了解有关 {{site.data.keyword.cloudant_short_notm}} 的更多信息。或者，尝试查看以下某个专注于特定语言的教程：

- [Liberty for Java 和 {{site.data.keyword.cloudant_short_notm}} ![外部链接图标](images/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/runtimes/liberty/getting-started.html#getting-started-tutorial){:new_window}
- [Node.js 和 {{site.data.keyword.cloudant_short_notm}} ![外部链接图标](images/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/runtimes/nodejs/getting-started.html#getting-started-tutorial){:new_window}
- [Swift 和 {{site.data.keyword.cloudant_short_notm}} ![外部链接图标](images/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/runtimes/swift/getting-started.html#getting-started-tutorial){:new_window}

要获取更多特定于语言的教程，请参阅[开始部署第一个应用程序 ![外部链接图标](images/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/){:new_window}。 

<div id="prerequisites"></div>

## 准备工作
{: #prereqs}

您需要 [{{site.data.keyword.cloud}} 帐户 ![外部链接图标](images/launch-glyph.svg "外部链接图标")](https://console.ng.bluemix.net/registration/){:new_window} 以及 {{site.data.keyword.cloudant_short_notm}} 服务的实例，并满足以下 Python 需求：

*	在系统上安装最新版本的 [Python 编程语言 ![外部链接图标](images/launch-glyph.svg "外部链接图标")](https://www.python.org/){:new_window}。
	
	要检查 Python，请在提示符处运行以下命令：
	```sh
	python --version
	```
	{:pre}
	
	您会看到类似于以下内容的结果：

	```
	Python 2.7.12
	```
	{:screen}

*	安装 [Python 库](libraries/supported.html#python)，以支持 Python 应用程序在 {{site.data.keyword.cloud_notm}} 上使用 {{site.data.keyword.cloudant_short_notm}}。
	
	要检查是否已成功安装该客户机库，请在提示符处运行以下命令：
	```sh
	pip freeze
	```
	{:pre}
	
	您会看到系统上安装的所有 Python 模块的列表。检查该列表，查找类似于以下内容的 {{site.data.keyword.cloudant_short_notm}} 条目：

	```
	cloudant==2.3.1
	```
	{:screen}
	
	如果未安装 `cloudant` 模块，请使用类似于以下内容的命令进行安装：
	
	```
	pip install cloudant==2.3.1
	```
	{:pre}

## 第 1 步：连接到 {{site.data.keyword.cloud_notm}} 上的 {{site.data.keyword.cloudant_short_notm}} 服务实例
{: #step-1-connect-to-your-cloudant-nosql-db-service-instance-on-ibm-cloud}

1.	运行 {{site.data.keyword.cloudant_short_notm}} 客户机库组件的 `import` 语句，以支持 Python 应用程序连接到 {{site.data.keyword.cloudant_short_notm}} 服务实例。
	```python
	from cloudant.client import Cloudant
	from cloudant.error import CloudantException
	from cloudant.result import Result, ResultByKey
	```
	{: codeblock}

2.  创建新的 {{site.data.keyword.cloudant_short_notm}} 服务凭证：
  <br>在 {{site.data.keyword.cloud_notm}} 控制台中，打开服务实例的仪表板。
  <br>在左侧导航中，单击`服务凭证`。<br>a. 单击`新建凭证`按钮。
  <br>![创建新的服务凭证](tutorials/images/img0050.png)
  <br>b. 在“添加新凭证”窗口中，输入新凭证的名称，如以下屏幕快照所示。
  <br>c.（可选）添加内联配置参数。
  <br>d. 单击`添加`按钮。
  <br>![添加新的服务凭证](tutorials/images/img0051.png)
  <br>您的凭证会添加到服务凭证表中。
  <br>e. 单击“操作”下的`查看凭证`。
  <br>![查看所有服务凭证](tutorials/images/img0052.png)
  <br>这将显示服务凭证的详细信息：
  <br>![{{site.data.keyword.cloudant_short_notm}} 服务凭证](tutorials/images/img0009.png)
   
3.	通过运行以下命令，建立与服务实例的连接。替换为先前步骤中获得的服务凭证：
	```python
	client = Cloudant("<username>", "<password>", url="<url>")
	client.connect()
	```
	{: codeblock}


## 第 2 步：创建数据库
{: #step-2-create-a-database}

1. 在 Python 应用程序中定义变量：
  ```python
  databaseName = "<yourDatabaseName>"
  ```
  {: codeblock}
  ... 其中，`<yourDatabaseName>` 是您希望为数据库提供的名称。
 

  数据库名称必须以字母开头，并且只能包含小写字符 (a-z)、数字 (0-9) 以及以下任意字符：`_`、`$`、`(`、`)`、`+`、`-` 和 `/`。
  {: tip}

2. 创建数据库：
  ```python
  myDatabase = client.create_database(databaseName)
  ```
  {: codeblock}

3. 确认数据库是否已成功创建：
  ```python
  if myDatabase.exists():
      print "'{0}' successfully created.\n".format(databaseName)
  ```
  {: codeblock}

## 第 3 步：将很小的数据集合存储为数据库中的文档
{: #step-3-store-a-small-collection-of-data-as-documents-within-the-database}

1. 定义数据的集合：
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

2. 使用 Python 代码“逐步”完成数据，并将其转换为 JSON 文档。每个文档都存储在数据库中：

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

  请注意，我们会检查以确保每个文档都已成功创建。
  {: tip}

## 第 4 步：通过查询检索数据
{: #step-4-retrieving-data-through-queries}

一个小型数据集合已存储为数据库中的文档。您可以在数据库中对这些数据执行最少检索或完整检索。最少检索可获取_有关_文档的基本数据。完整检索还会包含文档_内_的数据。

* 要运行最少检索，请执行以下操作：
  1. 首先，请求数据库中所有文档的列表。
    ```python
    result_collection = Result(myDatabase.all_docs)
    ```      
    {: codeblock}

    此列表会作为数组返回。

  2. 显示数组中元素的内容。
    ```python
    print "Retrieved minimal document:\n{0}\n".format(result_collection[0])
    ```
    {: codeblock}

    结果类似于以下示例：
    
    ```
    [{u'value': {u'rev': u'1-106e76a2612ea13468b2f243ea75c9b1'}, u'id': u'14be111aac74534cf8d390eaa57db888', u'key': u'14be111aac74534cf8d390eaa57db888'}]
    ```
    {:screen}
    
    `u` 前缀指示 Python 显示的是 Unicode 字符串。
    {: tip}

    如果我们对显示内容做进一步整理，可以看到返回的最少文档详细信息与以下示例相同：
    
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

    认为数据库中存储的第一个文档始终是结果列表中返回的第一个文档，这种想法不一定适用于 NoSQL 数据库，如 {{site.data.keyword.cloudant_short_notm}}。
    {: tip}

* 要运行完整检索，请在数据库中请求所有文档的列表，并通过提供 `include_docs` 选项来指定还必须返回文档内容。
  ```python
  result_collection = Result(myDatabase.all_docs, include_docs=True)
  print "Retrieved full document:\n{0}\n".format(result_collection[0])
  ```
  {: codeblock}
  
  结果类似于以下示例：
  ```
  [{u'value': {u'rev': u'1-7130413a8c7c5f1de5528fe4d373045c'}, u'id': u'0cfc7d902f613d5fdb7b7818e262353b', u'key': u'0cfc7d902f613d5fdb7b7818e262353b', u'doc': {u'temperatureField': 40, u'descriptionField': u'hot', u'numberField': 2, u'nameField': u'two', u'_id': u'0cfc7d902f613d5fdb7b7818e262353b', u'_rev': u'1-7130413a8c7c5f1de5528fe4d373045c'}}]
  ```
  {: screen}
  
  如果我们对显示内容做进一步整理，可以看到返回的完整文档详细信息与以下示例相同：
  
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

## 第 5 步：通过 {{site.data.keyword.cloudant_short_notm}} API 端点检索数据
{: #step-5-retrieving-data-through-the-cloudant-nosql-db-api-endpoint}

您还可以通过调用 {{site.data.keyword.cloudant_short_notm}} [`/_all_docs` 端点](api/database.html#get-documents)来请求所有文档及其内容的列表。

1. 确定要联系的端点，以及要随调用一起提供的任何参数：
  ```python
  end_point = '{0}/{1}'.format("<url>", databaseName + "/_all_docs")
  params = {'include_docs': 'true'}
  ```
  {: codeblock}
  ... 其中，`<url>` 是在第 1 步中找到的服务凭证中的 URL 值。


2. 向服务实例发送请求，然后显示的结果如下：
  ```python
  response = client.r_session.get(end_point, params=params)
  print "{0}\n".format(response.json())
  ```
  {: codeblock}

  结果类似于以下_缩略的_示例：
  
  ```
  {u'rows': [{u'value': {u'rev': u'1-6d8cb5905316bf3dbe4075f30daa9f59'}, u'id': u'0532feb6fd6180d79b842d871316c444', u'key': u'0532feb6fd6180d79b842d871316c444', u'doc': {u'temperatureField': 20, u'descriptionField': u'warm', u'numberField': 3, u'nameField': u'three', u'_id': u'0532feb6fd6180d79b842d871316c444', u'_rev': u'1-6d8cb5905316bf3dbe4075f30daa9f59'}}, ... , {u'value': {u'rev': u'1-3f61736fa96473d358365ce1665e3d97'}, u'id': u'db396f77bbe12a567b09177b4accbdbc', u'key': u'db396f77bbe12a567b09177b4accbdbc', u'doc': {u'temperatureField': 0, u'descriptionField': u'freezing', u'numberField': 5, u'nameField': u'five', u'_id': u'db396f77bbe12a567b09177b4accbdbc', u'_rev': u'1-3f61736fa96473d358365ce1665e3d97'}}], u'total_rows': 5, u'offset': 0}
  ```
  {:screen}
  
  我们可以稍微整理一下显示内容，可看到获得的详细信息（_缩略版_）类似于以下示例：
  
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
              "value":
              {
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

## 第 6 步：删除数据库
{: #step-6-delete-the-database}

完成对数据库的操作后，可以将其删除。

```python
try :
    client.delete_database(databaseName)
except CloudantException:
    print "There was a problem deleting '{0}'.\n".format(databaseName)
else:
    print "'{0}' successfully deleted.\n".format(databaseName)
```
{: codeblock}

我们提供了一些基本的错误处理，向您展示如何对潜在问题进行故障诊断并予以解决。

## 第 7 步：关闭与服务实例的连接
{: #step-7-close-the-connection-to-the-service-instance}

最后一步是断开 Python 客户机应用程序与服务实例的连接：

```python
client.disconnect()
```
{: codeblock}

## 后续步骤
{: #next-steps}

有关所有 {{site.data.keyword.cloudant_short_notm}} 产品的更多信息，请参阅 [{{site.data.keyword.cloudant_short_notm}} ![外部链接图标](images/launch-glyph.svg "外部链接图标")](http://www.ibm.com/analytics/us/en/technology/cloud-data-services/cloudant/){:new_window} 主站点。

有关 {{site.data.keyword.cloudant_short_notm}} 概念、任务和方法的更多详细信息和教程，请参阅 [{{site.data.keyword.cloudant_short_notm}} 文档](cloudant.html)。

## 附录：完整的 Python 代码列表
{: #appendix-complete-python-code-listing}

完整的 Python 代码列表如下所示。请务必将 `<username>`,
`<password>` 和 `<url>` 值替换为您的服务凭证。同样，将 `<yourDatabaseName>` 值替换为您的数据库的名称。

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
