---
title: ISSCommandWithParameters (драйвер OLE DB)
description: Узнайте о поддержке XML для SQL Server и определяемых пользователем типов интерфейсом ISSCommandWithParameters в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSCommandWithParameters interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 455387587cc73c3b86b9f94ac84920ed27e127c8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753154"
---
# <a name="isscommandwithparameters-ole-db"></a>Интерфейс ISSCommandWithParameters (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Интерфейс **ISSCommandWithParameters** обеспечивает поддержку [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] XML и пользовательских типов. Этот дополнительный интерфейс наследует основной интерфейс OLE DB **ICommandWithParameters**. Помимо трех методов, наследуемых от интерфейса **ICommandWithParameters** (**GetParameterInfo**, **MapParameterNames** и **SetParameterInfo**), интерфейс **ISSCommandWithParameters** содержит два новых метода, которые используются для обработки серверных типов данных.  
  
> [!NOTE]  
>  Интерфейс **ISSCommandWithParameters** может использоваться при применении компонентов службы, однако сами компоненты службы этот интерфейс не используют.  
  
|Метод|Description|  
|------------|-----------------|  
|[ISSCommandWithParameters::GetParameterProperties (OLE DB)](../../oledb/ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md)|Возвращает одну структуру набора свойств **SSPARAMPROPS** в массиве для каждого определяемого пользователем типа данных или XML-параметра, переданного команде, однако для других типов параметров не возвращается ничего.|  
|[ISSCommandWithParameters::SetParameterProperties (OLE DB)](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md)|Задает свойства параметров для каждого параметра в отдельности по порядковому номеру либо задает свойства всех параметров сразу путем указания массива структур **SSPARAMPROPS** .|  
  
## <a name="see-also"></a>См. также:  
 [Интерфейсы (OLE DB)](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)    
 [Использование типов данных XML](../../oledb/features/using-xml-data-types.md)   
 [Использование определяемых пользователем типов](../../oledb/features/using-user-defined-types.md)  
  
  
