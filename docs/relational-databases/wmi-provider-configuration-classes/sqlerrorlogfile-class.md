---
description: SqlErrorLogFile, класс
title: SqlErrorLogFile, класс
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
ms.assetid: 2b83ae4a-c0d4-414c-b6e5-a41ec7c13159
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 11c22c1756e580cc194db336fed4e192f5b67e3b
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91891644"
---
# <a name="sqlerrorlogfile-class"></a>SqlErrorLogFile, класс
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]
  Предоставляет свойства для просмотра информации о файле журнала [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
class SQLErrorLogFile  
{  
   uint32ArchiveNumber;  
   stringInstanceName;  
   datetimeLastModified;  
   uint32LogFileSize;  
   stringName;  
  
};  
```  
  
## <a name="properties"></a>Свойства  
 Класс SQLErrorLogFile определяет следующие свойства.  
  
| Свойство | Description |
| -------- | ----------- |
|арчивенумбер|Тип данных: **UInt32**<br /><br /> Тип доступа: только для чтения<br /><br /> <br /><br /> Номер архива для файла журнала.|  
|InstanceName|Тип данных: **строка**<br /><br /> Тип доступа: только для чтения<br /><br /> Квалификаторы: ключ<br /><br /> <br /><br /> Имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на котором хранится файл журнала.|  
|LastModified|Тип данных: **DateTime**<br /><br /> Тип доступа: только для чтения<br /><br /> <br /><br /> Дата последнего изменения файла журнала.|  
|LogFileSize|Тип данных: **UInt32**<br /><br /> Тип доступа: только для чтения<br /><br /> <br /><br /> Размер файла журнала в байтах.|  
|Имя|Тип данных: **строка**<br /><br /> Тип доступа: только для чтения<br /><br /> Квалификаторы: ключ<br /><br /> <br /><br /> Имя файла журнала.|  
  
## <a name="remarks"></a>Remarks  
  
| Тип | Имя |
| ---- | ---- |
|MOF|Sqlmgmprovider xpsp2up.mof|  
|DLL|Sqlmgmprovider.dll|  
|Пространство имен|\root\Microsoft\SqlServer\ComputerManagement10|  
  
## <a name="example"></a>Пример  
 В этом примере извлекаются данных обо всех файлах журналов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в указанном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Чтобы выполнить пример, замените \<*Instance_Name*> именем экземпляра, например "instance1".  
  
```  
on error resume next  
set strComputer = "."  
set objWMIService = GetObject("winmgmts:\\.\root\Microsoft\SqlServer\ComputerManagement10")  
set LogFiles = objWmiService.ExecQuery("SELECT * FROM SqlErrorLogFile WHERE InstanceName = '<Instance_Name>'")  
  
For Each logFile in LogFiles  
  
WScript.Echo "Instance Name:  " & logFile.InstanceName & vbNewLine _  
    & "Log File Name:  " & logFile.Name & vbNewLine _  
    & "Archive Number: " & logFile.ArchiveNumber & vbNewLine _  
    & "Log File Size:  " & logFile.LogFileSize & " bytes" & vbNewLine _  
    & "Last Modified:  " & logFile.LastModified & vbNewLine _  
  
Next   
```  
  
## <a name="comments"></a>Комментарии  
 Если параметр *instanceName* не указан в инструкции WQL, запрос возвратит сведения для экземпляра по умолчанию. Например, следующая инструкция WQL вернет информацию обо всех файлах журнала в текущем экземпляре (MSSQLSERVER).  
  
```  
"SELECT * FROM SqlErrorLogFile"  
```  
  
## <a name="security"></a>Безопасность  
 Для подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] файлу журнала через инструментарий WMI необходимо иметь следующие разрешения на локальном и на удаленном компьютерах:  
  
-   Доступ на чтение к пространству имен WMI **Root\Microsoft\SqlServer\ComputerManagement10** . По умолчанию доступ для чтения задается для всех с помощью разрешения «Включить учетную запись».  
  
    > [!NOTE]  
    >  Сведения о том, как проверить разрешения WMI, см. в разделе "безопасность" раздела [Просмотр автономных файлов журнала](../../relational-databases/logs/view-offline-log-files.md).  
  
-   Разрешение для чтения на папку, содержащую журналы ошибок. По умолчанию журналы ошибок расположены по следующему пути (где \<*Drive> * обозначает диск, на котором установлен [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , а \<*InstanceName*> — имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ):  
  
     ** \<Drive> : \PROGRAM Files\Microsoft SQL Server\MSSQL11** **. \<InstanceName> \MSSQL\Log**  
  
 При соединении с использованием брандмауэра убедитесь, что в брандмауэре задано исключение для WMI на удаленных целевых компьютерах. Дополнительные сведения см. [в статье удаленное подключение к WMI, начиная с Windows Vista](/windows/win32/wmisdk/connecting-to-wmi-remotely-starting-with-vista).  
  
## <a name="see-also"></a>См. также:  
 [Класс SqlErrorLogEvent](../../relational-databases/wmi-provider-configuration-classes/sqlerrorlogevent-class.md)   
 [Просмотр автономных файлов журнала](../../relational-databases/logs/view-offline-log-files.md)  
  
