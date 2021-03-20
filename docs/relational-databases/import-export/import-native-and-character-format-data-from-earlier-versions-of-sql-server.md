---
title: Импорт данных в собственном и символьном формате из предыдущих версий SQL Server
description: В SQL Server 2019 можно использовать bcp для импорта данных в собственном и символьном формате из других версий SQL Server с помощью параметра -V с квалификатором.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- earlier versions [SQL Server], import and export data formats
- -V switch
- data formats [SQL Server], earlier versions
- previous versions [SQL Server], import and export data formats
ms.assetid: e644696f-9017-428e-a5b3-d445d1c630b3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 18bd8c7b114329f5acacacd2c2230bf75ecfb6b5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750204"
---
# <a name="import-native-and-character-format-data-from-earlier-versions-of-sql-server"></a>Импорт данных в собственном и символьном формате из предыдущих версий SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

В [!INCLUDE [sssql14-md](../../includes/sssql14-md.md)] и более поздних версиях можно с помощью программы **bcp** импортировать данные в собственном и символьном формате из [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] или [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], задав ключ **-V**. Ключ **-V** указывает [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , что следует использовать типы данных из указанной более ранней версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]и что формат файла данных аналогичен формату в предыдущей версии.  
  
Чтобы задать более раннюю версию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для файла данных, используйте параметр **-V** с одним из следующих квалификаторов:  
  
|Версия SQL Server|Квалификатор|  
|------------------------|---------------|  
|[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]|**-V80**|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|**-V90**|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|**-V100**|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|**-V 110**|  
  
## <a name="interpretation-of-data-types"></a>Интерпретация типов данных  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и более поздних версиях была добавлена поддержка некоторых новых типов данных. Если нужно импортировать новый тип данных в более раннюю версию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , эти данные необходимо сохранить в формате, который могут прочитать старые клиенты **bcp** . Следующая таблица содержит сведения о том, как новые типы данных преобразуются для совместимости с более ранними версиями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Новые типы данных в SQL Server 2005|Совместимые типы данных в версии 6 *x*|Совместимые типы данных в версии 70|Совместимые типы данных в версии 80|  
|---------------------------------------|-------------------------------------------|-----------------------------------------|-----------------------------------------|  
|**bigint**|**decimal**|**decimal**|*|  
|**sql_variant**|**text**|**nvarchar(4000)**|*|  
|**varchar(max)**|**text**|**text**|**text**|  
|**nvarchar(max)**|**ntext**|**ntext**|**ntext**|  
|**varbinary(max)**|**image**|**image**|**image**|  
|XML|**ntext**|**ntext**|**ntext**|  
|UDT**|**image**|**image**|**image**|  
  
 *Этот тип поддерживается изначально.  
  
 **UDT обозначает определяемый пользователем тип.  
  
## <a name="exporting-using--v-80"></a>Экспорт с ключом -V 80  
 При массовом экспорте данных ключом **-V80** данные типов **nvarchar(max)** , **varchar(max)** , **varbinary(max)** , XML и определяемый пользователем тип в собственном режиме хранятся с 4-байтовым префиксом, как данные **text**, **image** и **ntext**, вместо 8-байтового префикса, используемого по умолчанию в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и более поздних версиях.  
  
## <a name="copying-date-values"></a>Копирование значений данных  
 Программа **bcp** использует API-интерфейс массового копирования ODBC. Таким образом, для импорта значений дат в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]программа **bcp** использует формат данных ODBC (*гггг-мм-дд чч:мм:сс*[ *.f...* ]).  
  
 Команда **bcp** экспортирует файлы данных в символьном формате с помощью формата ODBC по умолчанию для значений **datetime** и **smalldatetime** . Например, столбец типа **datetime** , содержащий дату `12 Aug 1998` , копируется с помощью массового копирования в файл данных в качестве строки символов `1998-08-12 00:00:00.000`.  
  
> [!IMPORTANT]  
>  При импорте данных в поле **smalldatetime** с помощью программы **bcp** нужно убедиться, что значение секунд равно 00,000; в противном случае во время операции возникнет ошибка. Тип данных **smalldatetime** содержит только значения с точностью до минуты. В данном случае инструкции BULK INSERT и INSERT ... SELECT * FROM OPENROWSET(BULK...) не приведут к ошибке, но усекут значение секунд.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Использование форматов данных для массового импорта или экспорта**  
  
-   [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Обратная совместимость компонента ядра СУБД SQL Server](../../database-engine/discontinued-database-engine-functionality-in-sql-server.md)   
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
