---
title: Изучение интерфейса SSMA для Oracle
description: Узнайте о Помощник по миграции SQL Server (SSMA) для процесса установки Oracle и ознакомьтесь с пользовательским интерфейсом SSMA.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- SSMA for Oracle, Metadata Explorers
- SSMA for Oracle, Toolbars
ms.assetid: df79664c-972e-4bef-865a-ce609789fee7
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 87e4f35fbb9b7a8979a501260e180a63ec31bddc
ms.sourcegitcommit: 3bb5ea67dc0d369b921f1bee4ffd4317aba2253c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2021
ms.locfileid: "107720453"
---
# <a name="explore-ssma-for-oracle-interface"></a>Изучение интерфейса SSMA для Oracle
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Помощник по миграции (SSMA) для Oracle позволяет быстро преобразовывать схемы баз данных Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] схемы, передавать полученные схемы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и переносить данные из Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
В этом разделе описывается процесс установки, а затем мы познакомимся с пользовательским интерфейсом SSMA.  
  
## <a name="installing-ssma"></a>Установка SSMA  
Чтобы использовать SSMA, необходимо сначала установить клиентскую программу SSMA на компьютере, который может обращаться как к исходной базе данных Oracle, так и к целевому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Затем необходимо установить пакет расширения и по крайней мере один поставщик Oracle (OLE DB или [!INCLUDE[vstecado](../../includes/vstecado_md.md)] ) на компьютере, на котором выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Эти компоненты поддерживают перенос данных и эмуляцию системных функций Oracle. Инструкции по установке см. в разделе [Installing SSMA for Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md).  
  
Чтобы запустить SSMA, нажмите кнопку **Пуск**, укажите пункт **все программы**, выберите **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Помощник по миграции для Oracle**, а затем щелкните **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Помощник по миграции для Oracle**.  
  
## <a name="ssma-for-oracle-user-interface"></a>SSMA для пользовательского интерфейса Oracle  
После установки SSMA можно использовать SSMA для миграции баз данных Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Перед началом работы он поможет ознакомиться с пользовательским интерфейсом SSMA. На следующей схеме показан пользовательский интерфейс для SSMA, включая обозреватель метаданных, метаданные, панели инструментов, панель вывода и панель списка ошибок.  
  
![SSMA для интерфейса пользователя Oracle](../../ssma/oracle/media/ssma_oracle_ui.jpg "SSMA для интерфейса пользователя Oracle")  
  
Чтобы начать миграцию, необходимо сначала создать новый проект. Затем вы подключаетесь к базе данных Oracle. После успешного подключения схемы Oracle отобразятся в обозревателе метаданных Oracle. Затем можно щелкнуть правой кнопкой мыши объекты в обозревателе метаданных Oracle, чтобы выполнить такие задачи, как создание отчетов, оценивающих преобразования в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Эти задачи также можно выполнить с помощью панелей инструментов и меню.  
  
Необходимо также подключиться к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . После успешного подключения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в обозревателе метаданных появится иерархия баз данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . После преобразования схем Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] схемы выберите эти преобразованные схемы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обозревателе метаданных, а затем синхронизируйте схемы с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
После синхронизации преобразованных схем с помощью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно вернуться в обозреватель метаданных Oracle и перенести данные из схем Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базы данных.  
  
Дополнительные сведения об этих задачах и способах их выполнения см. [в статье миграция баз данных Oracle в SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md).  
  
В следующих разделах описываются функции пользовательского интерфейса SSMA.  
  
### <a name="metadata-explorers"></a>Обозреватель метаданных  
SSMA содержит два обозревателя метаданных для просмотра и выполнения действий в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базах данных Oracle и.  
  
#### <a name="oracle-metadata-explorer"></a>Обозреватель метаданных Oracle  
Обозреватель метаданных Oracle отображает сведения о схемах Oracle. С помощью обозревателя метаданных Oracle можно выполнять следующие задачи:  
  
-   Просмотрите объекты в каждой схеме.  
  
-   Выберите объекты для преобразования, а затем преобразуйте объекты в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] синтаксис. Дополнительные сведения см. в разделе [Преобразование схем Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
-   Выберите таблицы для переноса данных, а затем перенесите данные из этих таблиц в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения см. [в разделе Перенос данных Oracle в SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>Обозреватель метаданных SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Обозреватель метаданных отображает сведения об экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . При подключении к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SSMA получает метаданные об этом экземпляре и сохраняет его в файле проекта.  
  
Можно использовать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Обозреватель метаданных для выбора преобразованных объектов базы данных Oracle, а затем синхронизировать эти объекты с экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Дополнительные сведения см. [в разделе Загрузка преобразованных объектов базы данных в SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
### <a name="metadata"></a>Метаданные  
Справа от каждого обозревателя метаданных находятся вкладки, описывающие выбранный объект. Например, если выбрать таблицу в обозревателе метаданных Oracle, отобразятся шесть вкладок: **Таблица**, **SQL**, **Сопоставление типов, отчет**, **Свойства** и **данные**. Вкладка **отчет** содержит сведения только после создания отчета, содержащего выбранный объект. Если выбрать таблицу в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обозревателе метаданных, отобразятся три вкладки: **Таблица**, **SQL** и **данные**.  
  
Большинство параметров метаданных доступны только для чтения. Однако можно изменить следующие метаданные:  
  
-   В обозревателе метаданных Oracle можно изменять процедуры и сопоставления типов. Чтобы преобразовать измененные процедуры и сопоставления типов, внесите изменения перед преобразованием схем.  
  
-   В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обозревателе метаданных можно изменить [!INCLUDE[tsql](../../includes/tsql-md.md)] для хранимых процедур. Чтобы увидеть эти изменения в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , внесите эти изменения перед загрузкой схем в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Изменения, внесенные в обозревателе метаданных, отражаются в метаданных проекта, а не в исходных или целевых базах данных.  
  
### <a name="toolbars"></a>Панели инструментов  
SSMA содержит две панели инструментов: панель инструментов проекта и панель инструментов миграции.  
  
#### <a name="the-project-toolbar"></a>Панель инструментов проекта  
Панель инструментов проекта содержит кнопки для работы с проектами, подключения к Oracle и подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Эти кнопки похожи на команды в меню **файл** .  
  
#### <a name="migration-toolbar"></a>Панель инструментов миграции  
В следующей таблице приведены команды панели инструментов миграции.  
  
|Кнопка|Компонент|  
|------|--------|  
|**Создать отчет**|Преобразует выбранные объекты Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] синтаксис, а затем создает отчет, показывающий, как было выполнено преобразование.<br /><br />Эта команда отключена, если в обозревателе метаданных Oracle не выбраны объекты.|  
|**Преобразовать схему**|Преобразует выбранные объекты Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] объекты.<br /><br />Эта команда отключена, если в обозревателе метаданных Oracle не выбраны объекты.|  
|**Перенос данных**|Переносит данные из базы данных Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Перед выполнением этой команды необходимо преобразовать схемы Oracle в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] схемы, а затем загрузить объекты в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Эта команда отключена, если в обозревателе метаданных Oracle не выбраны объекты.|  
|**Остановить**|Останавливает текущий процесс.|  
  
### <a name="menus"></a>Меню  
В следующей таблице показаны меню SSMA.  
  
|Меню|Описание|  
|----|-----------|  
|**Файл**|Содержит команды для работы с проектами, подключения к Oracle и подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Правка**|Содержит команды для поиска и работы с текстом на страницах со сведениями, например для копирования [!INCLUDE[tsql](../../includes/tsql-md.md)] из области сведений о SQL. Также содержит параметр **Управление закладками** , в котором отображается список существующих закладок. Для управления закладками можно использовать кнопки в правой части диалогового окна.|  
|**Вид**|Содержит команду **Синхронизация обозревателей метаданных** . Объект, синхронизирующий объекты между обозревателем метаданных Oracle и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обозревателем метаданных. Также содержит команды для отображения и скрытия **выходных** и **Список ошибок** панелей и **макетов** параметров для управления макетами.|  
|**Инструменты**|Содержит команды для создания отчетов и переноса объектов и данных. Также предоставляет доступ к диалоговым окнам **глобальные параметры** и **Параметры проекта** .|  
|**Тест-инженер**|Содержит команды для создания тестовых случаев, репозитория и системы управления резервным копированием и работы с ними.|  
|**Справка**|Предоставляет доступ к справке SSMA и диалоговому окну " **о программе** ".|  
  
### <a name="output-pane-and-error-list-pane"></a>Область вывода и область список ошибок  
Меню **вид** содержит команды для переключения видимости области вывода и список ошибок панели.  
  
-   В области вывода отображаются сообщения о состоянии от SSMA во время преобразования объектов, синхронизации объектов и переноса данных.  
  
-   В области список ошибок отображаются сообщения об ошибках, предупреждения и уведомления в списке, допускающем сортировку.  
  
## <a name="see-also"></a>См. также:  
[Перенос данных Oracle в SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md)  
[Справочник по пользовательскому интерфейсу &#40;OracleToSQL&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
