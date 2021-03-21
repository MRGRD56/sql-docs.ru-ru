---
description: SET TEXTSIZE (Transact-SQL)
title: SET TEXTSIZE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TEXTSIZE_TSQL
- TEXTSIZE
- SET_TEXTSIZE_TSQL
- SET TEXTSIZE
dev_langs:
- TSQL
helpviewer_keywords:
- SET TEXTSIZE statement
- SELECT statement [SQL Server], text size returned
- size [SQL Server], text and image data
- TEXTSIZE option
- text size returned [SQL Server]
ms.assetid: 787154a6-39a6-4dd6-a6d0-67b4364f95d5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d5f38e160faa074d2e710c1c044a0597a3ed91d9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755594"
---
# <a name="set-textsize-transact-sql"></a>SET TEXTSIZE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Указывает размер (в байтах) данных типа **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **text**, **ntext** и **image**, возвращаемых клиенту инструкцией SELECT.  
  
> [!IMPORTANT]
>  Типы данных **ntext**, **text** и **image** будут исключены в следующей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Следует избегать использования этих типов данных при новой разработке и запланировать изменение приложений, использующих их в настоящий момент. Вместо них следует использовать типы данных **nvarchar(max)**, **varchar(max)** и **varbinary(max)** .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SET TEXTSIZE { number }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *number*  
 Размер данных типа **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **text**, **ntext** или **image** в байтах. *number* должно быть целым числом не более 2 147 483 647 (2 ГБ).  Значение –1 означает неограниченный размер. Значение 0 устанавливает размер в 4 КБ, принятый по умолчанию.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client (10.0 и выше ) и драйвер ODBC для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически указывают `-1` (неограниченно) при подключении.  
  
 **Драйверы до [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008:**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для Native Client и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик OLE DB для Native Client (версия 9) для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически устанавливают для параметра TEXTSIZE значение 2147483647 при подключении.  
  
## <a name="remarks"></a>Примечания  
 Установка SET TEXTSIZE влияет на работу функции @@TEXTSIZE.  
  
 Значение TEXTSIZE устанавливается во время выполнения, а не во время синтаксического анализа.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="see-also"></a>См. также:  
 [@@TEXTSIZE (Transact-SQL)](../../t-sql/functions/textsize-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
