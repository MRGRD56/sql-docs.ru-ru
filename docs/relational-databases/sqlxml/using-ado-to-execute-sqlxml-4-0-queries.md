---
title: Использование ADO для выполнения запросов SQLXML 4.0
description: Узнайте, как выполнять запросы SQLXML 4,0 в приложении на основе COM с помощью расширений SQLXML для объекты данных ActiveX (ADO).
ms.custom: ''
ms.date: 12/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- query testers [SQLXML]
- test scripts
- ADO [SQLXML]
- queries [SQLXML], ADO
- SQLXML, ADO
ms.assetid: 3d54e3bb-7c5f-427e-82f8-1403a54c4f53
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4af0a1c4aa0c942092149dbb398c46da895ef7b7
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490402"
---
# <a name="using-ado-to-execute-sqlxml-40-queries"></a>Использование ADO для выполнения запросов SQLXML 4.0
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В предыдущих версиях SQLXML выполнение запросов по HTTP поддерживалось с помощью виртуальных каталогов SQLXML в IIS и ISAPI-фильтра SQLXML. В SQLXML 4.0 эти компоненты были удалены, так как похожая и перекрывающаяся функциональность предоставляется собственными веб-службами с поддержкой XML, начиная с версии [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 В качестве альтернативы можно выполнять запросы и использовать SQLXML 4.0 с приложениями на основе COM, используя расширения SQLXML для объектов данных ActiveX (ADO), которые появились в компонентах доступа к данным (MDAC) версии 2.6 и более поздних.  
  
 В этом разделе демонстрируется использование SQLXML и ADO в качестве части приложения Visual Basic Scripting Edition (VBScript) (скрипт с расширением имени файла vbs). Обеспечивает начальную процедуру установки, которая помогает создать повторно и тестировать образцы запросов в документации SQLXML 4.0.  
  
## <a name="creating-the-sqlxml-40-test-script"></a>Создание тестового скрипта SQLXML 4.0  
 В этой процедуре создается файл VBScript (.vbs), Sqlxml4test.vbs, который можно использовать для выполнения запросов SQLXML с использованием ADO-расширений SQLXML в ADO 2.6 и более поздних версий.  
  
#### <a name="to-create-the-sqlxml-40-query-tester-using-ado-vbscript"></a>Создание испытателя запросов SQLXML 4.0 с использованием ADO (VBScript)  
  
1.  Скопируйте следующий код и вставьте его в текстовый файл. Сохраните файл с именем Sqlxml4test.xml.  
  
    ```  
    WScript.Echo "Query process may take a few seconds to complete. Please be patient."  
  
    ' Note that for SQL Server Native Client to be used as the data provider,  
    ' it needs to be installed on the client computer first. Also, SQLXML extensions   
    ' for ADO are used and available in MDAC 2.6 or later.  
  
    'Set script variables.  
    inputFile = "@@FILE_NAME@@"  
    strServer = "@@SERVER_NAME@@"  
    strDatabase = "@@DATABASE_NAME@@"  
    dbGuid = "{5d531cb2-e6ed-11d2-b252-00c04f681b71}"  
  
    ' Establish ADO connection to SQL Server and   
    ' create an instance of the ADO Command object.  
    Set conn = CreateObject("ADODB.Connection")  
    Set cmd = CreateObject("ADODB.Command")  
    conn.Open "Provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Server=" & strServer & _  
              ";Database=" & strDatabase & ";Integrated Security=SSPI"  
    Set cmd.ActiveConnection = conn  
  
    ' Create the input stream as an instance of the ADO Stream object.  
    Set inStream = CreateObject("ADODB.Stream")  
    inStream.Open  
    inStream.Charset = "utf-8"  
    inStream.LoadFromFile inputFile  
  
    ' Set ADO Command instance to use input stream.  
    Set cmd.CommandStream = inStream  
  
    ' Set the command dialect.  
    cmd.Dialect = dbGuid  
  
    ' Set a second ADO Stream instance for use as a results stream.   
    Set outStream = CreateObject("ADODB.Stream")  
    outStream.Open  
  
    ' Set dynamic properties used by the SQLXML ADO command instance.   
    cmd.Properties("XML Root").Value = "ROOT"  
    cmd.Properties("Output Encoding").Value = "UTF-8"  
  
    ' Connect the results stream to the command instance and execute the command.  
    cmd.Properties("Output Stream").Value = outStream  
    cmd.Execute , , 1024  
  
    ' Echo cropped/partial results to console.  
    WScript.Echo Left(outStream.ReadText, 1023)  
  
    inStream.Close  
    outStream.Close  
    ```  
  
2.  Обновите следующие значения скриптов для образца, который предстоит протестировать, и тестовой среды.  
  
    -   Найдите «`@@FILE_NAME@@`» и замените его именем файла шаблона.  
  
    -   Найдите «`@@SERVER_NAME@@`» и замените его именем экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (например, «`(local)`», если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется локально).  
  
    -   Найдите «`@@DATABASE_NAME@@`» и замените его именем базы данных (например, «`AdventureWorks2012`» или «`tempdb`»).  
  
     Обновите любые другие значения, упомянутые в конкретных инструкциях для примера, который нужно создать повторно локально на компьютере.  
  
3.  Сохраните файл и закройте его.  
  
4.  Проверьте, что созданы все дополнительные файлы, такие как XML-шаблоны или схемы, которые являются частью образца, который нужно локально создать повторно на компьютере. Эти файлы должны находиться в том же каталоге, в котором сохранен файл тестового скрипта (Sqlxml4test.vbs).  
  
5.  Следуйте инструкциям следующего раздела по использованию тестового скрипта SQLXML 4.0.  

## <a name="using-the-sqlxml-40-test-script"></a>Использование тестового скрипта SQLXML 4.0  
 Следующая процедура описывает способ использования файлов Sqlxml4test.vbs для тестирования примеров запросов, предоставленных в этой документации.  
  
#### <a name="to-use-the-sqlxml-40-query-tester"></a>Использование испытателя запросов SQLXML 4.0  
  
1.  Проверьте, что установлен собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
    1.  В меню " **Пуск** " выберите пункт " **Параметры**", а затем " **Панель управления**".  
  
    2.  В панели управления откройте окно **Установка и удаление программ** .  
  
    3.  В списке установленных программ убедитесь, что в списке присутствует **Microsoft SQL Server Native Client** .  
  
        > [!NOTE]  
        >  Если необходимо установить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный клиент, см. статью [Установка SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md).  
  
2.  Проверьте, что на клиентском компьютере установлена версия MDAC 2.6 или более поздняя. Если необходимо проверить сведения о версии MDAC, можно использовать средство проверки компонентов MDAC, которое предоставляется бесплатно для загрузки с веб-сайта корпорации Майкрософт [http://www.microsoft.com](https://www.microsoft.com) . Чтобы получить дополнительные сведения, выполните поиск с ключевыми словами «MDAC Component Checker» на веб-сайте корпорации Майкрософт.  
  
3.  Выполните скрипт.  
  
     Файл VBScript можно выполнить из командной строки с использованием Cscript.exe или дважды щелкнув файл Sqlxml4test.vbs, чтобы вызвать сервер скриптов Windows (WScript.exe).  
  
     При выполнении скрипт должен вывести сообщение, предупреждая, что выполнение скрипта может занять некоторое время перед возвратом и отображением результатов запроса как вывода скрипта. После того как данные будут выведены, сравните их с ожидаемыми результатами запроса.  
  
  
