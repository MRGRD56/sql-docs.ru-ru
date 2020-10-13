---
title: Доступ к данным FILESTREAM с помощью OpenSqlFilestream | Документация Майкрософт
description: Узнайте, как получить доступ к данным FILESTREAM с OpenSqlFilestream. Просмотрите примеры использования API для управления Win32.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
apiname:
- OpenSqlFilestream
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- OpenSqlFilestream
ms.assetid: d8205653-93dd-4599-8cdf-f9199074025f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 959bc875e314f0fbb79b51e4f9ea36dbf5414dc9
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91810280"
---
# <a name="access-filestream-data-with-opensqlfilestream"></a>Доступ к данным FILESTREAM с OpenSqlFilestream
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  API OpenSqlFilestream получает дескриптор файла, совместимый с Win32, для большого двоичного объекта FILESTREAM, хранящегося в файловой системе. Дескриптор может быть передан в любой из следующих API-интерфейсов Win32: [ReadFile](https://go.microsoft.com/fwlink/?LinkId=86422), [WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile), [TransmitFile](/windows/win32/api/mswsock/nf-mswsock-transmitfile), [SetFilePointer](/windows/win32/api/fileapi/nf-fileapi-setfilepointer), [SetEndOfFile](/windows/win32/api/fileapi/nf-fileapi-setendoffile) или [FlushFileBuffers](/windows/win32/api/fileapi/nf-fileapi-flushfilebuffers). При передаче этого дескриптора любому другому API Win32 будет возвращена ошибка ERROR_ACCESS_DENIED. Дескриптор следует закрыть, передав его API-интерфейсу Win32 [CloseHandle](/windows/win32/api/handleapi/nf-handleapi-closehandle) перед фиксацией или откатом транзакции. Если дескриптор не будет закрыт, то это вызовет утечку ресурсов со стороны сервера.  
  
 Весь доступ к контейнеру данных FILESTREAM осуществляется в транзакции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[tsql](../../includes/tsql-md.md)] могут выполняться в ней же. Это обеспечивает согласованность данных SQL и данных FILESTREAM BLOB.  
  
 Чтобы получить доступ к объекту FILESTREAM BLOB с помощью Win32, необходимо включить [авторизацию Windows](../../relational-databases/security/choose-an-authentication-mode.md) .  
  
> [!IMPORTANT]  
>  Если открывается доступ к файлу для записи, то транзакция принадлежит агенту FILESTREAM. Пока транзакция не будет разблокирована, возможен только файловый ввод-вывод Win32. Для освобождении транзакции дескриптор записи должен быть закрыт.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HANDLE OpenSqlFilestream (  
    LPCWSTR FilestreamPath,  
    SQL_FILESTREAM_DESIRED_ACCESS DesiredAccess,  
    ULONG OpenOptions,  
    LPBYTE FilestreamTransactionContext,  
    SIZE_T FilestreamTransactionContextLength,  
    PLARGE_INTEGER AllocationSize);  
```  
  
#### <a name="parameters"></a>Параметры  
 *FilestreamPath*  
 [входящее значение] Путь типа **nvarchar(max)** , который возвращается функцией [PathName](../../relational-databases/system-functions/pathname-transact-sql.md) . PathName можно вызывать только из контекста учетной записи, которая имеет разрешения SELECT или UPDATE [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для таблицы FILESTREAM и столбца.  
  
 *DesiredAccess*  
 [in] Задает режим, используемый при доступе к данным FILESTREAM BLOB. Это значение передается функции [DeviceIoControl Function](/windows/win32/api/ioapiset/nf-ioapiset-deviceiocontrol).  
  
|Имя|Значение|Значение|  
|----------|-----------|-------------|  
|SQL_FILESTREAM_READ|0|Данные могут быть считаны из файла.|  
|SQL_FILESTREAM_WRITE|1|Данные могут быть записаны в файл.|  
|SQL_FILESTREAM_READWRITE|2|Данные могут быть считаны из файла и записаны в него.|  
  
> [!NOTE]  
>  Эти значения определяются в перечислении SQL_FILESTREAM_DESIRED_ACCESS, расположенном в файле sqlncli.h.  
  
 *OpenOptions*  
 [in] Атрибуты и флаги файла. В этот параметр может входить любое сочетание следующих флагов.  
  
|Флаг|Значение|Значение|  
|----------|-----------|-------------|  
|SQL_FILESTREAM_OPEN_NONE|0x00000000L|Файл открывается или создается без специальных параметров.|  
|SQL_FILESTREAM_OPEN_FLAG_ASYNC|0x00000001L|Файл открывается или создается для асинхронного ввода-вывода.|  
|SQL_FILESTREAM_OPEN_FLAG_NO_BUFFERING|0x00000002L|Система открывает файл, не используя системное кэширование.|  
|SQL_FILESTREAM_OPEN_FLAG_NO_WRITE_THROUGH|0x00000004L|Система не записывает данные в промежуточный кэш. Запись производится непосредственно на диск.|  
|SQL_FILESTREAM_OPEN_FLAG_SEQUENTIAL_SCAN|0x00000008L|Доступ к файлу осуществляется последовательно от начала до конца. Система может использовать это в качестве указания для оптимизации кэширования файлов. Если приложение перемещает указатель файла для случайного доступа, кэширование может произойти неоптимально.|  
|SQL_FILESTREAM_OPEN_FLAG_RANDOM_ACCESS|0x00000010L|Доступ к файлу осуществляется случайным образом. Система может использовать это в качестве указания для оптимизации кэширования файлов.|  
  
 *FilestreamTransactionContext*  
 [входящее значение] Значение, возвращаемое функцией [GET_FILESTREAM_TRANSACTION_CONTEXT](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md) .  
  
 *FilestreamTransactionContextLength*  
 [входящее значение] Число байтов в данных типа **varbinary(max)** , возвращаемых функцией GET_FILESTREAM_TRANSACTION_CONTEXT. Функция возвращает массив из N байт. Число N определяется функцией и является свойством возвращаемого массива байт.  
  
 *AllocationSize*  
 [in] Указывает изначально выделяемый размер файла данных в байтах. Не учитывается в режиме чтения. Этот параметр может иметь значение NULL, в этом случае используется поведение файловой системы по умолчанию.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Если функция завершается успешно, возвращается открытый дескриптор на указанный файл. Если функция завершается неудачно, возвращается значение INVALID_HANDLE_VALUE. Дополнительные сведения об ошибке можно получить с помощью функции GetLastError().  
  
## <a name="examples"></a>Примеры  
 В следующих примерах показано, как можно использовать функцию API `OpenSqlFilestream` для получения дескриптора Win32.  
  
 [!code-cs[FILESTREAM#FS_CS_ReadAndWriteBLOB](../../relational-databases/blob/codesnippet/csharp/access-filestream-data-w_0_1.cs)]  
  
 [!code-vb[FILESTREAM#FS_VB_ReadAndWriteBLOB](../../relational-databases/blob/codesnippet/visualbasic/access-filestream-data-w_0_2.vb)]  
  
 [!code-cpp[FILESTREAM#FS_CPP_WriteBLOB](../../relational-databases/blob/codesnippet/cpp/access-filestream-data-w_0_3.cpp)]  
  
## <a name="remarks"></a>Remarks  
 Чтобы использовать эту функцию API, должен быть установлен собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client устанавливается совместно с клиентскими средствами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в статье [Установка SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md).  
  
## <a name="see-also"></a>См. также:  
 [Большой двоичный объект &#40;BLOB-объект& #41;Данные&#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)   
 [Создание частичных обновлений данных FILESTREAM](../../relational-databases/blob/make-partial-updates-to-filestream-data.md)   
 [Избегание конфликтов в операциях баз данных в приложениях FILESTREAM](../../relational-databases/blob/avoid-conflicts-with-database-operations-in-filestream-applications.md)  
  
