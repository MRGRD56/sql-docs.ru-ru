---
description: Выборка строк (поставщик собственного клиента OLE DB)
title: Выборка строк (поставщик собственного клиента OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- fetching rows
- OLE DB rowsets, fetching
- rowsets [OLE DB], fetching
- IRowset interface
- SQL Server Native Client OLE DB provider, fetching
ms.assetid: 5e6dbe36-b682-464d-adfa-8e886f9bd452
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4f86131da122b5c7266eeb8d23b02016f047ec13
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753534"
---
# <a name="fetching-rows-native-client-ole-db-provider"></a>Выборка строк (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Интерфейс **IRowset** — это базовый интерфейс набора строк. Интерфейс **IRowset** предоставляет методы для последовательной выборки строк, извлечения данных из этих строк и управления строками. Объекты-получатели используют методы интерфейса **IRowset** для всех базовых операций с набором строк. Сюда входят выборка и освобождение строк, а также получение значений столбцов.  
  
 Когда объект-получатель получает указатель интерфейса для набора строк, первым шагом обычно является определение свойств этого набора строк с помощью метода **IRowsetInfo::GetProperties**. Этот метод возвращает сведения об интерфейсах, предоставляемых набором строк, а также о свойствах набора строк, не отображаемых в качестве отдельных интерфейсов, таких как максимальное количество активных строк и количество строк, операции обновления которых одновременно ожидают в очереди.  
  
 Следующим шагом потребителя является определение характеристик или метаданных столбцов в наборе строк. Для этого можно воспользоваться методом **IColumnsInfo** для получения простых сведений о столбце или методом **IColumnsRowset** для получения расширенных сведений о столбце. Метод **GetColumnInfo** возвращает следующие сведения.  
  
-   Количество столбцов в результирующем наборе.  
  
-   Массив структур DBCOLUMNINFO, по одной на столбец.  
  
     Порядок структур соответствует порядку столбцов в наборе строк. Каждая структура DBCOLUMNINFO включает метаданные столбца, такие как имя столбца, порядковый номер столбца, максимально возможная длина значения в столбце, тип данных в столбце, точность и длина.  
  
-   Указатель хранилища для всех строковых значений в одном блоке распределения.  
  
 Потребитель определяет, какие столбцы ему требуются, либо на основе метаданных, либо на основе текста команды, создавшей набор строк. Он определяет порядковые номера необходимых столбцов на основе сведений об упорядочивании, возвращенных методом **IColumnsInfo**, или на основе порядковых номеров в наборе строк метаданных столбцов, возвращаемых методом **IColumnsRowset**.  
  
 Интерфейсы **IColumnsInfo** и **IColumnsRowset** используются для извлечения сведений о столбце из набора строк. Интерфейс **IColumnsInfo** возвращает ограниченный набор сведений, тогда как интерфейс **IColumnsRowset** предоставляет все метаданные.  
  
> [!NOTE]  
>  В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версии 7.0 и более ранних необязательный столбец метаданных DBCOLUMN_COMPUTEMODE, возвращаемый методом **IColumnsInfo::GetColumnsInfo**, содержит значение DBSTATUS_S_ISNULL (а не значения, указывающие, является ли столбец вычисляемым), так как он не может установить, является ли вычисляемым базовый столбец.  
  
 Порядковые номера используются для указания привязок к столбцу. Привязка — это структура, связывающая элемент пользовательской структуры со столбцом. Привязываться может значение данных, длина или значение состояния столбца.  
  
 Набор привязок объединяется в метод доступа. Он создается с помощью метода **IAccessor::CreateAccessor**. Метод доступа может содержать несколько привязок, чтобы данные из нескольких столбцов можно было получать или задавать одним вызовом. Потребитель может создать несколько методов доступа для различных способов использования в различных частях приложения. Можно создавать и освобождать методы доступа, пока существует набор строк.  
  
 Для выборки строк из базы данных объект-получатель вызывает метод, например **IRowset::GetNextRows** или **IRowsetLocate::GetRowsAt**. Эти операции выборки помещают данные строк с сервера в буфер строк поставщика. Потребитель не имеет прямого доступа к буферу строк поставщика. Объект-получатель использует метод **IRowset::GetData** для копирования данных из буфера поставщика в буфер объекта-получателя и метод **IRowsetChange::SetData** для копирования изменений из буфера объекта-получателя в буфер поставщика.  
  
 Объект-получатель вызывает метод **GetData** и передает дескриптор строке и методу доступа, а указатель буферу, выделенному объектом-получателем. Метод **GetData** преобразует данные и возвращает столбцы, как указано в привязках, использовавшихся для создания метода доступа. Объект-получатель может вызвать метод **GetData** несколько раз для одной строки, используя разные методы доступа и буферы, следовательно, он может получить несколько копий одних и тех же данных.  
  
 Данные из столбцов с переменной длиной могут обрабатываться различными способами. Прежде всего, такие столбцы могут быть привязаны к конечному разделу структуры потребителя. Это вызывает усечение, если длина данных превышает размер буфера. Потребитель может определить, что произошло усечение, проверив состояние DBSTATUS_S_TRUNCATED. Возвращаемая длина — это всегда действительная длина в байтах, соответственно, потребитель может также определить, сколько данных было усечено.  
  
 Когда объект-получатель завершает выборку или обновление строк, он освобождает их с помощью метода **ReleaseRows**. Это позволяет высвободить ресурсы, затраченные на хранение копии строк в наборе, и создает свободное пространство для новых строк. Потребитель затем может повторить цикл выборки или создания строк и доступа к данным в них.  
  
 Завершив работу с набором строк, объект-получатель вызывает метод **IAccessor::ReleaseAccessor** для освобождения метода доступа. Он вызывает метод **IUnknown::Release** для всех интерфейсов, предоставленных набором строк, для освобождения набора строк. После освобождения набора строк принудительно освобождаются все оставшиеся строки или методы доступа, которые могут удерживаться потребителем.  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Следующая позиция выборки](../../relational-databases/native-client-ole-db-rowsets/fetching-rows-next-fetch-position.md)  
  
## <a name="see-also"></a>См. также:  
 [Наборы строк](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
