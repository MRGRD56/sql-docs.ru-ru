---
description: sys.sysfiles (Transact-SQL)
title: Файлы sys.sys(Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysfiles
- sys.sysfiles_TSQL
- sys.sysfiles
- sysfiles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysfiles system table
- sys.sysfiles compatibility view
ms.assetid: 3b47f38d-1cff-404d-89d3-9342c451c802
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34ec2c47be129384cca6be84894251a74cb996f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201374"
---
# <a name="syssysfiles-transact-sql"></a>sys.sysfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждого файла базы данных.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**ИД**|**smallint**|Идентификационный номер файла, уникальный для каждой базы данных.|  
|**Идентификатор**|**smallint**|Идентификационный номер файловой группы.|  
|**size**|**int**|Размер файла в страницах по 8 КБ.|  
|**MAXSIZE**|**int**|Максимальный размер файла, в страницах по 8 КБ.<br /><br /> 0 = не возрастает.<br /><br /> -1 = размер файла может увеличиваться до полного заполнения диска.<br /><br /> 268435456 = файл журнала может увеличиваться до 2 ТБ.<br /><br /> Примечание. базы данных, которые были обновлены с неограниченным размером файла журнала, будут сообщать-1 о максимальном размере файла журнала.|  
|**квот**|**int**|Предельный размер базы данных. Может быть либо числом страниц, либо процентом размера файла в зависимости от значения **Status**.<br /><br /> 0 = не возрастает.|  
|**status**|**int**|Биты состояния для значения **роста** в мегабайтах (МБ) или килобайтах (КБ).<br /><br /> 0x2 = дисковый файл.<br /><br /> 0x40 = файл журнала.<br /><br /> 0x100000 = масштаб увеличения базы данных. Это значение определяет увеличение в процентах, а не в количестве страниц.|  
|**Счетчик**|**int**|Зарезервировано.|  
|**name**|**sysname**|Логическое имя файла.|  
|**filename**|**nvarchar(260)**|Имя физического устройства. Включает полный путь к файлу.|  
  
## <a name="see-also"></a>См. также:  
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
