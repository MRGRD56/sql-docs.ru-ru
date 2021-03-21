---
title: Выполнение операций массового копирования
description: Узнайте, как выполнять операции массового копирования и обеспечить быструю передачу данных в базу данных с помощью OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- bulk copy [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], bulk copy operations
- OLE DB Driver for SQL Server, bulk copy operations
- MSOLEDBSQL, bulk copy operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 13ddae176474396d5bbb718051069b67f60c0a8c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751194"
---
# <a name="performing-bulk-copy-operations"></a>Выполнение операций массового копирования
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Функция массового копирования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поддерживает передачу больших объемов данных в таблицу или представление [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или из них. Данные можно также передать путем указания инструкции SELECT. Данные можно передавать между [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и файлом данных операционной системы, таким как ASCII-файл. Файлы данных могут иметь различные форматы. Формат определяется для массового копирования в файле форматирования. По желанию данные можно загрузить в переменные программы и передать в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с помощью функций и методов массового копирования.  
  
 Пример приложения, демонстрирующий эту функцию, см. в разделе [Массовое копирование данных с использованием IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md).  
  
 Приложение обычно использует массовое копирование одним из следующих способов.  
  
-   Массовое копирование из таблицы, представления или результирующего набора инструкции Transact-SQL в файл данных, куда данные помещаются в том же формате, что и в таблице или представлении.  
  
     Такой файл называется файлом данных собственного режима.  
  
-   Массовое копирование из таблицы, представления или результирующего набора инструкции Transact-SQL в файл данных, куда данные помещаются в формате, отличном от формата в таблице или представлении.  
  
     В этом случае создается отдельный файл форматирования, который определяет характеристики (тип данных, позицию, длину, признак конца и т. д.) каждого столбца, когда он помещается в файл данных. Если все столбцы преобразуются в символьный формат, результирующий файл называется файлом данных символьного режима.  
  
-   Массовое копирование из файлов данных в таблицу или представление.  
  
     При необходимости файл форматирования используется для определения макета файла данных.  
  
-   Загрузите данные в переменные программы, а затем выполните импорт данных в таблицу или представление с помощью функций массового копирования для массового копирования по строке за раз.  
  
 Файлы данных, используемые функциями массового копирования, не приходится создавать при помощи другой программы массового копирования. Любая другая система может создать файл данных и файл форматирования в соответствии с определениями массового копирования. Эти файлы можно затем использовать в программе массового копирования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для импорта данных в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Например, можно экспортировать данные из электронной таблицы в файл с разделителями-символами табуляции, построить файл форматирования, описывающий файл с разделителями-символами табуляции, а затем использовать функции массового копирования для быстрого импорта данных в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Файлы данных, созданные при массовом копировании, можно также импортировать в другие приложения. Например, можно использовать функции массового копирования для экспорта данных из таблицы или представления в файл с разделителями-символами табуляции, который затем можно загрузить в электронную таблицу.  
  
 Программисты, пишущие приложения, использующие функции массового копирования, должны следовать основным правилам для обеспечения хорошей производительности массового копирования. Дополнительные сведения о поддержке операций массового копирования в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]см. в разделе [Массовый импорт и экспорт данных (SQL Server)](../../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).  
  
## <a name="limitations-and-restrictions"></a>Ограничения  
 Определяемый пользователем тип данных CLR должен быть привязан как двоичные данные. Даже если файл форматирования указывает SQLCHAR как тип данных для целевого столбца определяемого пользователем типа, программа bcp будет рассматривать данные как двоичные.  
  
 Нельзя использовать инструкцию SET FMTONLY OFF с операциями массового копирования. Ее использование может стать причиной завершения операции массового копирования с ошибкой или получению непредвиденных результатов.  
  
## <a name="ole-db-driver-for-sql-server"></a>Драйвер OLE DB для SQL Server 
 OLE DB Driver for SQL Server реализует два метода для выполнения операций массового копирования с базой данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Первый метод предусматривает использование интерфейса [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md) для операций массового копирования, работающих с памятью, а второй предусматривает использование интерфейса [IBCPSession](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md) для операций массового копирования, работающих с файлами.  
  
### <a name="using-memory-based-bulk-copy-operations"></a>Использование операций массового копирования, работающих с памятью  
 В драйвере OLE DB для SQL Server реализован интерфейс **IRowsetFastLoad**, предоставляющий поддержку операций массового копирования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], работающих с памятью. В интерфейсе **IRowsetFastLoad** реализованы методы [IRowsetFastLoad::Commit](../../oledb/ole-db-interfaces/irowsetfastload-commit-ole-db.md) и [IRowsetFastLoad::InsertRow](../../oledb/ole-db-interfaces/irowsetfastload-insertrow-ole-db.md).  
  
#### <a name="enabling-a-session-for-irowsetfastload"></a>Включение сеанса для метода IRowsetFastLoad  
 Потребитель уведомляет OLE DB Driver for SQL Server о необходимости массового копирования, устанавливая значение VARIANT_TRUE для свойства источника данных SSPROP_ENABLEFASTLOAD, которое определено в OLE DB Driver for SQL Server. Установив это свойство для источника данных, потребитель создает сеанс OLE DB Driver for SQL Server. Новый сеанс позволяет потребителю получить доступ к интерфейсу **IRowsetFastLoad**.  
  
> [!NOTE]  
>  Если для инициализации источника данных используется интерфейс **IDataInitialize**, то необходимо задать свойство SSPROP_IRowsetFastLoad в параметре *rgPropertySets* метода **IOpenRowset::OpenRowset**. В противном случае вызов метода **OpenRowset** возвращает ошибку E_NOINTERFACE.  
  
 Включение сеанса для массового копирования ограничивает поддержку OLE DB Driver for SQL Server для интерфейсов в сеансе. Сеанс с включенным массовым копированием предоставляет только следующие интерфейсы:  
  
-   **IDBSchemaRowset**  
  
-   **IGetDataSource**  
  
-   **IOpenRowset**  
  
-   **ISupportErrorInfo**  
  
-   **ITransactionJoin**  
  
 Чтобы отключить создание набора строк с включенным массовым копированием и вернуть сеанс драйвера OLE DB для SQL Server к обычной обработке, задайте для свойства SSPROP_ENABLEFASTLOAD значение VARIANT_FALSE.  
  
#### <a name="irowsetfastload-rowsets"></a>Наборы строк IRowsetFastLoad  
 Наборы строк для массового копирования драйвера OLE DB для SQL Server доступны только для записи, но они предоставляют интерфейсы, позволяющие потребителю определить структуру таблицы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Следующие интерфейсы доступны в OLE DB Driver for SQL Server с поддержкой массового копирования для набора строк SQL Server:  
  
-   **IAccessor**  
  
-   **IColumnsInfo**  
  
-   **IColumnsRowset**  
  
-   **IConvertType**  
  
-   **IRowsetFastLoad**  
  
-   **IRowsetInfo**  
  
-   **ISupportErrorInfo**  
  
 Свойства SSPROP_FASTLOADOPTIONS, SSPROP_FASTLOADKEEPNULLS и SSPROP_FASTLOADKEEPIDENTITY, зависящие от поставщика, управляют режимом работы набора строк для массового копирования драйвера OLE DB для SQL Server. Свойства указаны в элементе *rgProperties* элемента параметра *rgPropertySets* интерфейса **IOpenRowset**.  
  
|Идентификатор свойства|Описание|  
|-----------------|-----------------|  
|SSPROP_FASTLOADKEEPIDENTITY|Столбец: нет<br /><br /> Ч/З Чтение/запись<br /><br /> Тип: VT_BOOL.<br /><br /> Значение по умолчанию: VARIANT_FALSE<br /><br /> Описание. Поддерживает значения идентификаторов, указанные объектом-получателем.<br /><br /> VARIANT_FALSE: значения для столбца идентификаторов в таблице [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] формируются [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Любое значение, привязанное к столбцу, игнорируется OLE DB Driver for SQL Server.<br /><br /> VARIANT_TRUE: потребитель привязывает метод доступа, предоставляющий значение, к столбцу идентификаторов [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Свойство идентификатора недоступно для столбцов, допускающих значение NULL, так что потребитель предоставляет уникальное значение для каждого вызова **IRowsetFastLoad::Insert**.|  
|SSPROP_FASTLOADKEEPNULLS|Столбец: нет<br /><br /> Ч/З Чтение/запись<br /><br /> Тип: VT_BOOL.<br /><br /> Значение по умолчанию: VARIANT_FALSE<br /><br /> Описание. Поддерживает значение NULL для столбцов с ограничением DEFAULT. Затрагивает только столбцы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], которые допускают значение NULL и к которым применено ограничение DEFAULT.<br /><br /> VARIANT_FALSE: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] вставляет значение по умолчанию для столбца при вставке потребителем драйвера OLE DB для SQL Server строки, содержащей значение NULL для столбца.<br /><br /> VARIANT_TRUE: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] вставляет значение NULL для значения столбца при вставке потребителем драйвера OLE DB для SQL Server строки, содержащей значение NULL для столбца.|  
|SSPROP_FASTLOADOPTIONS|Столбец: нет<br /><br /> Ч/З Чтение/запись<br /><br /> Тип: VT_BSTR<br /><br /> По умолчанию: нет<br /><br /> Описание. Это свойство похоже на параметр **-h** "*hint*[,...*n*]" программы **bcp**. Следующие строки можно использовать в качестве параметров при массовом копировании данных в таблицу.<br /><br /> **ORDER**(*column*[**ASC** &#124; **DESC**][,...*n*]): порядок сортировки данных в файле данных. Производительность массового копирования увеличивается, если загружаемый файл данных упорядочен по кластеризованному индексу таблицы.<br /><br /> **ROWS_PER_BATCH** = *bb*: Количество строк данных в каждом пакете (значение *bb*). Сервер оптимизирует массовую загрузку в соответствии со значением *bb*. По умолчанию значение аргумента **ROWS_PER_BATCH** неизвестно.<br /><br /> **KILOBYTES_PER_BATCH** = *cc*: количество килобайт (KБ) данных в каждом пакете (значение cc). По умолчанию значение **KILOBYTES_PER_BATCH** неизвестно.<br /><br /> **TABLOCK**: блокировка уровня таблицы запрашивается на время операции массового копирования. Применение этого параметра значительно повышает производительность, так как удержание блокировки только в течение операции массового копирования уменьшает вероятность конфликтов блокировок в таблице. Таблица может загружаться одновременно несколькими клиентами, если она не содержит индексов и указан параметр **TABLOCK**. По умолчанию работа блокировки определяется параметром таблицы **table lock on bulk load**.<br /><br /> **CHECK_CONSTRAINTS**: любые ограничения на *имя_таблицы* проверяются на протяжении операции массового копирования. По умолчанию ограничения не учитываются.<br /><br /> **FIRE_TRIGGER**: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] использует управление версиями строк для триггеров. Версии строк сохраняются в хранилище версий базы данных **tempdb**. Поэтому оптимизации массовой записи в журнал доступны даже при включенных триггерах. Перед массовым импортом пакета с большим количеством строк с включенными триггерами, возможно, придется увеличить размер базы данных **tempdb**.|  
  
### <a name="using-file-based-bulk-copy-operations"></a>Использование операций массового копирования, работающих с файлами  
 В драйвере OLE DB для SQL Server предусмотрен интерфейс **IBCPSession**, предоставляющий поддержку операций массового копирования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], работающих с файлами. В интерфейсе **IBCPSession** реализованы методы [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md), [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md), [IBCPSession::BCPControl](../../oledb/ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md), [IBCPSession::BCPDone](../../oledb/ole-db-interfaces/ibcpsession-bcpdone-ole-db.md), [IBCPSession::BCPExec](../../oledb/ole-db-interfaces/ibcpsession-bcpexec-ole-db.md), [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md), [IBCPSession::BCPReadFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md) и [IBCPSession::BCPWriteFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md).  
  
  
## <a name="see-also"></a>См. также:  
 [Возможности драйвера OLE DB для SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [Свойства источника данных &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)   
 [Массовый импорт и экспорт данных (SQL Server)](../../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)   
 [IBCPSession (OLE DB)](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   

