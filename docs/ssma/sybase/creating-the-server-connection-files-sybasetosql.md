---
description: Создание файлов подключения к серверу (SybaseToSQL)
title: Создание файлов подключения к серверу (SybaseToSQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Creating Server Connection Files
- Sybase Console,Server Connection File Validation
ms.assetid: 35ef396f-9f98-429d-9fc5-4f413d08fb37
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 41fa50fba0f6d1398788246422a84b825594a5f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056669"
---
# <a name="creating-the-server-connection-files-sybasetosql"></a>Создание файлов подключения к серверу (SybaseToSQL)
Сведения о сервере можно указать либо в разделе Servers файла сценария, либо в отдельном файле соединения сервера. Параметр командной строки для файла соединения сервера имеет значение, `-c <serverconnectionfile>` . Если один и тот же идентификатор сервера имеется как в файле скрипта, так и в файле соединения сервера, то будет считаться определение сервера в файле скрипта.  
  
**Пример**.  
  
```  
1.<!--Sample of server connection file commands -->  
  
<sybase name="<source-server-unique-name>">  
  
  <standard-mode>  
  
    <provider value="Ole DB Provider"/>  
  
    <server-name value="<server-name>"/>  
  
    <server-port value="<port>"/>  
  
    <user-id value="<password>"/>  
  
  </standard-mode>  
  
</sybase>  
```  
  
```xml  
<!--Connection to SQL Server-->  
  
<sql-server name="<target-server-unique-name>">  
  
  <sql-server-authentication>  
  
    <database value="<database-name>"/>  
  
    <server value="<server-name>"/>  
  
    <user-id value="<user-name>"/>  
  
    <password value="<password>"/>  
  
    <encrypt value="<true/false>"/>  
  
    <trust-server-certificate value="<true/false>"/>  
  
  </sql-server-authentication >  
  
</sql-server>  
```  
  
```  
2.<!-Sample of server connection file commands-->  
<sybase name="<source-server-unique-name>">  
  
  <advanced-mode>  
  
    <connection-string value="User ID=<user-name>;Password=<password>;Provider=ASEOLEDB.1;Server=<server-name>;Port=<port>;OLE DB Services = -2;"/>  
  
  </advanced-mode >  
  
</sybase>  
```  
  
```xml  
<!--Connection to SQL Server-->  
  
<sql-server name="<target-server-unique-name>">  
  
  <sql-server-authentication >  
  
    <database value="<database-name>"/>  
  
    <server value="<server-name>"/>  
  
    <user-id value="<user-name>"/>  
  
    <password value="<password>"/>  
  
  </sql-server-authentication >  
  
</sql-server>  
```  
  
## <a name="server-connection-file-validation"></a>Проверка файлов подключения к серверу  
Пользователь может легко проверить файл подключения своего сервера относительно файла определения схемы **S2SSConsoleScriptServersSchema. xsd** , доступного в папке Schemas.  
  
## <a name="next-step"></a>Следующий шаг  
Следующий шаг в работе консоли заключается [в выполнении консоли SSMA &#40;SybaseToSQL&#41;](../../ssma/sybase/executing-the-ssma-console-sybasetosql.md)  
  
## <a name="see-also"></a>См. также:  
[Выполнение команд консоли SSMA](executing-the-ssma-console-sybasetosql.md)  
  
