---
title: Шаг 3. Подключение к SQL с помощью pyodbc
description: Шаг 3 — это подтверждение концепции, которое показывает, как можно подключиться к SQL Server с помощью Python и pyODBC. В этих простых примерах демонстрируется выделение и вставка данных.
ms.custom: ''
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4bfd6e52-817d-4f0a-a33d-11466e3f0484
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 808cb17d29362e76fdcc69171b21c9ffffd5c1ed
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837131"
---
# <a name="step-3-proof-of-concept-connecting-to-sql-using-pyodbc"></a>Шаг 3. Подтверждение концепции, подразумевающее подключение к SQL с помощью pyodbc

Этот пример является подтверждением концепции. Пример кода упрощен для ясности и для него не гарантируется соблюдение рекомендаций корпорации Майкрософт.  

Чтобы приступить к работе, выполните следующий пример скрипта. Создайте файл с именем test.py и добавляйте фрагменты кода по ходу работы.

```
> python test.py
```
  
## <a name="connect"></a>Подключение  
  
```python
import pyodbc 
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'tcp:myserver.database.windows.net' 
database = 'mydb' 
username = 'myusername' 
password = 'mypassword' 
cnxn = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()

```  

## <a name="run-query"></a>Выполнение запроса  
  
Функция cursor.execute может использоваться для извлечения результирующего набора из запроса к базе данных SQL. Эта функция принимает запрос и возвращает результирующий набор, по которому может быть выполнена итерация с использованием cursor.fetchone().

```python
#Sample select query
cursor.execute("SELECT @@version;") 
row = cursor.fetchone() 
while row: 
    print(row[0])
    row = cursor.fetchone()

```  
  
## <a name="insert-a-row"></a>Вставка строки  
  
В этом примере вы узнаете, как безопасно выполнить инструкцию [INSERT](../../../t-sql/statements/insert-transact-sql.md) и передать параметры. Параметры защищают приложение от [внедрения кода SQL](../../../relational-databases/tables/primary-and-foreign-key-constraints.md).

```python
#Sample insert query
count = cursor.execute("""
INSERT INTO SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) 
VALUES (?,?,?,?,?)""",
'SQL Server Express New 20', 'SQLEXPRESS New 20', 0, 0, CURRENT_TIMESTAMP).rowcount
cnxn.commit()
print('Rows inserted: ' + str(count))
```  

## <a name="azure-active-directory-and-the-connection-string"></a>Azure Active Directory и строка подключения

pyODBC использует драйвер Microsoft ODBC для SQL Server.
Если ваша версия драйвера ODBC — 17.1 или более поздняя, интерактивный режим Azure Active Directory драйвера ODBC можно использовать через pyODBC.
Этот интерактивный параметр работает, если Python и pyODBC разрешают драйверу ODBC отображать диалоговое окно. Этот параметр доступен только в ОС Windows.

### <a name="example-connection-string-for-azure-active-directory-interactive-authentication"></a>Пример строки подключения для использования с интерактивной проверкой подлинности Azure Active Directory

В следующем примере представлена строка подключения ODBC, определяющая интерактивную проверку подлинности Azure Active Directory.

`server=Server;database=Database;UID=UserName;Authentication=ActiveDirectoryInteractive;`

См. сведения о параметрах проверки подлинности драйвера ODBC в руководстве по [использованию Azure Active Directory с драйвером ODBC](../../odbc/using-azure-active-directory.md#new-andor-modified-dsn-and-connection-string-keywords).

## <a name="next-steps"></a>Дальнейшие действия
  
Дополнительную информацию можно найти в [Центре разработчика Python](https://azure.microsoft.com/develop/python/).
