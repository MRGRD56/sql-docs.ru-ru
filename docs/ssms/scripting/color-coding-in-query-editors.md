---
title: Выделение цветом в редакторах запросов
description: Узнайте, как категории текста кодируются цветом, чтобы упростить поиск определенного текста, а также как можно настроить пользовательскую цветовую схему.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], color coding
- color coding [Query Editor]
ms.assetid: 802882dc-c997-4e3f-8a01-994bb43169ae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96fe150cd54cfd37074949a278ed40570dd18e83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354754"
---
# <a name="color-coding-in-query-editors"></a>Выделение цветом в редакторах запросов

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Тексту, который вводится в редакторах кода, присваивается категория. Каждая категория идентифицируется по цвету. Цвета помогают быстро находить текст в коде. Например, комментарии выделены темно-зеленым. В следующей таблице приведены наиболее часто применяемые цвета. Чтобы просмотреть полный список цветов и их категорий, а также настроить пользовательскую цветовую схему, в меню **Сервис** выберите пункт **Параметры** . Дополнительные сведения об изменении цветов по умолчанию см. в разделе [Change Font Color, Size, and Style](./change-font-color-size-and-style.md).  
  
## <a name="default-code-colors"></a>Цвета по умолчанию  
  
|Color|Категория|  
|-----------|--------------|  
|Красный|Строка SQL|  
|Темно-зеленый|Комментарий|  
|Черный на серебристом фоне|Инструкция SQLCMD|  
|Пурпурный|Системная функция|  
|Зеленый|Системная таблица, представление или функция с табличным значением. А также системные схемы sys и INFORMATION_SCHEMA.|  
|Синий|Ключевое слово|  
|Бирюзовый|Номера строк или параметр шаблона|  
|Каштановый|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] хранимая процедура|  
|Темно-серый|Операторы|  
  
## <a name="status-bar"></a>Строка состояния  
 Можно назначить зарегистрированным серверам или серверам компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] в обозревателе объектов различные цвета в строке состояния редактора запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Это поможет идентифицировать, к какому серверу подключено каждое из окон редактора, когда одновременно открыто много окон. Дополнительные сведения о настройке цветов строки состояния см. в статье [Строка состояния (редактор запросов компонента Database Engine)](./status-bar-database-engine-query-editor.md).  
  
 В некоторых редакторах строка состояния не отображается, либо не поддерживается несколько цветов.  
  
