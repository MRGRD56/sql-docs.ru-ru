---
description: DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL)
title: DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.technology: data-warehouse
ms.prod_service: synapse-analytics, pdw
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: pmasl
ms.author: umajay
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 29b7af17720e83fe6ceb338b1fe9370e754080b8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754534"
---
# <a name="dbcc-pdw_showexecutionplan-transact-sql"></a>DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Отображает план выполнения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для запроса, выполняющегося в определенном вычислительном либо управляющем узле [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Позволяет устранять проблемы с производительностью запросов, выполняющихся в вычислительных узлах или управляющем узле.
  
После определения проблем с производительностью запросов SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполняющихся в вычислительных узлах, повысить производительность можно несколькими способами. Возможные способы оптимизации производительности запросов в вычислительных узлах включают в себя создание статистики по нескольким столбцам, создание некластеризованных индексов или использование указаний запросов.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
Синтаксис для [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]:

```syntaxsql
DBCC PDW_SHOWEXECUTIONPLAN ( distribution_id, spid )  
[ ; ]  
```  

Синтаксис для [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:
  
```syntaxsql
DBCC PDW_SHOWEXECUTIONPLAN ( pdw_node_id, spid )  
[ ; ]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Аргументы  
 *distribution_id*  
 Идентификатор распределения, в котором выполняется план запроса. Это целое число; не может быть равно NULL. Значение должно находиться в диапазоне от 1 до 60. Используется при разработке для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
 *pdw_node_id*  
 Идентификатор узла, в котором выполняется план запроса. Это целое число; не может быть равно NULL. Используется для устройства.  
  
 *spid*  
 Идентификатор сеанса [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], в котором выполняется план запроса. Это целое число; не может быть равно NULL.  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение CONTROL для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
Требуется разрешение VIEW-SERVER-STATE для устройства.
  
## <a name="examples-sssdw"></a>Примеры: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]  
  
### <a name="a-dbcc-pdw_showexecutionplan-basic-syntax"></a>A. Базовый синтаксис DBCC PDW_SHOWEXECUTIONPLAN  
 При выполнении в экземпляре [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] измените приведенный выше запрос так, чтобы также выбирался идентификатор distribution_id.  
  
```sql
SELECT [sql_spid], [pdw_node_id], [request_id], [dms_step_index], [type], [start_time], [end_time], [status], [distribution_id]  
FROM sys.dm_pdw_dms_workers   
WHERE [status] <> 'StepComplete' and [status] <> 'StepError'  
order by request_id, [dms_step_index];  
```  
  
В результате будет возвращаться идентификатор SPID для каждого активного распределения. Чтобы узнать, что выполнялось в рамках распределения 1 в сеансе 375, следует выполнить приведенную ниже команду.
  
```sql
DBCC PDW_SHOWEXECUTIONPLAN ( 1, 375 );  
```  

## <a name="examples-sspdw"></a>Примеры: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="b-dbcc-pdw_showexecutionplan-basic-syntax"></a>Б. Базовый синтаксис DBCC PDW_SHOWEXECUTIONPLAN  
 Если запрос выполняется слишком долго, значит, он производит операцию плана запроса DMS или операцию плана запроса SQL.  
  
Если запрос выполняет операцию плана запроса DMS, с помощью приведенного ниже запроса можно получить список идентификаторов узлов и сеансов для незавершенных этапов.
  
```sql
SELECT [sql_spid], [pdw_node_id], [request_id], [dms_step_index], [type], [start_time], [end_time], [status]   
FROM sys.dm_pdw_dms_workers   
WHERE [status] <> 'StepComplete' and [status] <> 'StepError'  
AND pdw_node_id = 201001   
ORDER BY request_id, [dms_step_index], [distribution_id];  
```  
  
Полученные в результате предыдущего запроса значения sql_spid и pdw_node_id можно использовать как параметры инструкции DBCC PDW_SHOWEXEUCTIONPLAN. Например, приведенная ниже команда отображает план выполнения для pdw_node_id 201001 и sql_spid 375.
  
```sql
DBCC PDW_SHOWEXECUTIONPLAN ( 201001, 375 );  
```  

## <a name="see-also"></a>См. также раздел

- [DBCC PDW_SHOWPARTITIONSTATS (Transact-SQL)](dbcc-pdw-showpartitionstats-transact-sql.md)  
- [DBCC PDW_SHOWSPACEUSED (Transact-SQL)](dbcc-pdw-showspaceused-transact-sql.md)
