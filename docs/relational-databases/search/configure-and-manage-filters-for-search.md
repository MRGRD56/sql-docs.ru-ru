---
description: Настройка поисковых фильтров и управление ими
title: Настройка поисковых фильтров и управление ими | Документация Майкрософт
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], filters
- filters [full-text search]
ms.assetid: 7ccf2ee0-9854-4253-8cca-1faed43b7095
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3cb9d2938bddb44f22f4429daf0de0705ea53b65
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460127"
---
# <a name="configure-and-manage-filters-for-search"></a>Настройка поисковых фильтров и управление ими
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Индексирование документа в столбце с типом данных **varbinary**, **varbinary(max)**, **image** или **xml** требует дополнительной обработки. Такая обработка должна выполняться фильтром. Фильтр извлекает из документа текстовые данные (устранение форматирования). Затем фильтр отправляет текст в компонент средства разбиения по словам для языка, связанного со столбцом таблицы.  
 
## <a name="filters-and-document-types"></a>Фильтры и типы документов
Данный фильтр зависит от типа данных документа (DOC, PDF, XLS, XML и т. д.). Такие фильтры реализуют интерфейс IFilter. Для получения дополнительных сведений об этих типах документов выполните запрос к представлению каталога [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md) .  
  
Двоичные документы можно хранить в одном столбце **varbinary(max)** или **image** . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выбирает для каждого документа правильный фильтр в соответствии с расширением файла. Поскольку при сохранении файла в столбце типа **varbinary(max)** или **image** его расширение не отображается, расширение файла (DOC, DOCX, PDF и т. д.) нужно хранить в отдельном столбце таблицы, который называется столбцом типов. Столбец типов может иметь любой символьный тип данных и содержит расширение файла документа (например, DOC в случае документа [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word). В таблице **Document** базы данных [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)]столбец **Document** имеет тип **varbinary(max)**, а столбец **FileExtension**— тип **nvarchar(8)**.  

**Просмотр столбца типов в существующем полнотекстовом индексе**  
  
-   [sys.fulltext_index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)  
  
> [!NOTE]  
>  Фильтр может быть способен обрабатывать объекты, внедренные в родительский объект, в зависимости от его реализации. Однако в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] фильтры не настроены на переход по ссылкам на другие объекты.  

## <a name="installed-filters"></a>Установленные фильтры 
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] устанавливаются собственные фильтры XML и HTML. Кроме того, любые фильтры для собственных форматов [!INCLUDE[msCoName](../../includes/msconame-md.md)] (DOC, XDOC, PPT и т. д.), которые уже установлены в операционной системе, также загружаются средствами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Чтобы определить, какие фильтры загружены в экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]в данный момент, используйте хранимую процедуру [sp_help_fulltext_system_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md) следующим образом:  

```sql
EXEC sp_help_fulltext_system_components 'filter';   
```  

> [!NOTE]
> Даже при использовании последней версии пакета фильтра Office, включающей поддержку файлов XLSX, SQL Server не поддерживает электронные таблицы в строгом формате Open XML.  Ошибка не возвращается. SQL Server просто не удается проиндексировать содержимое электронных таблиц в строгом формате Open XML.

## <a name="non-microsoft-filters"></a>Фильтры сторонних разработчиков
Прежде чем использовать фильтры для форматов, не принадлежащих [!INCLUDE[msCoName](../../includes/msconame-md.md)], их необходимо вручную загрузить в экземпляр сервера. Сведения об установке дополнительных фильтров см. в статье [Просмотр или изменение зарегистрированных фильтров и разделителей слов](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md).  
  
  
## <a name="see-also"></a>См. также  
 [sys.fulltext_index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)   
 [Совместимость FILESTREAM с другими компонентами SQL Server](../../relational-databases/blob/filestream-compatibility-with-other-sql-server-features.md)  
  
  
