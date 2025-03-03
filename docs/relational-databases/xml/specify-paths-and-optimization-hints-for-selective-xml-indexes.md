---
title: Пути и указания по оптимизации для селективных XML-индексов | Документация Майкрософт
description: Узнайте, как задавать пути к узлам и указания по оптимизации при создании или изменении селективных XML-индексов.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
ms.assetid: 486ee339-165b-4aeb-b760-d2ba023d7d0a
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 71fa7fad610b309232d1fd80e381fbc1674d1143
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487462"
---
# <a name="specify-paths-and-optimization-hints-for-selective-xml-indexes"></a>Задайте путь и указания по оптимизации для селективных XML-индексов
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В данном разделе описывается настройка путей узла для индексирования и задание указаний по оптимизации для индексирования при создании или изменении селективных XML-индексов.  
  
 Пути узла и указания по оптимизации задаются одновременно в одной из следующих инструкций:  
  
-   В предложении **FOR** инструкции **CREATE**. Дополнительные сведения см. в разделе [CREATE SELECTIVE XML INDEX (Transact-SQL)](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
-   В предложении **ADD** инструкции **ALTER**. Дополнительные сведения см. в разделе [ALTER INDEX (селективные XML-индексы)](../../t-sql/statements/alter-index-selective-xml-indexes.md).  
  
 Дополнительные сведения о селективных XML-индексах см. в разделе [Выборочный XML-индекс (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md).  
  
##  <a name="understanding-xquery-and-sql-server-types-in-untyped-xml"></a><a name="untyped"></a> Основные сведения о типах Xquery и SQL Server в нетипизированном XML  
 Селективные XML-индексы поддерживают две системы типов — типы XQuery и типы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Индексированный путь можно использовать для сопоставления с выражением Xquery или с возвращаемым типом метода value() типа данных XML.  
  
-   Если путь для индексирования не снабжен заметками или ключевым словом XQUERY, путь будет сопоставляться с выражением Xquery. Есть две вариации путей узла, снабженных ключевым словом XQUERY.  
  
    -   Если ключевое слово XQUERY и тип данных XQuery не указаны, будут использоваться сопоставления по умолчанию. Обычно производительность и хранение не являются оптимальными.  
  
    -   Если было указано ключевое слово XQUERY и тип данных Xquery, а также другие указания по оптимизации (необязательно), вы можете достичь наилучшей возможной производительности и наиболее эффективного хранения. Однако приведение может завершиться ошибкой.  
  
-   Если путь для индексирования снабжен ключевым словом SQL, путь сопоставляется с возвращаемым типом метода value() типа данных XML. Укажите подходящий тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , то есть возвращаемый тип, который ожидается от метода value().  
  
 Есть малозаметные различия между системой типов XML выражений Xquery и системой типов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , примененных к методу value() типа данных XML. Различия следующие:  
  
-   Система типов Xquery учитывает конечные пробелы. Например, в соответствии с семантикой типов Xquery строки «abc» и «abc » не равны, тогда как в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] эти строки равны.  
  
-   Типы данных с плавающей запятой Xquery поддерживают специальные значения +/- нуль и +/- бесконечность. Эти особые значения не поддерживаются в типах данных с плавающей запятой [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="xquery-types-in-untyped-xml"></a>Типы Xquery в нетипизированном XML  
  
-   Типы XQuery сопоставляются с выражениями XQuery во всех методах типа данных XML, включая метод value().  
  
-   Типы XQuery поддерживают такие указания оптимизации: node(), SINGLETON, DATA TYPE и MAXLENGTH.  
  
 В выражениях Xquery с нетипизированным XML вы можете выбрать между двумя режимами работы.  
  
-   **Режим сопоставления по умолчанию**. В этом режиме при создании селективного XML-индекса указывается только путь.  
  
-   **Определяемый пользователем режим сопоставления**. В этом режиме указывается как путь, так и необязательные указания по оптимизации.  
  
 Режим сопоставления по умолчанию использует консервативный режим хранения, который всегда безопасен и универсален. Он может быть сопоставлен с любым типом выражения. Ограничением режима сопоставления по умолчанию является неоптимальная производительность, поскольку требуется увеличение количества приведений среды выполнения. Также будут недоступны вторичные индексы.  
  
 Ниже приведен пример селективного XML-индекса, созданного с сопоставлениями по умолчанию. Для всех трех методов используется тип узла (**xs:untypedAtomic**) и количество элементов по умолчанию.  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_UX_default  
ON Tbl(xmlcol)  
FOR  
(  
mypath01 =  '/a/b',  
mypath02 = '/a/b/c',  
mypath03 = '/a/b/d'  
)  
```  
  
 Определяемый пользователем режим сопоставления позволяет указать тип и количество элементов для узла, чтобы получить более высокую производительность. Однако это повышение производительности достигается за счет снижения безопасности (поскольку приведения могут завершаться с ошибками) и универсальности (поскольку с селективным XML-индексом сопоставляется только заданный тип).  
  
 Для нетипизированного XML поддерживаются следующие типы Xquery:  
  
-   **xs:boolean**  
  
-   **xs:double**  
  
-   **xs:string**  
  
-   **xs:date**  
  
-   **xs:time**  
  
-   **xs:dateTime**  
  
 Если тип не указан, предполагается, что узел имеет тип данных **xs:untypedAtomic** .  
  
 Отображенный селективный XML-индекс вы можете оптимизировать следующим образом:  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_UX_optimized  
ON Tbl(xmlcol)  
FOR  
(  
mypath= '/a/b' as XQUERY 'node()',  
pathX = '/a/b/c' as XQUERY 'xs:double' SINGLETON,  
pathY = '/a/b/d' as XQUERY 'xs:string' MAXLENGTH(200) SINGLETON  
)  
-- mypath - Only the node value is needed; storage is saved.  
-- pathX - Performance is improved; secondary indexes are possible.  
-- pathY - Performance is improved; secondary indexes are possible; storage is saved.  
```  
  
### <a name="sql-server-types-in-untyped-xml"></a>Типы SQL Server в нетипизированном XML  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] соответствуют возвращаемому значению метода value().  
  
-   Типы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживают указание оптимизации SINGLETON.  
  
 Указание типа обязательно для путей, возвращающих типы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Необходимо использовать тот же тип [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , который бы использовался в методе value().  
  
 Обратите внимание на следующий запрос:  
  
```sql  
SELECT T.record,  
    T.xmldata.value('(/a/b/d)[1]', 'NVARCHAR(200)')  
FROM myXMLTable T  
```  
  
 Указанный запрос возвращает значение из пути `/a/b/d` , упакованного в тип данных NVARCHAR(200), поэтому тип данных, указываемый для узла, является очевидным. Однако нет схемы, в которой можно было бы указать количество элементов узла в нетипизированном XML. Чтобы указать, что узел `d` должен встречаться не более одного раза в родительском узле `b`, создайте селективный XML-индекс, в котором подсказка оптимизации SINGLETON используется следующим образом:  
  
```sql  
CREATE SELECTIVE XML INDEX example_sxi_US  
ON Tbl(xmlcol)  
FOR  
(  
node1223 = '/a/b/d' as SQL NVARCHAR(200) SINGLETON  
)  
```  
  
  
##  <a name="understanding-selective-xml-index-support-for-typed-xml"></a><a name="typed"></a> Основные сведения о поддержке селективных XML-индексов для типизированного XML  
 Типизированный XML в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — это схема, связанная с данным XML-документом. Схема определяет общую структуру документа и типы узлов. Если схема существует, то селективный XML-индекс применяет структуру схемы, когда пользователь повышает пути, поэтому нет необходимости указывать типы XQUERY для путей.  
  
 Селективные XML-индексы поддерживают следующие типы XSD:  
  
-   **xs:anyUri**  
  
-   **xs:boolean**  
  
-   **xs:date**  
  
-   **xs:dateTime**  
  
-   **xs:day**  
  
-   **xs:decimal**  
  
-   **xs:double**  
  
-   **xs:float**  
  
-   **xs:int**  
  
-   **xs:integer**  
  
-   **xs:language**  
  
-   **xs:long**  
  
-   **xs:name**  
  
-   **xs:NCName**  
  
-   **xs:negativeInteger**  
  
-   **xs:nmtoken**  
  
-   **xs:nonNegativeInteger**  
  
-   **xs:nonPositiveInteger**  
  
-   **xs:positiveInteger**  
  
-   **xs:qname**  
  
-   **xs:short**  
  
-   **xs:string**  
  
-   **xs:time**  
  
-   **xs:token**  
  
-   **xs:unsignedByte**  
  
-   **xs:unsignedInt**  
  
-   **xs:unsignedLong**  
  
-   **xs:unsignedShort**  
  
 Если селективный XML-индекс создается для документа, имеющего связанную с ним схему, то при указании типа XQUERY при создании или изменении индекса будет возвращена ошибка. Пользователь может использовать заметки типов SQL в части повышения пути. Тип SQL должен быть допустимым преобразованием из типа XSD, определенного в схеме, иначе будет выдана ошибка. Поддерживаются все типы SQL, имеющие адекватное представление в XSD, за исключением типов данных даты-времени.  
  
> [!NOTE]  
>  Селективный индекс используется, если в повышении пути селективного XML-индекса указан тот же тип, что и в возвращаемом значении метода value().  
  
 Следующие указания оптимизации можно использовать с типизированными XML-документами.  
  
-   Указание по оптимизации node().  
  
-   Указание оптимизации MAXLENGTH можно использовать с типами xs:string, чтобы сократить индексируемое значение.  
  
 Дополнительные сведения об указаниях оптимизации см. в разделе [Задание указаний по оптимизации](#hints).  
  
##  <a name="specifying-paths"></a><a name="paths"></a> Указание путей  
 Селективный XML-индекс позволяет индексировать только подмножество узлов из сохраненных XML-данных, значимое для запросов, которые планируется выполнять. Когда подмножество значимых узлов намного меньше, чем общее количество узлов в XML-документе, селективный XML-индекс сохраняет только значимые узлы. Для эффективного использования селективного XML-индекса необходимо определить нужное подмножество узлов для индексирования.  
  
### <a name="choosing-the-nodes-to-index"></a>Выбор узлов для индексирования  
 Следующие два простых принципа позволяют определить нужное подмножество узлов, которые необходимо добавить в селективный XML-индекс.  
  
1.  **Принцип 1**. Чтобы оценить заданное выражение XQuery, необходимо проиндексировать все узлы, подлежащие изучению.  
  
    -   Индексируйте все узлы, существование или значения которых используются в выражении XQuery.  
  
    -   Индексируйте все узлы в выражении XQuery, к которым применяются предикаты XQuery.  
  
     Рассмотрим следующий простой запрос к [образцу XML-документа](#sample) в данном разделе:  
  
    ```sql  
    SELECT T.record FROM myXMLTable T  
    WHERE T.xmldata.exist('/a/b[./c = "43"]') = 1  
    ```  
  
     Для возвращения экземпляров XML, которые удовлетворяют запросу, селективному XML-индексу необходимо просматривать 2 узла в каждом из экземпляров XML.  
  
    -   Узел `c`, так как его значение используется в выражении XQuery.  
  
    -   Узел `b`, поскольку предикат применяется на узел`b` в выражении XQuery.  
  
2.  **Принцип 2**. Для достижения наилучшей производительности рекомендуется индексировать все узлы, необходимые для вычисления заданного выражения XQuery. Если индексируются только некоторые узлы, селективный XML-индекс улучшает оценку вложенных выражений, содержащих только индексированные узлы.  

 Для улучшения производительности инструкции SELECT, описанной выше, вы можете создать следующий селективный XML-индекс.  
  
```sql  
CREATE SELECTIVE XML INDEX simple_sxi  
ON Tbl(xmlcol)  
FOR  
(  
    path123 =  '/a/b',  
    path124 =  '/a/b/c'  
)  
```  
  
### <a name="indexing-identical-paths"></a>Индексирование одинаковых путей  
 Невозможно повысить уровень одинаковых путей в качестве того же типа данных с другими именами пути. Например, следующий запрос формирует ошибку, поскольку пути `pathOne` и `pathTwo` идентичны:  
  
```sql  
CREATE SELECTIVE INDEX test_simple_sxi ON T1(xmlCol)  
FOR  
(  
    pathOne = 'book/authors/authorID' AS XQUERY 'xs:string',  
    pathTwo = 'book/authors/authorID' AS XQUERY 'xs:string'  
)  
```  
  
 Однако вы можете повысить уровень одинаковых путей в качестве различных типов данных с разными именами. Например, следующий запрос теперь является допустимым, поскольку типы данных отличаются:  
  
```sql  
CREATE SELECTIVE INDEX test_simple_sxi ON T1(xmlCol)  
FOR  
(  
    pathOne = 'book/authors/authorID' AS XQUERY 'xs:double',  
    pathTwo = 'book/authors/authorID' AS XQUERY 'xs:string'  
)  
```  
  
### <a name="examples"></a>Примеры  
 Ниже приведены дополнительные примеры выбора правильных узлов для индексации для различных типов XQuery.  
  
 **Пример 1**  
  
 Вот простой запрос Xquery, в котором используется метод exist():  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b/c/d/e/h') = 1  
```  
  
 В следующей таблице показаны узлы, которые должны быть индексированы для того, чтобы данный запрос мог использовать селективный XML-индекс.  
  
|Узел, который необходимо включить в индекс|Причина для индексирования этого узла|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e/h**|Наличие узла `h` вычисляется в методе exist().|  
  
 **Пример 2**  
  
 Вот более сложный вариант предыдущего запроса XQuery с примененным предикатом:  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b/c/d/e[./f = "SQL"]') = 1  
```  
  
 В следующей таблице показаны узлы, которые должны быть индексированы для того, чтобы данный запрос мог использовать селективный XML-индекс.  
  
|Узел, который необходимо включить в индекс|Причина для индексирования этого узла|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|Предикат применяется для узла `e`.|  
|**/a/b/c/d/e/f**|Значение узла `f` вычисляется внутри предиката.|  
  
 **Пример 3**  
  
 Вот более сложный запрос с предложением value():  
  
```sql  
SELECT T.record,  
    T.xmldata.value('(/a/b/c/d/e[./f = "SQL"]/g)[1]', 'nvarchar(100)')  
FROM myXMLTable T  
```  
  
 В следующей таблице показаны узлы, которые должны быть индексированы для того, чтобы данный запрос мог использовать селективный XML-индекс.  
  
|Узел, который необходимо включить в индекс|Причина для индексирования этого узла|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|Предикат применяется для узла `e`.|  
|**/a/b/c/d/e/f**|Значение узла `f` вычисляется внутри предиката.|  
|**/a/b/c/d/e/g**|Значение узла `g` возвращается методом value().|  
  
 **Пример 4**  
  
 Вот запрос, в котором предложение FLWOR используется внутри предложения exist(). (Имя FLWOR образуется из 5 предложений, которые могут составлять выражение FLWOR в XQuery: for, let, where, order by и return.)  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('  
  For $x in /a/b/c/d/e  
  Where $x/f = "SQL"  
  Return $x/g  
') = 1  
```  
  
 В следующей таблице показаны узлы, которые должны быть индексированы для того, чтобы данный запрос мог использовать селективный XML-индекс.  
  
|Узел, который необходимо включить в индекс|Причина для индексирования этого узла|  
|----------------------------------|-----------------------------------|  
|**/a/b/c/d/e**|Наличие узла `e` вычисляется в предложении FLWOR.|  
|**/a/b/c/d/e/f**|Значение узла `f` вычисляется в предложении FLWOR.|  
|**/a/b/c/d/e/g**|Наличие узла `g` вычисляется методом exist().|  
  
  
##  <a name="specifying-optimization-hints"></a><a name="hints"></a> Задание указаний по оптимизации  
 Необязательные указания оптимизации вы можете использовать для указания дополнительных подробностей сопоставления для узла, индексированного селективным XML-индексом. Например, вы можете указать тип данных, количество элементов узла, а также некоторые сведения о структуре данных. Эта дополнительная информация поддерживает более эффективное сопоставление. Она также позволяет получить улучшение производительности, эффективности хранения или и того и другого.  
  
 Использование указаний оптимизации необязательно. Всегда вы можете принять сопоставления по умолчанию, которые являются надежными, но не обеспечивают оптимальную производительность и эффективность хранения.  
  
 Некоторые указания по оптимизации (например, указание SINGLETON) накладывают ограничения на данные. В некоторых случаях, когда эти ограничения не выполняются, могут возникать ошибки.  
  
### <a name="benefits-of-optimization-hints"></a>Преимущества использования указаний оптимизации  
 В следующей таблице приведены указания по оптимизации, которые поддерживают более эффективное хранение или улучшение в производительности.  
  
|Указание по оптимизации|Более эффективное хранение|повышение производительности.|  
|-----------------------|----------------------------|--------------------------|  
|**node()**|Да|Нет|  
|**SINGLETON**|Нет|Да|  
|**DATA TYPE**|Да|Да|  
|**MAXLENGTH**|Да|Да|  
  
### <a name="optimization-hints-and-data-types"></a>Указания по оптимизации и типы данных  
 Узлы вы можете индексировать как типы данных XQuery или как типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . В следующей таблице показано, какие указания оптимизации поддерживаются для каждого типа данных.  
  
|Указание по оптимизации|Типы данных XQuery|Типы данных SQL|  
|-----------------------|-----------------------|--------------------|  
|**node()**|Да|Нет|  
|**SINGLETON**|Да|Да|  
|**DATA TYPE**|Да|Нет|  
|**MAXLENGTH**|Да|Нет|  
  
### <a name="node-optimization-hint"></a>Указание по оптимизации node()  
 Область применения: Типы данных XQuery  
  
 Вы можете использовать оптимизацию node(), чтобы указать узел, значение которого не требуется для вычисления обычного запроса. Это указание снижает требования к хранилищу, поскольку обычному запросу необходимо вычислить только существование узла. (По умолчанию селективный XML-индекс хранит значения всех повышенных узлов, за исключением сложных типов узлов.)  
  
 Рассмотрим следующий пример:  
  
```sql  
SELECT T.record FROM myXMLTable T  
WHERE T.xmldata.exist('/a/b[./c=5]') = 1  
```  
  
 Для использования селективного XML-индекса для оценки этого запроса повысьте узлы `b` и `c`. Однако, поскольку значение узла `b` не требуется, вы можете использовать указание node() со следующим синтаксисом:  
  
 `/a/b/ as node()`  
  
 Если для запроса необходимо значение узла, который был индексирован с указанием node(), селективный XML-индекс использовать нельзя.  
  
### <a name="singleton-optimization-hint"></a>Указание по оптимизации SINGLETON  
 Область применения: Типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или XQuery  
  
 Указание по оптимизации SINGLETON указывает количество элементов узла. Это указание повышает производительность запросов, поскольку известно, что узел отображается максимум один раз в его родителе или предке.  
  
 Рассмотрим [образец XML-документа](#sample) в этом разделе.  
  
 Для использования селективного XML-индекса в запросе к этому документу вы можете задать указание SINGLETON для узла `d` , поскольку он встречается максимум один раз в его родительском элементе.  
  
 Если было задано указание SINGLETON, а узел встречается более одного раза в его родителе или предке, то при создании индекса (для существующих данных) или при выполнении запроса (для новых данных) возникнет ошибка.  
  
### <a name="data-type-optimization-hint"></a>Указание по оптимизации DATA TYPE  
 Область применения: Типы данных XQuery  
  
 Указание по оптимизации DATA TYPE позволяет задать тип данных XQuery или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для индексированного узла. Тип данных используется для столбца в таблице данных селективного XML-индекса, соответствующего индексированному узлу.  
  
 Если приведение существующего значения в указанный тип данных завершается ошибкой, операция вставки (в индекс) не завершается ошибкой, однако в таблицу данных индекса вставляется значение NULL.  
  
### <a name="maxlength-optimization-hint"></a>Указание по оптимизации MAXLENGTH  
 Область применения: Типы данных XQuery  
  
 Указание по оптимизации MAXLENGTH позволяет ограничить длину данных xs:string. Указание MAXLENGTH несущественно для типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , поскольку длина указывается при указании типов данных VARCHAR или NVARCHAR.  
  
 Если существующая строка длиннее значения MAXLENGTH, вставка этого значения в индекс завершается с ошибкой.  
  
  
##  <a name="sample-xml-document-for-examples"></a><a name="sample"></a> Образец XML-документа для примеров  
 Следующий образец XML-документа указывается в примерах этого раздела.  
  
```xml  
<a>  
    <b>  
         <c atc="aa">10</c>  
         <c atc="bb">15</c>  
         <d atd1="dd" atd2="ddd">md </d>  
    </b>  
     <b>  
        <c></c>  
        <c atc="">117</c>  
     </b>  
</a>  
```  
  
  
## <a name="see-also"></a>См. также:  
 [Выборочный XML-индекс (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [Создание, изменение и удаление селективных XML-индексов](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)  
  
  
