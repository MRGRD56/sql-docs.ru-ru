---
title: Олицетворение и учетные данные для подключений | Документация Майкрософт
description: В SQL Server интеграции со средой CLR может потребоваться олицетворение вызывающего объекта в проверке подлинности Windows с помощью свойства SqlContext. WindowsIdentity.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- impersonation [CLR integration]
- security [CLR integration]
- database objects [CLR integration], connections
- connections [CLR integration]
- authentication [CLR integration]
- user impersonation [CLR integration]
- credentials [CLR integration]
- database objects [CLR integration], security
ms.assetid: 293dce7d-1db2-4657-992f-8c583d6e9ebb
author: rothja
ms.author: jroth
ms.openlocfilehash: a6487b61d9c21ee86acad28413fb8a0439731b33
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91810849"
---
# <a name="impersonation-and-credentials-for-connections"></a>Олицетворение и учетные данные для соединений
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В условиях интеграции [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] со средой CLR использовать проверку подлинности Windows сложнее, чем проверку подлинности SQL Server, но более безопасно. При использовании проверки подлинности Windows имейте ввиду следующие замечания.  
  
 По умолчанию процесс SQL Server, который подключается к Windows, приобретает контекст безопасности учетной записи службы Windows SQL Server. Однако возможно также сопоставить функцию CLR с удостоверением-посредником, чтобы у исходящих соединений был контекст безопасности, отличный от учетной записи службы Windows.  
  
 В некоторых случаях может потребоваться олицетворение вызывающего объекта с помощью свойства **SqlContext. WindowsIdentity** вместо запуска в качестве учетной записи службы. Экземпляр **WindowsIdentity** представляет идентификатор клиента, вызвавшего вызывающий код, и доступен только тогда, когда клиент использовал проверку подлинности Windows. После получения экземпляра **WindowsIdentity** можно вызвать метод **impersonate** , чтобы изменить маркер безопасности потока, а затем открыть подключения ADO.NET от имени клиента.  
  
 После вызова SQLContext. WindowsIdentity. IMPERSONATE вы не сможете получить доступ к локальным данным, и вы не сможете получить доступ к системным данным. Чтобы снова получить доступ к данным, необходимо вызвать метод WindowsImpersonationContext. Undo.  
  
 В следующем примере показано олицетворение вызывающего объекта с помощью свойства **SqlContext. WindowsIdentity** .  
  
 Visual C#  
  
```  
WindowsIdentity clientId = null;  
WindowsImpersonationContext impersonatedUser = null;  
  
clientId = SqlContext.WindowsIdentity;  
  
// This outer try block is used to protect from   
// exception filter attacks which would prevent  
// the inner finally block from executing and   
// resetting the impersonation.  
try  
{  
   try  
   {  
      impersonatedUser = clientId.Impersonate();  
      if (impersonatedUser != null)  
         return GetFileDetails(directoryPath);  
         else return null;  
   }  
   finally  
   {  
      if (impersonatedUser != null)  
         impersonatedUser.Undo();  
   }  
}  
catch  
{  
   throw;  
}  
```  
  
> [!NOTE]  
>  Сведения об изменениях в работе олицетворения см. в разделе [критические изменения в функциях ядро СУБД в SQL Server 2016](../../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md).  
  
 Более того, если был получен экземпляр идентификатора [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows, то по умолчанию нельзя перенести этот экземпляр на другой компьютер; по умолчанию инфраструктура безопасности Windows не позволяет делать этого. Однако существует механизм под названием «делегирование», который позволяет распространять идентификаторы Windows на несколько доверенных компьютеров. Дополнительные сведения о делегировании см. в статье TechNet "[Переход по протоколу Kerberos и ограниченное делегирование](/previous-versions/windows/it-pro/windows-server-2003/cc739587(v=ws.10))".  
  
## <a name="see-also"></a>См. также:  
 [Объект SqlContext](../../../relational-databases/clr-integration-data-access-in-process-ado-net/sqlcontext-object.md)  
  
