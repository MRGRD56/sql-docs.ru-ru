---
description: SET FIPS_FLAGGER (Transact-SQL)
title: SET FIPS_FLAGGER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FIPS_FLAGGER
- SET_FIPS_FLAGGER_TSQL
- FIPS_FLAGGER_TSQL
- SET FIPS_FLAGGER
dev_langs:
- TSQL
helpviewer_keywords:
- SET FIPS_FLAGGER statement
- FIPS 127-2 standard
- FIPS_FLAGGER option
ms.assetid: e82f6bee-6cf6-4061-be22-9ad2e8e9d3d6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bb266f6aadef4690df7ebb7f16a17e1c5ae2ca05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190387"
---
# <a name="set-fips_flagger-transact-sql"></a>SET FIPS_FLAGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Указывает режим проверки на соответствие стандарту FIPS 127-2. Основывается на стандарте ISO. Сведения о совместимости SQL Server FIPS см. в статье об [использовании SQL Server 2016 в режиме совместимости с FIPS 140-2](https://support.microsoft.com/help/4014354/how-to-use-sql-server-2016-in-fips-140-2-compliant-mode). 
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SET FIPS_FLAGGER ( 'level' |  OFF )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 **'** *level* **'**  
 Уровень соответствия стандарту FIPS 127-2 для проверки всех операций базы данных. При наличии конфликтов операции базы данных с выбранным уровнем стандартов ISO [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] формирует предупреждение.  
  
 *level* должен иметь одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|ENTRY|Проверка на соответствие начальному уровню стандарта ISO.|  
|FULL|Проверка на полное соответствие стандарту ISO.|  
|INTERMEDIATE|Проверка на соответствие промежуточному уровню стандарта ISO.|  
|OFF|Без проверки стандарта.|  
  
## <a name="remarks"></a>Комментарии  
 Значение параметра `SET FIPS_FLAGGER` устанавливается во время выполнения или запуска, а не во время синтаксического анализа. Проверка на этапе синтаксического анализа означает, что если инструкция SET присутствует в пакете или хранимой процедуре, то она вступает в силу независимо от того, достигает ли фактическое выполнение кода соответствующей точки. Кроме того, инструкция `SET` вступает в силу до выполнения любых операторов. Например, если инструкция `SET` находится в блоке `IF...ELSE`, который никогда не выполняется во время обработки, то она, тем не менее, `SET` вступает в силу, поскольку блок `IF...ELSE` подвергается синтаксическому анализу.  
  
 Если инструкция `SET FIPS_FLAGGER` установлена в хранимой процедуре, значение `SET FIPS_FLAGGER` восстанавливается после того, как управление выходит из этой хранимой процедуры. Поэтому инструкция `SET FIPS_FLAGGER`, определенная в динамическом коде SQL, не действует на инструкции, следующие за инструкцией динамического SQL.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="see-also"></a>См. также  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
