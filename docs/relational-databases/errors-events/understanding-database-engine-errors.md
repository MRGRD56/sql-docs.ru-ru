---
title: Основные сведения об ошибках ядра СУБД | Документация Майкрософт
description: Сведения об атрибутах ошибок, вызванных ядром СУБД SQL Server, а также о том, как получить доступ ко всем системным и пользовательским сообщениям об ошибках из sys.messages.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- errors [SQL Server], about errors
- errors [SQL Server], Database Engine
- errors [SQL Server]
- Database Engine [SQL Server], errors
ms.assetid: ddaca9d3-956f-46a5-8cd3-a7a15ec75878
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c836216ffa3acd6bdb24c660114980c1f3d2c259
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737704"
---
# <a name="understanding-database-engine-errors"></a>Основные сведения об ошибках компонента Database Engine
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Ошибки, возникшие в компоненте [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], имеют атрибуты, описанные в следующей таблице.  
  
|attribute|Описание|  
|---------------|-----------------|  
|Номер ошибки|Каждое сообщение имеет уникальный номер ошибки.|  
|Строка сообщения об ошибке|Сообщение об ошибке содержит диагностические сведения о причине ошибки. Многие сообщения об ошибках имеют подстановочные переменные, в которые заносятся сведения, например имя объекта, вызвавшего ошибку.|  
|Severity|Степень серьезности ошибки указывает, насколько она значительна. Ошибки с низкой степенью серьезности, например 1 или 2, являются информационными сообщениями или предупреждениями низкого уровня. Ошибки с высокой степенью серьезности указывают на проблемы, которые должны быть решены как можно быстрее. Дополнительные сведения о степенях серьезности см. в разделе [Степени серьезности ошибок компонента Database Engine](../../relational-databases/errors-events/database-engine-error-severities.md).|  
|Состояние|Некоторые сообщения об ошибках могут возникнуть в нескольких точках кода компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Например, ошибка 1105 может возникнуть при различных условиях. Каждое условие, которое вызывает ошибку, присваивает уникальный код состояния.<br /><br /> При просмотре баз данных со сведениями об известных неполадках, таких как база знаний [!INCLUDE[msCoName](../../includes/msconame-md.md)] , можно использовать номер состояния, чтобы определить, является ли записанная неполадка возникшей ошибкой. Например, если статья базы знаний описывает ошибку 1105 с состоянием 2, а получена ошибка 1105 с состоянием 3, ошибка, вероятно, возникла не по той причине, которая описана в статье.<br /><br /> Инженер поддержки [!INCLUDE[msCoName](../../includes/msconame-md.md)] также может использовать код состояния, содержащийся в сообщении об ошибке, чтобы найти место в исходном коде, где эта ошибка возникла. Эти данные могут предоставить дополнительные сведения для диагностики проблемы.|  
|Имя процедуры|Имя хранимой процедуры или триггера, в которых произошла ошибка.|  
|Номер строки|Указывает на инструкцию в пакете, хранимой процедуре, триггере или функции, которая сформировала ошибку.|  
  
 Все системные и пользовательские сообщения об ошибках в экземпляре компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] содержатся в представлении каталога **sys.messages** . Инструкцию RAISERROR можно использовать для возвращения пользовательских ошибок приложению.  
  
 Все API-интерфейсы базы данных, например пространство имен [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] **SQLClient**, ActiveX Data Objects (ADO), OLE DB и Open Database Connectivity (ODBC), сообщают основные атрибуты ошибки. Эти сведения включают номер ошибки и строку сообщения. Однако не все API-интерфейсы сообщают остальные атрибуты ошибки.  
  
 Сведения об ошибке, которая появляется внутри области TRY конструкции TRY…CATCH, могут быть получены в коде [!INCLUDE[tsql](../../includes/tsql-md.md)] при использовании таких функций, как ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY и ERROR_STATE, внутри области соответствующего блока CATCH. Дополнительные сведения см. в разделе [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
 Следующий пример обращается с запросом к представлению каталога `sys.messages` , возвращающим список всех системных и пользовательских сообщений об ошибках в компоненте [!INCLUDE[ssDE](../../includes/ssde-md.md)] , содержащих английский текст (`1033`).  
  
```  
SELECT  
    message_id,  
    language_id,  
    severity,  
    is_event_logged,  
    text  
  FROM sys.messages  
  WHERE language_id = 1033;  
```  
  
 Дополнительные сведения см. в разделе [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md).  
  
## <a name="see-also"></a>См. также:  
 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR (Transact-SQL)](../../t-sql/functions/error-transact-sql.md)   
 [TRY...CATCH (Transact-SQL)](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE (Transact-SQL)](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE (Transact-SQL)](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER (Transact-SQL)](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE (Transact-SQL)](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY (Transact-SQL)](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE (Transact-SQL)](../../t-sql/functions/error-state-transact-sql.md)  
  
  
