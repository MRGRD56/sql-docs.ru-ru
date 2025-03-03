---
description: Выборка итоговых данных
title: Выборка результирующих данных | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC], fetching
- SQLBindCol function
- result sets [ODBC], fetching
- fetching [ODBC]
- ODBC data types, fetching
- SQLFetch function
- SQL Server Native Client ODBC driver, data types
- SQLGetData function
ms.assetid: b289c7fb-5017-4d7e-a2d3-19401e9fc4cd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f151fefdb895259720904b9eda81719be1703bb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752784"
---
# <a name="fetching-result-data"></a>Выборка итоговых данных
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Приложение ODBC имеет три параметра для выборки данных результата.  
  
 Первый вариант основан на [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md). Перед получением результирующего набора приложение использует **SQLBindCol** для привязки каждого столбца результирующего набора к программной переменной. После привязки столбцов драйвер передает данные текущей строки в переменные, привязанные к столбцам результирующего набора каждый раз, когда приложение вызывает **SQLFetch** или [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md). Если столбец результирующего набора и программная переменная имеют разные типы данных, драйвер выполняет преобразования данных. Если приложение имеет SQL_ATTR_ROW_ARRAY_SIZE больше 1, оно может привязать результирующие столбцы к массивам переменных, которые будут заполнены при каждом вызове **SQLFetchScroll**.  
  
 Второй вариант основан на [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md). Приложение не использует **SQLBindCol** для привязки столбцов результирующего набора к переменным программы. После каждого вызова **SQLFetch** приложение вызывает **SQLGetData** один раз для каждого столбца в результирующем наборе. **SQLGetData** инструктирует драйвер для передачи данных из определенного столбца результирующего набора в определенную переменную программы и задает типы данных столбца и переменной. Это позволяет драйверу преобразовать данные, если столбец результата и программная переменная имеют разные типы данных. Столбцы типа **Text**, **ntext** и **Image** обычно слишком велики, чтобы вместить их в переменную программы, но по-прежнему можно получить с помощью **SQLGetData**. Если данные типа **Text**, **ntext** или **Image** в столбце Result больше, чем переменная программы, **SQLGetData** возвращает SQL_SUCCESS_WITH_INFO и SQLSTATE 01004 (строковые данные, усеченные справа). Последовательные вызовы **SQLGetData** возвращают последовательные фрагменты данных **текста** или **изображения** . По достижении конца данных **SQLGetData** возвращает SQL_SUCCESS. Если SQL_ATTR_ROW_ARRAY_SIZE больше 1, то каждая выборка возвращает набор строк. Перед использованием **SQLGetData** необходимо сначала использовать функцию **SQLSetPos** , чтобы указать определенную строку в наборе строк в качестве текущей строки.  
  
 Третий вариант — использовать сочетание **SQLBindCol** и **SQLGetData**. Приложение может, например, привязать первые десять столбцов результирующего набора, а затем при каждой выборки вызвать **SQLGetData** три раза, чтобы получить данные из трех несвязанных столбцов. Обычно это используется, когда результирующий набор содержит один или несколько столбцов типа **Text** или **Image** .  
  
 В зависимости от параметров курсора, заданных для результирующего набора, приложение может также использовать параметры прокрутки **SQLFetchScroll** для прокрутки результирующего набора.  
  
 Чрезмерное использование **SQLBindCol** для привязки столбца результирующего набора к программной переменной является дорогостоящим, так как **SQLBindCol** приводит к тому, что драйвер ODBC выделяет память. При привязке результирующего столбца к переменной эта привязка остается действующей до тех пор, пока не будет вызван [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) для освобождения маркера инструкции или вызов [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) с *параметром fOption* , для которого задано значение SQL_UNBIND. По завершении инструкции привязки автоматически не снимаются.  
  
 Эта логика позволяет эффективно выполнять инструкцию SELECT несколько раз с различными параметрами. Так как результирующий набор сохраняет одну и ту же структуру, можно привязать результирующий набор один раз, обработать все инструкции SELECT, а затем вызвать **SQLFreeStmt** с параметром *параметром foption* , чтобы SQL_UNBIND после последнего выполнения. Не следует вызывать **SQLBindCol** для привязки столбцов в результирующем наборе без предварительного вызова **SQLFreeStmt** с *параметром fOption* , равным SQL_UNBIND, чтобы освободить все предыдущие привязки.  
  
 При использовании **SQLBindCol** можно выполнить привязку на уровне строки или на уровне столбца. Привязка параметров на уровне строк быстрее, чем привязка на уровне столбцов.  
  
 **SQLGetData** можно использовать для получения данных по столбцам вместо привязки столбцов результирующего набора с помощью **SQLBindCol**. Если результирующий набор содержит всего несколько строк, использование **SQLGetData** вместо **SQLBindCol** выполняется быстрее. в противном случае **SQLBindCol** обеспечивает наилучшую производительность. Если данные не всегда помещаются в один и тот же набор переменных, следует использовать **SQLGetData** вместо постоянной повторной привязки. **SQLGetData** можно использовать только для столбцов, которые находятся в списке выбора после привязки всех столбцов к **SQLBindCol**. Этот столбец также должен присутствовать после всех столбцов, на которых уже используется **SQLGetData**.  
  
 Функции ODBC, которые работают с перемещением данных в переменные программы, такие как **SQLGetData**, **SQLBindCol** и [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md), поддерживают неявное преобразование типов данных. Например, если приложение привязывает целочисленный столбец к строковой программной переменной, драйвер автоматически преобразует данные из целочисленного в символьный тип перед тем, как поместить их в программную переменную.  
  
 Количество преобразований данных в приложениях должно быть минимальным. Если преобразование данных не требуется для вычислений, производимых приложением, приложения привяжут столбцы и параметры к программным переменным того же типа данных. Однако, если данные должны быть преобразованы из одного типа в другой, более эффективным будет преобразовать данные с помощью драйвера, чем делать это в приложении. Драйвер для собственного клиента ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обычно только передает данные непосредственно из сетевых буферов в переменные приложения. Обращение к драйверу для добавления преобразования данных приведет к буферизации данных драйвером и использованию времени ЦП для преобразования данных.  
  
 Программные переменные должны быть достаточно большими для хранения данных, передаваемых из столбца, за исключением данных типа **Text**, **ntext** и **Image** . Если приложение пытается получить результирующий набор данных и сохранить его в переменной недостаточного размера, драйвер сформирует предупреждение. Это заставит драйвер выделить память для сообщения, а драйвер и приложение будут вынуждены тратить время ЦП на обработку сообщения и ошибок. Приложение должно либо выделить достаточно большую переменную для сохранения полученных данных, либо использовать функцию SUBSTRING в списке выбора для уменьшения размера столбца в результирующем наборе.  
  
 Следует соблюдать осторожность при использовании SQL_C_DEFAULT для указания типа переменной C. SQL_C_DEFAULT указывает, что тип переменной C соответствует типу данных SQL столбца или параметра. Если для столбца **ntext**, **nchar** или **nvarchar** задано SQL_C_DEFAULT, в приложение возвращаются данные в Юникоде. Это может привести к возникновению различных проблем, если приложение не было настроено для обработки данных в Юникоде. Аналогичные типы проблем могут возникать при использовании типа данных **uniqueidentifier** (SQL_GUID).  
  
 данные типа **Text**, **ntext** и **Image** обычно слишком велики, чтобы вместить их в одну переменную программы, и обычно обрабатываются с помощью **SQLGetData** вместо **SQLBindCol**. При использовании серверных курсоров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента оптимизирован для передачи данных для непривязанных столбцов типа **Text**, **ntext** или **Image** на момент выборки строки. Данные типа **Text**, **ntext** или **Image** на самом деле не извлекаются с сервера, пока приложение не выдаст **SQLGetData** для столбца.  
  
 Эту оптимизацию можно применить к приложениям, чтобы не отображались данные типа **Text**, **ntext** или **Image** , пока пользователь прокручивает курсор вверх и вниз. После того как пользователь выберет строку, приложение может вызвать **SQLGetData** для получения данных типа **Text**, **ntext** или **Image** . Это сохраняет передачу данных типа **Text**, **ntext** или **Image** для любой строки, которую пользователь не выбирает, и может сохранять передачу очень больших объемов данных.  
  
## <a name="see-also"></a>См. также:  
 [Обработка результатов &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
