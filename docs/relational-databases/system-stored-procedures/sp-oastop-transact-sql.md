---
description: sp_OAStop (Transact-SQL)
title: sp_OAStop (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAStop
- sp_OAStop_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAStop
ms.assetid: aa9eab66-c4f7-4ec7-9f0d-5d24d16da654
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 71886c5eec626891fd860c0a75d331033b2dd32a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196055"
---
# <a name="sp_oastop-transact-sql"></a>sp_OAStop (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Останавливает серверную среду выполнения хранимых процедур OLE-автоматизации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_OAStop      
```  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или ненулевое число (неуспешное завершение), которое является целочисленным значением типа HRESULT, возвращаемого объектом OLE-автоматизации.  
  
 Дополнительные сведения о кодах возврата HRESULT см. в разделе [коды возврата OLE Automation и сведения об ошибке](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="remarks"></a>Замечания  
 Единая среда выполнения совместно используется всеми клиентами хранимых процедур OLE-автоматизации. Если один клиент вызывает **sp_OAStop** общая среда выполнения будет остановлена для всех клиентов. После остановки среды выполнения любой вызов **sp_OACreate** перезапускает среду выполнения.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в предопределенной роли сервера **sysadmin** или разрешение EXECUTE непосредственно в этой хранимой процедуре. `Ole Automation Procedures` для использования любой системной процедуры, связанной с OLE Automation, необходимо **включить** конфигурацию.  
  
## <a name="examples"></a>Примеры  
 Следующий пример продемонстрирует остановку общей среды выполнения OLE-автоматизации.  
  
```  
EXEC sp_OAStop;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры OLE-автоматизации &#40;&#41;Transact — SQL ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Пример скрипта OLE-автоматизации](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
