---
title: Создание клиентских приложений для данных FILESTREAM | Документация Майкрософт
description: Узнайте, как использовать интерфейсы API Win32 для создания клиентских приложений, обращающихся к данным FILESTREAM. Ознакомьтесь с доступными функциями, необходимыми шагами, примерами и рекомендациями.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], Win32
ms.assetid: 8a02aff6-e54c-40c6-a066-2083e9b090aa
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5de9ab4b9a0e89197173dcee5f0e6f7ffb1b7e75
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91809951"
---
# <a name="create-client-applications-for-filestream-data"></a>Создание клиентских приложений для данных FILESTREAM
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Интерфейсы API Win32 можно использовать для считывания и записи данных в BLOB-объект FILESTREAM. Требуются следующие шаги.  
  
-   Считывание пути к файлу FILESTREAM.  
  
-   Считывание текущего контекста транзакций.  
  
-   Получение дескриптора Win32 и его использование для чтения и записи данных в объект FILESTREAM BLOB.  
  
> [!NOTE]  
>  Для примеров в этом разделе требуется база данных с поддержкой FILESTREAM и таблица, которая создана в разделе [Создание базы данных с поддержкой FILESTREAM](../../relational-databases/blob/create-a-filestream-enabled-database.md) и [Создание таблицы для хранения данных FILESTREAM](../../relational-databases/blob/create-a-table-for-storing-filestream-data.md).  
  
##  <a name="functions-for-working-with-filestream-data"></a><a name="func"></a> Функции для работы с данными FILESTREAM  
 Если для хранения данных больших двоичных объектов (BLOB) используется FILESTREAM, то для работы с файлами могут быть использованы API-интерфейсы Win32. Для поддержки работы с данными FILESTREAM BLOB в приложениях Win32 в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предусмотрены следующие функции и API-интерфейсы.  
  
-   [PathName](../../relational-databases/system-functions/pathname-transact-sql.md) возвращает в BLOB путь в виде токена. Этот токен позволяет приложению получить дескриптор Win32 и работать с данными большого двоичного объекта.  
  
     Если база данных, содержащая данные FILESTREAM, относится к группе доступности AlwaysOn, то функция PathName возвращает имя виртуальной сети (VNN) вместо имени компьютера.  
  
-   [GET_FILESTREAM_TRANSACTION_CONTEXT()](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md) возвращает токен, который представляет текущую транзакцию сеанса. Данный токен позволяет приложению связать потоковые операции файловой системы FILESTREAM с транзакцией.  
  
-   [OpenSqlFilestream API](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md) получает дескриптор файла Win32. Этот дескриптор позволяет приложению передать поток данных FILESTREAM, после чего приложение передает этот дескриптор следующим API-интерфейсам Win32: [ReadFile](https://go.microsoft.com/fwlink/?LinkId=86422), [WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile), [TransmitFile](/windows/win32/api/mswsock/nf-mswsock-transmitfile), [SetFilePointer](/windows/win32/api/fileapi/nf-fileapi-setfilepointer), [SetEndOfFile](/windows/win32/api/fileapi/nf-fileapi-setendoffile) или [FlushFileBuffers](/windows/win32/api/fileapi/nf-fileapi-flushfilebuffers). Если при помощи этого дескриптора приложение вызывает любой другой API-интерфейс, возвращается ошибка ERROR_ACCESS_DENIED. Приложение должно закрыть дескриптор с помощью функции [CloseHandle](/windows/win32/api/handleapi/nf-handleapi-closehandle).  
  
 Весь доступ к контейнеру данных FILESTREAM осуществляется в транзакции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[tsql](../../includes/tsql-md.md)] можно выполнить в одной транзакции, обеспечивая согласованность данных SQL и данных FILESTREAM.  
  
##  <a name="steps-for-accessing-filestream-data"></a><a name="steps"></a> Действия для доступа к данным FILESTREAM  
  
###  <a name="reading-the-filestream-file-path"></a><a name="path"></a> Чтение пути файла FILESTREAM  
 Каждая ячейка в таблице FILESTREAM имеет связанный с ней путь к файлу. Для считывания этого пути используйте свойство **PathName** столбца **varbinary(max)** в инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] . В следующем примере показано, как считать путь файла столбца **varbinary(max)** .  
  
 [!code-sql[FILESTREAM#FS_PathName](../../relational-databases/blob/codesnippet/tsql/create-client-applicatio_1.sql)]  
  
###  <a name="reading-the-transaction-context"></a><a name="trx"></a> Чтение контекста транзакции  
 Чтобы получить текущий контекст транзакции, используйте функцию [!INCLUDE[tsql](../../includes/tsql-md.md)] [GET_FILESTREAM_TRANSACTION_CONTEXT()](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md). В следующем примере показано, как запустить транзакцию и считать текущий контекст транзакции.  
  
 [!code-sql[FILESTREAM#FS_GET_TRANSACTION_CONTEXT](../../relational-databases/blob/codesnippet/tsql/create-client-applicatio_2.sql)]  
  
###  <a name="obtaining-a-win32-file-handle"></a><a name="handle"></a> Получение дескриптора файла Win32  
 API OpenSqlFilestream получает дескриптор файла Win32. Этот API-интерфейс экспортируется из файла sqlncli.dll. Возвращенный дескриптор можно передать в любую из следующих функций API-интерфейса Win32: [ReadFile](https://go.microsoft.com/fwlink/?LinkId=86422), [WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile), [TransmitFile](/windows/win32/api/mswsock/nf-mswsock-transmitfile), [SetFilePointer](/windows/win32/api/fileapi/nf-fileapi-setfilepointer), [SetEndOfFile](/windows/win32/api/fileapi/nf-fileapi-setendoffile) или [FlushFileBuffers](/windows/win32/api/fileapi/nf-fileapi-flushfilebuffers). В следующих примерах показано, как получить дескриптор файла Win32 и использовать его для чтения и записи данных в объект FILESTREAM BLOB.  
  
 [!code-cs[FILESTREAM#FS_CS_ReadAndWriteBLOB](../../relational-databases/blob/codesnippet/csharp/create-client-applicatio_3.cs)]  
  
 [!code-vb[FILESTREAM#FS_VB_ReadAndWriteBLOB](../../relational-databases/blob/codesnippet/visualbasic/create-client-applicatio_4.vb)]  
  
 [!code-cpp[FILESTREAM#FS_CPP_WriteBLOB](../../relational-databases/blob/codesnippet/cpp/create-client-applicatio_5.cpp)]  
  
##  <a name="best-practices-for-application-design-and-implementation"></a><a name="best"></a> Рекомендации по проектированию и реализации приложений  
  
-   При проектировании и реализации приложений, использующих FILESTREAM, примите к сведению следующие рекомендации.  
  
-   Представляйте неинициализированный столбец FILESTREAM с помощью значения NULL, а не 0x. Значение 0x вызовет создание файла, а NULL — нет.  
  
-   Избегайте операций вставки и удаления в таблицах, которые содержат столбцы FILESTREAM, отличные от NULL. Эти операции могут изменять таблицы FILESTREAM, которые используются для сборки мусора. Это может приводить к постепенному снижению производительности приложения.  
  
-   В приложениях, использующих репликацию, следует применять функцию NEWSEQUENTIALID() вместо NEWID(). Функция NEWSEQUENTIALID() создает идентификаторы GUID в этих приложениях эффективнее, чем NEWID().  
  
-   Функции API FILESTREAM предназначены для потокового доступа к данным на платформе Win32. Старайтесь не использовать [!INCLUDE[tsql](../../includes/tsql-md.md)] для чтения или записи двоичных объектов FILESTREAM, если их размер больше 2 МБ. Если необходимо выполнять чтение или запись больших двоичных объектов из [!INCLUDE[tsql](../../includes/tsql-md.md)], убедитесь, что данные объекта использованы до того, как открывать такой объект FILESTREAM BLOB из Win32. Неиспользование всех данных [!INCLUDE[tsql](../../includes/tsql-md.md)] может привести к сбою при открытии или закрытии любого последующего объекта FILESTREAM.  
  
-   Старайтесь не использовать инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] , которые обновляют большие двоичные объекты FILESTREAM или прикрепляют к ним данные. Это приводит к буферизации данных таких объектов в базе данных tempdb с последующей передачей в новый физический файл.  
  
-   Старайтесь избегать мелких обновлений больших двоичных объектов FILESTREAM. Каждая такая операция приводит к копированию базовых файлов FILESTREAM. Если приложению приходится прикреплять небольшие двоичные объекты, их следует записывать в столбец **varbinary(max)** , а затем выполнять одиночную операцию записи в большой двоичный объект FILESTREAM, когда число объектов достигнет установленного предела.  
  
-   Старайтесь не получать размер большого количества файлов больших двоичных объектов в приложении. На эту операцию уходит много времени, поскольку размер не хранится в компоненте [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Если необходимо выяснить размер файла большого двоичного объекта, воспользуйтесь функцией [!INCLUDE[tsql](../../includes/tsql-md.md)] DATALENGTH(), чтобы определить размер объекта, если он закрыт. Функция DATALENGTH() не открывает объект, чтобы определить его размер.  
  
-   Если в приложении используется протокол Message Block1 (SMB1), то для оптимизации производительности данные больших двоичных объектов FILESTREAM должны считываться блоками по 60 килобайт.  
  
## <a name="see-also"></a>См. также:  
 [Избегание конфликтов в операциях баз данных в приложениях FILESTREAM](../../relational-databases/blob/avoid-conflicts-with-database-operations-in-filestream-applications.md)   
 [Доступ к данным FILESTREAM с OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)   
 [Большой двоичный объект &#40;BLOB-объект& #41;Данные&#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)   
 [Создание частичных обновлений данных FILESTREAM](../../relational-databases/blob/make-partial-updates-to-filestream-data.md)  
  
