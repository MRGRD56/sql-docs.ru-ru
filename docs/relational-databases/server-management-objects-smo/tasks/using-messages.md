---
description: Использование сообщений
title: Использование сообщений | Документация Майкрософт
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- messages [SMO]
ms.assetid: 4037a866-4826-4c1f-890c-e7e3658adf13
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b010efd758d7d7c84a082527bc4793bcac323d46
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482985"
---
# <a name="using-messages"></a>Использование сообщений
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  В объектах SMO системные сообщения представлены <xref:Microsoft.SqlServer.Management.Smo.SystemMessageCollection> объектом, принадлежащим объекту **сервера** . Так как системные сообщения нельзя изменить, свойства объекта **SystemMessage** доступны только для чтения.  
  
 Определяемые пользователем сообщения представлены в SMO программно объектом <xref:Microsoft.SqlServer.Management.Smo.UserDefinedMessageCollection>. Существующие определяемые пользователем сообщения могут быть обнаружены при проходе по коллекции. Новые определяемые пользователем сообщения могут создаваться путем создания нового объекта **UserDefinedMessage** и установки соответствующих свойств.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах кода для создания приложения необходимо выбрать среду программирования, шаблон программирования и язык программирования. Дополнительные сведения см. [в статье Создание проекта Visual C&#35; SMO в Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="finding-a-particular-system-message-in-visual-basic"></a>Обнаружение определенного системного сообщения на языке Visual Basic  
 Пример кода показывает, как определить системное сообщение по идентификатору и отобразить его.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference an existing system message using the ItemByIdAndLanguage method.
Dim msg As SystemMessage
msg = srv.SystemMessages.ItemByIdAndLanguage(14126, "us_english")
'Display the message ID and  text.
Console.WriteLine(msg.ID.ToString + " " + msg.Text)
```
  
## <a name="finding-a-particular-system-message-in-visual-c"></a>Обнаружение определенного системного сообщения на языке Visual C#  
 Пример кода показывает, как определить системное сообщение по идентификатору и отобразить его.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            Server srv = new Server();  
            //Reference an existing system message using the   
            //ItemByIdAndLanguage method.   
            SystemMessage msg = default(SystemMessage);  
            msg = srv.SystemMessages.ItemByIdAndLanguage(14126, "us_english");  
            //Display the message ID and text.   
            Console.WriteLine(msg.ID.ToString() + " " + msg.Text);  
        }  
```  
  
## <a name="finding-a-particular-system-message-in-powershell"></a>Обнаружение определенного системного сообщения в PowerShell  
 Пример кода показывает, как определить системное сообщение по идентификатору и отобразить его.  
  
```powershell  
# Set the path context to the local, default instance of SQL Server.  
CD \sql\localhost\  
$srv = get-item default  
  
#Get the message 14126 in US English and display it  
$msg = $srv.SystemMessages.ItemByIdAndLanguage(14126, "us_english")  
$msg.ID.ToString() + " "+ $msg.Text  
```  
  
## <a name="adding-a-new-user-defined-message-in-visual-basic"></a>Добавление нового определяемого пользователем сообщения на языке Visual Basic  
 Пример кода демонстрирует, как создать определяемое пользователем сообщение с идентификатором больше 50 000.  
  
```VBNET  
Dim mysrv As Server  
mysrv = New Server  
Dim udm As UserDefinedMessage  
udm = New UserDefinedMessage(mysrv, 50003, "us_english", 16, "Test message")  
udm.Create()  
```  
  
## <a name="adding-a-new-user-defined-message-in-visual-c"></a>Добавление нового определяемого пользователем сообщения на языке Visual C#  
 Пример кода демонстрирует, как создать определяемое пользователем сообщение с идентификатором больше 50 000.  
  
```csharp  
{  
  
            Server mysrv = new Server();  
  
            UserDefinedMessage udm = new UserDefinedMessage(mysrv, 50030, "us_english",16, "Test message");  
            udm.Create();  
             UserDefinedMessage  msg = mysrv.UserDefinedMessages.ItemByIdAndLanguage(50030, "us_english");  
            //Display the message ID and text.   
            Console.WriteLine(msg.ID.ToString() + " " + msg.Text);  
  
        }  
```  
  
## <a name="adding-a-new-user-defined-message-in-powershell"></a>Добавление нового, определяемого пользователем сообщения в PowerShell  
 Пример кода демонстрирует, как создать определяемое пользователем сообщение с идентификатором больше 50 000.  
  
```powershell  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#Create a new message  
  
$udm = New-Object -TypeName Microsoft.SqlServer.Management.SMO.UserDefinedMessage -argumentlist `  
$srv, 50030, "us_english", 16, "Test message"  
$udm.Create()  
$msg = $srv.UserDefinedMessages.ItemByIdAndLanguage(50030, "us_english");  
$msg  
```  
  
  
