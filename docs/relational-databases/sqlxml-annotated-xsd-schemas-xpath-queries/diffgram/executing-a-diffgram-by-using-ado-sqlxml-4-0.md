---
title: Исполнение DiffGram с помощью ADO (SQLXML)
description: Узнайте, как выполнить файл DiffGram в приложении Microsoft Visual Basic с помощью ADO (SQLXML 4,0), чтобы установить соединение с экземпляром Microsoft SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- providers [SQLXML], SQLOLEDB Provider
- ADO [SQLXML]
- SQLXMLOLEDB Provider, DiffGrams
- data providers [SQLXML], SQLOLEDB Provider
- DiffGrams [SQLXML], ADO
ms.assetid: 741fce82-de83-4923-86eb-30acb5b9a5e6
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a97baf82eb40f52168d332ee5beb0cc75c283a03
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490564"
---
# <a name="executing-a-diffgram-by-using-ado-sqlxml-40"></a>Выполнение дельты с использованием ADO (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Следующее приложение на языке [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic использует ADO для установки соединения с экземпляром Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и выполняет DiffGram. В этом приложении дельты и схема XSD хранятся в файле. Приложение загружает DiffGram из заданного файла. Вы можете использовать любую из дельтами (и связанную с ней схему XSD), описанную в разделе [примеры DiffGram](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).  
  
 Ниже приводится последовательность действий в образце приложения.  
  
-   Объект **conn** (**ADODB. Соединение**) устанавливает соединение с выполняющимся экземпляром SQL Server на определенном сервере.  
  
-   Объект **cmd** (**ADODB. Command**) выполняется для установленного соединения.  
  
-   В качестве диалекта команды задано значение DBGUID_MSSQLXML.  
  
-   Объект DiffGram копируется в поток команд (**strmIn**) из файла.  
  
-   Поток вывода команды задается как объект **стрмаут** (**ADODB. Stream**) для получения возвращенных данных.  
  
-   При использовании поставщика SQLOLEDB вы по умолчанию получаете функциональность Microsoft SQLXML, предоставляемую библиотекой Sqlxmlx.dll. Чтобы использовать Sqlxml4.dll с поставщиком SQLOLEDB, свойство **версии SQLXML** должно иметь значение **SQLXML. 4.0** для объекта **соединения** поставщика SQLOLEDB.  
  
-   Команда (дельта) будет выполнена.  
  
 Следующий код представляет собой образец приложения.  
  
> [!NOTE]  
>  В коде необходимо задать имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в строке соединения.  
  
```  
Private Sub Command1_Click()  
  Dim cmd As New ADODB.Command  
  Dim conn As New ADODB.Connection  
  Dim strmOut As New ADODB.Stream  
  Dim strmIn As New ADODB.Stream  
  
  'Open a connection to SQL Server.  
  conn.Provider = "SQLOLEDB"  
  conn.Open "server=SqlServerName; database=tempdb; Integrated Security=SSPI; "  
  conn.Properties("SQLXML Version") = "SQLXML.4.0"  
  Set cmd.ActiveConnection = conn  
  strmIn.Open  
  strmIn.Charset = "UTF-8"  
  strmIn.LoadFromFile "C:\SomeFilePath\SampleDiffGram.xml"  
  strmIn.Position = 0  
  Set cmd.CommandStream = strmIn  
  
  strmOut.Open  
  cmd.Properties("Output Stream").Value = strmOut  
  cmd.Properties("Output Encoding").Value = "UTF-8"  
  
  cmd.Dialect = "{5d531cb2-e6ed-11d2-b252-00c04f681b71}"  
  cmd.Properties("Mapping Schema") = "C:\SomeFilePath\SampleDiffGram.xml"  
  cmd.Execute , , adExecuteStream  
  strmOut.Position = 0  
  Set cmd = Nothing  
  strmOut.Charset = "UTF-8"  
  strmOut.SaveToFile "C:\DropIt.txt", adSaveCreateOverWrite  
  strmOut.Close  
  Set strmOut = Nothing  
  
End Sub  
```  
  
### <a name="to-test-the-diffgram"></a>Проверка дельты  
  
1.  В папку на компьютере скопируйте любую из дельтами и соответствующую схему XSD из одного из примеров в [примерах DiffGram](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).  
  
2.  Откройте Visual Basic и создайте проект стандартного исполняемого файла.  
  
3.  Добавьте эти ссылки в проект:  
  
    ```  
    Microsoft ActiveX Data Objects 2.8 Library  
    ```  
  
4.  На панели элементов щелкните **CommandButton**, а затем нарисуйте кнопку в форме.  
  
5.  Дважды нажмите кнопку, чтобы изменить ее код, и добавьте код приложения из раздела.  
  
6.  Отредактируйте код, чтобы в нем указывались имена файлов DiffGram и XSD. Также отредактируйте строку соединения.  
  
7.  Запустите приложение. Результат выполнения зависит от типа DiffGram.  

