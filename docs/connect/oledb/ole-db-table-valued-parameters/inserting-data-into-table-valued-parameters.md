---
title: Вставка данных в возвращающие табличное значение параметры (драйвер OLE DB) | Документация Майкрософт
description: Драйвер OLE DB Driver for SQL Server поддерживает задание объектами-получателями данных для строк параметров с табличными значениями принудительно и по запросу.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, inserting data into
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c815febe24b321efb1ec1eb1dc99080baf3c5d43
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750844"
---
# <a name="inserting-data-into-table-valued-parameters"></a>Вставка данных в параметры, возвращающие табличные значения
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Драйвер OLE DB Driver for SQL Server поддерживает две модели задания потребителем данных для строк параметров с табличными значениями: принудительная и по запросу. Образец, в котором демонстрируется применение модели по запросу, см. в разделе [Образцы программирования служб SQL Server](https://msftdpprodsamples.codeplex.com/).  
  
> [!NOTE]  
>  Столбец возвращающего табличное значение параметра должен содержать во всех строках либо значения по умолчанию, либо значения, отличные от значений по умолчанию. Нельзя, чтобы в одних строках были значения по умолчанию, а в других — значения, отличные от них. Следовательно, в привязках табличных параметров единственными значениями состояния, разрешенными для столбца набора строк возвращающего табличное значение параметра, являются DBSTATUS_S_ISNULL и DBSTATUS_S_OK. Значение DBSTATUS_S_DEFAULT приведет к сбою, и значению состояния привязки будет присвоено значение DBSTATUS_E_BADSTATUS.  
  
## <a name="push-model-loads-all-table-valued-parameter-data-in-memory"></a>Принудительная модель (загружает все данные табличного параметра в память)  
 Принудительная модель напоминает использование наборов параметров (то есть параметра DBPARAMS в методе ICommand::Execute). Принудительная модель используется только в том случае, если объекты набора строк возвращающего табличное значение параметра применяются без пользовательской реализации интерфейсов IRowset. Принудительная модель рекомендуется в том случае, если набор строк возвращающего табличное значение параметра содержит небольшое число строк и не потребует от приложения большого объема памяти. Эта модель проще, поскольку она не нуждается в больших возможностях приложения потребителя, довольствуясь стандартными возможностями приложений OLE DB.  
  
 Ожидается, что перед выполнением команды потребитель предоставит поставщику все данные для возвращающего табличное значение параметра. Для этого он заполняет объект набора строк табличного параметра для каждого возвращающего табличное значение параметра. Объект набора строк возвращающего табличное значение параметра реализует для набора строк операции Insert, Set и Delete, которые потребитель будет использовать для обработки данных возвращающего табличное значение параметра. Поставщик получает данные из объекта набора строк возвращающего табличное значение параметра во время выполнения.  
  
 Как только набор строк возвращающего табличное значение параметра предоставляется потребителю, он может начать работу с ним как с набором строк. Затем потребитель получает сведения о типе каждого столбца (тип, максимальная длина, точность и масштаб) с помощью интерфейсного метода IColumnsInfo::GetColumnInfo или IColumnsRowset::GetColumnsRowset. После этого потребитель создает метод доступа для определения привязок для данных. Следующий шаг — вставка строк данных в набор строк возвращающего табличное значение параметра. Это можно выполнить, используя IRowsetChange::InsertRow. Методы IRowsetChange::SetData и IRowsetChange::DeleteRows также могут использоваться в объекте набора строк возвращающего табличное значение параметра, если необходимо управление данными. Для объекта набора строк возвращающего табличное значение параметра имеется счетчик ссылок, как и у объектов потока.  
  
 При использовании IColumnsRowset::GetColumnsRowset на объекте набора строк результирующего столбца будут происходить последующие вызовы методов IRowset::GetNextRows, IRowset::GetData и IRowset::ReleaseRows.  
  
 После того как драйвер OLE DB для SQL Server начнет выполнять команду, значения возвращающего табличное значение параметра выбираются из объекта набора строк и отправляются на сервер.  
  
 Принудительная модель требует минимальных затрат от потребителя, но использует больше памяти, чем модель по запросу, поскольку все данные возвращающего табличное значение параметра во время выполнения приходится хранить в памяти.  
  
## <a name="pull-model-obtaining-table-valued-parameter-data-on-demand-from-the-consumer"></a>Модель «по запросу» (получает данные возвращающего табличное значение параметра по запросу потребителя)  
 Применение модели по запросу выгодно в следующих двух случаях.  
  
-   Для создания потока строк.  
  
-   Если в качестве значения возвращающего табличное значение параметра используется набор строк от другого поставщика.  
  
 В модели получения по запросу заказчик предоставляет данные поставщику по запросу. Этот подход следует использовать в том случае, если приложение реализует множество операций вставки и хранение набора строк возвращающего табличное значение параметра потребовало бы большого объема памяти. Если используются несколько поставщиков OLE DB, то модель по запросу позволяет потребителю предоставлять любые объекты наборов строк в качестве значения возвращающего табличное значение параметра.  
  
 Для применения модели по запросу потребитель должен предоставить собственную реализацию объекта набора строк. При использовании модели по запросу с наборами строк возвращающего табличное значение параметра (CLSID_ROWSET_TVP) потребителю необходимо выполнить статистическую обработку объекта набора строк возвращающего табличное значение параметра, которые поставщик передает с помощью метода ITableDefinitionWithConstraints::CreateTableWithConstraints или IOpenRowset::OpenRowset. Ожидается, что объект потребителя переопределяет только реализацию интерфейса IRowset. Должны быть переопределены следующие функции.  
  
-   IRowset::GetNextRows  
  
-   IRowset::AddRefRows  
  
-   IRowset::GetData  
  
-   IRowset::ReleaseRows  
  
-   IRowset::RestartPosition  
  
 Драйвер OLE DB для SQL Server считывает одновременно одну или несколько строк из объекта набора строк потребителя для поддержки потокового поведения возвращающих табличные значения параметров. Например, пользователь может иметь данные набора строк возвращающего табличное значение параметра на диске (а не в памяти) и реализовать считывание данных с диска, если это требуется драйвером OLE DB для SQL Server.  
  
 Потребитель передает свой формат данных OLE DB Driver for SQL Server с помощью метода IAccessor::CreateAccessor в объекте набора строк, возвращающего табличное значение параметра. При считывании данных из буфера потребителя поставщик удостоверяется, что все изменяемые столбцы со значениями, отличающимися от значений по умолчанию, доступны хотя бы через один дескриптор метода доступа и использует соответствующие дескрипторы для считывания данных столбца. Чтобы избежать неоднозначности, между столбцом набора строк возвращающего табличное значение параметра и привязкой должно быть однозначное соответствие. Повторяющиеся привязки к одному и тому же столбцу приведут к ошибке. Кроме того, элементы *iOrdinal* каждого метода доступа DBBindings должны быть последовательно упорядочены. Метод IRowset::GetData будет вызываться ровно столько раз, сколько существует методов доступа для одной строки, а порядок вызовов определяется порядком возрастания значений *iOrdinal*.  
  
 Ожидается, что поставщик реализует большую часть интерфейсов, предоставляемых объектом набора строк возвращающего табличное значение параметра. Потребитель реализует объект набора строк с минимальным количеством интерфейсов (IRowset). Благодаря передаче интерфейсов вслепую оставшиеся интерфейсы объекта набора строк реализуются объектом набора строк возвращающего табличное значение параметра.  
  
 Для всех остальных объектов набора строк (например, получаемых для поставщиков OLE DB), предоставленный потребителем набор строк должен реализовать все обязательные интерфейсы объектов набора строк, как указано в спецификации OLE DB.  
  
 Во время выполнения драйвер OLE DB для SQL Server выполняет обратный вызов объекта набора строк для выборки строк и считывания данных столбца.  
  
## <a name="see-also"></a>См. также:  
 [Возвращающие табличные значения параметры &#40;OLE DB&#41;](../../oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Использование возвращающих табличные значения параметров &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
