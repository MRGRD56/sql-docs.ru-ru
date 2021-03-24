---
description: Иерархические данные (SQL Server)
title: Иерархические данные (SQL Server) | Документация Майкрософт
ms.custom: ''
ms.date: 10/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchies [SQL Server], tables to support
- hierarchyid [Database Engine], concepts
- hierarchical tables [Database Engine]
- SqlHierarchyId
- hierarchyid [Database Engine]
- hierarchical queries [SQL Server], using hierarchyid data type
ms.assetid: 19aefa9a-fbc2-4b22-92cf-67b8bb01671c
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7fcefb597fe97e90bfda78fe934c83646414c23f
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611184"
---
# <a name="hierarchical-data-sql-server"></a>Иерархические данные (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

  Встроенный тип данных **hierarchyid** упрощает хранение и запрос иерархических данных. **hierarchyid** оптимизирован для представления деревьев, которые являются наиболее распространенным типом иерархических данных.  
  
 Иерархические данные представляют собой набор элементов данных, связанных между собой иерархическими связями. Иерархические связи — это связи, в которых один из элементов данных является родителем другого элемента. Примеры иерархических данных, которые обычно хранятся в базах данных, включают следующее.  
  
-   Организационная структура  
  
-   Файловая система  
  
-   группа задач в проекте;  
  
-   Классификация языковых терминов  
  
-   Диаграмма связей между веб-страницами  
  
 Тип данных [hierarchyid](../t-sql/data-types/hierarchyid-data-type-method-reference.md) используется для создания таблиц с иерархической структурой или для описания иерархической структуры данных, которые хранятся в другом расположении. Для запросов и управления иерархическими данными следует использовать [функции hierarchyid](../t-sql/data-types/hierarchyid-data-type-method-reference.md) в [!INCLUDE[tsql](../includes/tsql-md.md)] .  
  
##  <a name="key-properties-of-hierarchyid"></a><a name="keyprops"></a> Основные свойства типа hierarchyid  
 Значение типа данных **hierarchyid** представляет позицию в древовидной иерархии. Значения **hierarchyid** обладают следующими свойствами.  
  
-   Исключительная компактность  
  
     Среднее число бит, необходимое для представления узла в древовидной структуре с *n* узлами, зависит от среднего количества потомков у узла. Для структур с низкой степенью ветвления (0-7) объем занимаемой памяти равен примерно 6\*logA *n* бит, где A — среднее ветвление. Для представления узла в иерархии организации, насчитывающей 100 000 человек со средним уровнем ветвления 6, необходимо около 38 бит. Эта величина округляется до 40 бит (5 байт), которые необходимы для хранения.  
  
-   Сравнение проводится в порядке приоритета глубины  
  
     Если заданы два значения **hierarchyid** — **a** и **b**, **a<b** означает, что значение a появляется раньше значения b, если проходить по дереву с приоритетным направлением в глубину. Индексы для типов данных **hierarchyid** располагаются в порядке приоритета глубины, а узлы, встречающиеся рядом при проходе по дереву с приоритетным направлением глубины, хранятся рядом друг с другом. Например, потомки некоторой записи хранятся рядом с этой записью.  
  
-   Поддержка произвольных вставок и удалений  
  
     С помощью метода [GetDescendant](../t-sql/data-types/getdescendant-database-engine.md) можно в любой момент создать одноуровневый элемент, расположенный справа от заданного узла, слева от заданного узла или между любыми двумя другими одноуровневыми элементами. Свойство сравнения сохраняется, если произвольное число узлов вставляется в иерархию или удаляется из нее. Большинство операций вставки и удаления сохраняют свойство компактности. Однако операции вставки между двумя узлами приводят к созданию значений hierarchyid, обладающих менее компактным представлением.  
  
  
##  <a name="limitations-of-hierarchyid"></a><a name="limits"></a> Ограничения типа данных hierarchyid  
 Тип данных **hierarchyid** имеет следующие ограничения.  
  
-   Столбец типа **hierarchyid** не принимает древовидную структуру автоматически. Приложение должно создать и назначить значения **hierarchyid** таким образом, чтобы они отражали требуемые связи между строками. Некоторые приложения могут содержать столбец типа **hierarchyid** , указывающий местоположение в иерархии, определенной в другой таблице.  
  
-   Параллельными процессами создания и присвоения значений **hierarchyid** управляет само приложение. Нет никакой гарантии, что значения **hierarchyid** уникальны, если приложение не использует ограничение уникального ключа или не обеспечивает уникальность своей логикой.  
  
-   Иерархические связи, представленные значениями типа **hierarchyid** , не обеспечиваются и не проверяются, как связи по внешнему ключу. Можно, а иногда и удобно иметь иерархическую связь, в которой у A есть потомок B и когда A удаляется, у B остается связь с несуществующей записью. Если это неприемлемо, приложение должно запросить потомков, прежде чем удалять родителей.  
  
  
##  <a name="when-to-use-alternatives-to-hierarchyid"></a><a name="alternatives"></a> Использование других способов представления иерархических данных  
 Кроме типа **hierarchyid** , есть еще два способа представления иерархических данных:  
  
-   Родители-потомки  
  
-   XML  
  
 Использование типа **hierarchyid** дает больше возможностей. Однако есть ситуации (см. ниже), когда другие методы более эффективны.  
  
### <a name="parentchild"></a>Родители-потомки  
 Если используется подход «родители-потомки», в каждой строке содержится ссылка на родительскую переменную. Следующая таблица определяет типичную таблицу для хранения строк в связи типа «родители-потомки».  
  
```sql
USE AdventureWorks2012 ;  
GO  
  
CREATE TABLE ParentChildOrg  
   (  
    BusinessEntityID int PRIMARY KEY,  
    ManagerId int REFERENCES ParentChildOrg(BusinessEntityID),  
    EmployeeName nvarchar(50)   
   ) ;  
GO  
```  
  
 Сравнение подхода "родители-потомки" и **hierarchyid** для распространенных операций  
  
-   Запросы по поддеревьям выполняются значительно быстрее, если используется тип **hierarchyid**.  
  
-   Запросы по прямым потомкам обрабатываются немного медленнее, если используется тип **hierarchyid**.  
  
-   Операции по перемещению узлов типа **hierarchyid**, не являющихся конечными, выполняются медленнее.  
  
-   Вставка неконечных узлов и вставка или перемещение конечных узлов имеют для типа данных **hierarchyid** одинаковую сложность.  
  
 Метод «родители-потомки» может оказаться более эффективным в следующих случаях.  
  
-   Размер ключа очень важен. При одинаковом количестве узлов значение типа **hierarchyid** занимает столько же или больше памяти, чем значение одного из целочисленных типов (**smallint**, **int**, **bigint**). По этой причине можно использовать метод "родители-потомки", но только в редких случаях, так как тип **hierarchyid** обеспечивает более эффективное использование памяти при операциях ввода-вывода и требует меньше команд ЦП по сравнению со структурами типа "родители-потомки".  
  
-   Запросы редко бывают обращены сразу к нескольким частям иерархии. Иными словами, запрос обычно касается только одной точки иерархической структуры. В этих случаях хранение данных в одном месте не имеет значения. Например, метод «родители-потомки» предпочтителен в случае, если таблица организаций используется только для обработки заработной платы отдельных работников.  
  
-   Структура неконечных поддеревьев часто меняется, и очень важна производительность. В иерархической структуре «родители-потомки» изменение местоположения строки иерархии касается только одной строки. Если используется тип **hierarchyid** , изменение местоположения строки влияет на *n8* строк, где *n* — количество узлов в перемещаемом поддереве.  
  
     Если поддерево без конечных узлов необходимо часто перемещать и важна высокая производительность, но при этом большинство перемещений происходит на определенном уровне иерархии, рекомендуется разбить данные на две иерархические структуры: более высоких и более низких уровней. В этом случае все перемещения будут происходить на последнем уровне верхней иерархии. Например, рассмотрим иерархию веб-сайтов, входящих в одну службу. Сайты содержат много страниц, имеющих иерархическую структуру. Сайты службы могут перемещаться в другие места в иерархии сайтов, но реорганизация страниц происходит редко. Это можно представить так:  
  
    ```sql
    CREATE TABLE HostedSites   
       (  
        SiteId hierarchyid, PageId hierarchyid  
       ) ;  
    GO  
    ```  
  
  
### <a name="xml"></a>XML  
 XML-документ является деревом, поэтому один экземпляр типа данных XML может представлять всю структуру. В [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] при создании XML-индекса для внутреннего использования применяются значения типа **hierarchyid**, которые обозначают положение внутри иерархии.  
  
 Использование типа данных XML может быть выгодно в следующих случаях.  
  
-   Всегда хранится и извлекается вся структура данных.  
  
-   Приложение обрабатывает данные в формате XML.  
  
-   Поиск с помощью предикатов происходит крайне редко, и производительность здесь не критична.  
  
 Например, если приложение отслеживает работу многочисленных организаций, всегда хранит и извлекает всю организационную иерархию и не запрашивает подробные данные по отдельным организациям, таблица может иметь следующий вид.  
  
```sql
CREATE TABLE XMLOrg   
    (  
    Orgid int,  
    Orgdata xml  
    ) ;  
GO  
```  
  
  
##  <a name="indexing-strategies-for-hierarchical-data"></a><a name="indexing"></a> Ниже описаны подходы к индексированию иерархических данных:  
 Есть два подхода к индексированию иерархических данных:  
  
-   **В глубину**  
  
     В индексе преимущественно в глубину строки поддерева хранятся рядом друг с другом. Например, записи всех сотрудников, в цепи подчиненности которых есть данный руководитель, хранятся рядом с записью руководителя.  
  
     В индексе преимущественно в глубину все узлы поддерева узла хранятся вместе. Поэтому индекс преимущественно в глубину эффективен для обработки запросов по поддеревьям. Например, «найти все файлы в этой папке и ее подкаталогах».  
  
-   **В ширину**  
  
     Если используется индексирование в ширину, строки одного уровня иерархии хранятся вместе. Например, записи всех сотрудников, напрямую подчиненных одному и тому же руководителю, хранятся рядом друг с другом.  
  
     В индексе преимущественно в ширину все прямые потомки узла хранятся в одном месте. Поэтому индекс преимущественно в ширину эффективен для запросов по прямым потомкам. Например: «найти всех прямых подчиненных этого начальника».  
  
 Выбор стратегии индексирования (в глубину, в ширину или обе) и ключа кластеризации зависит от того, какие из вышеуказанных типов запросов обрабатываются чаще и какие операции более важны (SELECT или DML). Пример использования стратегий индексирования см. в разделе [Tutorial: Using the hierarchyid Data Type](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md).  
  
  
### <a name="creating-indexes"></a>Создание индексов  
 Для организации данных в ширину можно использовать метод GetLevel(). В следующем примере создаются оба типа индекса: преимущественно в глубину и преимущественно в ширину.  
  
```sql
USE AdventureWorks2012 ;   -- wmimof
GO  
  
CREATE TABLE Organization  
   (  
    BusinessEntityID hierarchyid,  
    OrgLevel as BusinessEntityID.GetLevel(),   
    EmployeeName nvarchar(50) NOT NULL  
   ) ;  
GO  
  
CREATE CLUSTERED INDEX Org_Breadth_First   
    ON Organization(OrgLevel,BusinessEntityID) ;  
GO  
  
CREATE UNIQUE INDEX Org_Depth_First   
    ON Organization(BusinessEntityID) ;  
GO  
```  
  
  
## <a name="examples"></a>Примеры  
  
### <a name="simple-example"></a>Простой пример  
 Следующий пример намеренно упрощен, чтобы легче было приступить к работе. Сначала создайте таблицу для хранения определенных географических данных.  
  
```sql
CREATE TABLE SimpleDemo  
(
    Level hierarchyid NOT NULL,  
    Location nvarchar(30) NOT NULL,  
    LocationType nvarchar(9) NULL
);
```  
  
 Теперь введите данные для некоторых континентов, стран, штатов и городов.  
  
```sql
INSERT SimpleDemo  
    VALUES   
('/1/', 'Europe', 'Continent'),  
('/2/', 'South America', 'Continent'),  
('/1/1/', 'France', 'Country'),  
('/1/1/1/', 'Paris', 'City'),  
('/1/2/1/', 'Madrid', 'City'),  
('/1/2/', 'Spain', 'Country'),  
('/3/', 'Antarctica', 'Continent'),  
('/2/1/', 'Brazil', 'Country'),  
('/2/1/1/', 'Brasilia', 'City'),  
('/2/1/2/', 'Bahia', 'State'),  
('/2/1/2/1/', 'Salvador', 'City'),  
('/3/1/', 'McMurdo Station', 'City');  
```  
  
 Выберите данные, добавляя столбец, который преобразовывает данные уровня в текстовое значение, удобное для восприятия. Этот запрос также отсортирует результат по типу данных **hierarchyid** .  
  
```sql
SELECT CAST(Level AS nvarchar(100)) AS [Converted Level], *   
    FROM SimpleDemo ORDER BY Level;  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```  
Converted Level  Level     Location         LocationType  
/1/              0x58      Europe           Continent  
/1/1/            0x5AC0    France           Country  
/1/1/1/          0x5AD6    Paris            City  
/1/2/            0x5B40    Spain            Country  
/1/2/1/          0x5B56    Madrid           City  
/2/              0x68      South America    Continent  
/2/1/            0x6AC0    Brazil           Country  
/2/1/1/          0x6AD6    Brasilia         City  
/2/1/2/          0x6ADA    Bahia            State  
/2/1/2/1/        0x6ADAB0  Salvador         City  
/3/              0x78      Antarctica       Continent  
/3/1/            0x7AC0    McMurdo Station  City  
```  
  
 Обратите внимание, что иерархия имеет допустимую структуру, несмотря на отсутствие внутреннего согласования. Байя — единственный штат. Он отображается в иерархии как одноранговый по отношению к городу Бразилиа. Аналогично полярная станция Макмердо не имеет родительской страны. Необходимо решить, подходит ли этот тип иерархии для использования.  
  
 Добавьте еще одну строку и выберите результаты.  
  
```sql
INSERT SimpleDemo  
    VALUES ('/1/3/1/', 'Kyoto', 'City'), ('/1/3/1/', 'London', 'City');  
SELECT CAST(Level AS nvarchar(100)) AS [Converted Level], * FROM SimpleDemo ORDER BY Level;  
```  
  
 Это демонстрирует наличие других возможных проблем. Киото можно ввести в качестве уровня `/1/3/1/` даже при отсутствии родительского уровня `/1/3/`. И Лондон и Киото имеют одинаковое значение свойства **hierarchyid**. Опять-таки пользователи должны решить, подходит ли этот тип иерархии для использования, и заблокировать значения, недопустимые для использования.  
  
 Кроме того, в этой таблице не используется верхняя часть иерархии `'/'`. Она была опущена, потому что общий родительский объект для всех континентов отсутствует. Его можно добавить путем добавления всей планеты.  
  
```sql
INSERT SimpleDemo  
    VALUES ('/', 'Earth', 'Planet');  
```  
  
##  <a name="related-tasks"></a><a name="tasks"></a> Связанные задачи  
  
###  <a name="migrating-from-parentchild-to-hierarchyid"></a><a name="migrating"></a> Переход со структуры «родители-потомки» на тип hierarchyid  
 Большинство деревьев представлены методом «родители-потомки». Для перехода со структуры "родители-потомки" на тип **hierarchyid** проще всего создать временный столбец или временную таблицу для хранения количества узлов на каждом уровне иерархии. Пример преобразования таблицы с организацией "родители-потомки" см. в статье [Учебник. Использование типа данных hierarchyid](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md)(урок 1).  
  
  
###  <a name="managing-a-tree-using-hierarchyid"></a><a name="BKMK_ManagingTrees"></a> Управление древовидной структурой с помощью hierarchyid  
 Хотя столбец **hierarchyid** не обязательно представляет дерево, приложение легко может обеспечить это.  
  
-   При формировании новых значений выполните одно из следующих действий.  
  
    -   Следите за последним номером потомка в строке родитель.  
  
    -   Вычислите последнего потомка. Для эффективного вычисления требуется наличие индекса по ширине.  
  
-   Обеспечьте уникальность, создав для столбца уникальный индекс, возможно, как часть ключа кластеризации. Чтобы обеспечить уникальность вставляемых значений, выполните одно из следующих действий.  
  
    -   Определяйте нарушения уникальных ключей и проводите повторные попытки.  
  
    -   Определите уникальность каждого нового дочернего узла перед его вставкой с использованием сериализуемой транзакции.  
  
  
#### <a name="example-using-error-detection"></a>Пример использования обнаружения ошибок  
 В следующем примере образец кода вычисляет значение нового потомка **EmployeeId** , определяет любое нарушение ключа, а затем возвращается к маркеру **INS_EMP** для повторного вычисления значения новой строки **EmployeeId** :  
  
```sql
USE AdventureWorks ;  
GO  
  
CREATE TABLE Org_T1  
   (  
    EmployeeId hierarchyid PRIMARY KEY,  
    OrgLevel AS EmployeeId.GetLevel(),  
    EmployeeName nvarchar(50)   
   ) ;  
GO  
  
CREATE INDEX Org_BreadthFirst ON Org_T1(OrgLevel, EmployeeId);
GO  
  
CREATE PROCEDURE AddEmp(@mgrid hierarchyid, @EmpName nvarchar(50) )   
AS  
BEGIN  
    DECLARE @last_child hierarchyid;
INS_EMP:   
    SELECT @last_child = MAX(EmployeeId) FROM Org_T1   
        WHERE EmployeeId.GetAncestor(1) = @mgrid;
    INSERT INTO Org_T1 (EmployeeId, EmployeeName)  
        SELECT @mgrid.GetDescendant(@last_child, NULL), @EmpName;
-- On error, return to INS_EMP to recompute @last_child  
IF @@error <> 0 GOTO INS_EMP   
END ;  
GO  
```  
  
  
#### <a name="example-using-a-serializable-transaction"></a>Пример использования сериализуемой транзакции  
 Индекс **Org_BreadthFirst** обеспечивает использование поиска по диапазону для определения значения **\@last_child**. В дополнение к другим случаям ошибок, проверка которых может потребоваться приложению, нарушение повторяющихся ключей после вставки может свидетельствовать о попытке добавления нескольких сотрудников с одним и тем же идентификатором; вследствие этого вычисление значения **\@last_child** должно быть выполнено повторно. Следующий код вычислит новое значение узла в сериализуемой транзакции:  
  
```sql
CREATE TABLE Org_T2  
    (  
    EmployeeId hierarchyid PRIMARY KEY,  
    LastChild hierarchyid,   
    EmployeeName nvarchar(50)   
    ) ;  
GO  
  
CREATE PROCEDURE AddEmp(@mgrid hierarchyid, @EmpName nvarchar(50))   
AS  
BEGIN  
DECLARE @last_child hierarchyid  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE  
BEGIN TRANSACTION   
  
SELECT @last_child  =  EmployeeId.GetDescendant(LastChild,NULL)
FROM Org_T2
WHERE EmployeeId = @mgrid

UPDATE Org_T2 SET LastChild = @last_child  WHERE EmployeeId = @mgrid

INSERT Org_T2 (EmployeeId, EmployeeName)   
    VALUES(@last_child, @EmpName)  
COMMIT  
END ;  
```  
  
 Следующий код заполняет таблицу тремя строками и возвращает результаты:  
  
```sql
INSERT Org_T2 (EmployeeId, EmployeeName)   
    VALUES(hierarchyid::GetRoot(), 'David') ;  
GO  
AddEmp 0x , 'Sariya'  
GO  
AddEmp 0x58 , 'Mary'  
GO  
SELECT * FROM Org_T2  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```  
EmployeeId LastChild EmployeeName  
---------- --------- ------------  
0x        0x58       David  
0x58      0x5AC0     Sariya  
0x5AC0    NULL       Mary  
```  
  
  
###  <a name="enforcing-a-tree"></a><a name="BKMK_EnforcingTrees"></a> Принудительное формирование древовидной структуры  
 Приведенный выше пример показывает, как можно использовать приложение для поддержания целостности дерева. Обеспечить целостность дерева с помощью ограничений можно, создав для вычисляемого столбца, который определяет родителя для каждого узла, ограничение внешнего ключа относительно идентификатора первичного ключа.  
  
```sql
CREATE TABLE Org_T3  
(  
   EmployeeId hierarchyid PRIMARY KEY,  
   ParentId AS EmployeeId.GetAncestor(1) PERSISTED    
      REFERENCES Org_T3(EmployeeId),  
   LastChild hierarchyid,   
   EmployeeName nvarchar(50)  
)  
GO  
```  
  
 Этот метод обеспечения взаимосвязи предпочтительней в случае, если прямой DML-доступ к таблице имеет код, который не в состоянии обеспечить целостность ее иерархического дерева. Однако этот метод может снизить производительность, поскольку ограничение необходимо проверять при каждой DML-операции.  
  
  
###  <a name="finding-ancestors-by-using-the-clr"></a><a name="findclr"></a> Поиск предков с помощью среды CLR  
 Одной из частых операций, в которых участвуют два узла из иерархии, является нахождение ближайшего общего предка. Эта операция может быть описана в среде [!INCLUDE[tsql](../includes/tsql-md.md)] либо в среде CLR, поскольку тип данных **hierarchyid** доступен в обеих. Рекомендуется использовать среду CLR, поскольку она обеспечивает большую производительность.  
  
 Используйте следующий код CLR, чтобы найти список предков и ближайшего общего предка:  
  
```csharp
using System;  
using System.Collections;  
using System.Text;  
using Microsoft.SqlServer.Server;  // SqlFunction Attribute
using Microsoft.SqlServer.Types;   // SqlHierarchyId
  
public partial class HierarchyId_Operations  
{  
    [SqlFunction(FillRowMethodName = "FillRow_ListAncestors")]
    public static IEnumerable ListAncestors(SqlHierarchyId h)
    {  
        while (!h.IsNull)  
        {  
            yield return (h);  
            h = h.GetAncestor(1);  
        }  
    }  
    public static void FillRow_ListAncestors(
        Object obj,
        out SqlHierarchyId ancestor
        )
    {  
        ancestor = (SqlHierarchyId)obj;  
    }  
  
    public static HierarchyId CommonAncestor(
        SqlHierarchyId h1,
        HierarchyId h2
        )  
    {  
        while (!h1.IsDescendantOf(h2))  
            h1 = h1.GetAncestor(1);  
  
        return h1;  
    }  
}  
```  
  
 Чтобы получить возможность использовать методы **ListAncestor** и **CommonAncestor** в следующих примерах [!INCLUDE[tsql](../includes/tsql-md.md)] , постройте библиотеки DLL и создайте сборку **HierarchyId_Operations** в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , выполнив код, аналогичный следующему:  
  
```sql
CREATE ASSEMBLY HierarchyId_Operations   
    FROM '<path to DLL>\ListAncestors.dll';
GO  
```  
  
  
###  <a name="listing-ancestors"></a><a name="ancestors"></a> Перечисление предков  
 Создание списка предков узла — это распространенная операция, использующаяся, например, для демонстрации позиции в организации. Одним из способов ее реализации является применение возвращающей табличное значение функции, использующей класс **HierarchyId_Operations** , определенный выше:  
  
 Использование среды [!INCLUDE[tsql](../includes/tsql-md.md)]:  
  
```sql
CREATE FUNCTION ListAncestors (@node hierarchyid)  
RETURNS TABLE (node hierarchyid)  
AS  
EXTERNAL NAME HierarchyId_Operations.HierarchyId_Operations.ListAncestors  
GO  
```  
  
 Пример использования:  
  
```sql
DECLARE @h hierarchyid  
SELECT @h = OrgNode   
FROM HumanResources.EmployeeDemo    
WHERE LoginID = 'adventure-works\janice0' -- /1/1/5/2/  
  
SELECT LoginID, OrgNode.ToString() AS LogicalNode  
FROM HumanResources.EmployeeDemo AS ED  
JOIN ListAncestors(@h) AS A   
   ON ED.OrgNode = A.Node  
GO  
```  
  
  
###  <a name="finding-the-lowest-common-ancestor"></a><a name="lowestcommon"></a> Нахождение ближайшего общего предка  
 С помощью класса **HierarchyId_Operations** , определенного выше, создайте следующую функцию [!INCLUDE[tsql](../includes/tsql-md.md)] для нахождения ближайшего общего предка, затрагивающую два узла в иерархии:  
  
```sql
CREATE FUNCTION CommonAncestor (@node1 hierarchyid, @node2 hierarchyid)  
RETURNS hierarchyid  
AS  
EXTERNAL NAME HierarchyId_Operations.HierarchyId_Operations.CommonAncestor  
GO  
```  
  
 Пример использования:  
  
```sql
DECLARE @h1 hierarchyid, @h2 hierarchyid;
  
SELECT @h1 = OrgNode   
FROM  HumanResources.EmployeeDemo   
WHERE LoginID = 'adventure-works\jossef0'; -- Node is /1/1/3/  
  
SELECT @h2 = OrgNode   
FROM HumanResources.EmployeeDemo    
WHERE LoginID = 'adventure-works\janice0'; -- Node is /1/1/5/2/  
  
SELECT OrgNode.ToString() AS LogicalNode, LoginID   
FROM HumanResources.EmployeeDemo    
WHERE OrgNode = dbo.CommonAncestor(@h1, @h2) ;  
```  
  
 Результирующий узел — /1/1/  
  
  
###  <a name="moving-subtrees"></a><a name="BKMK_MovingSubtrees"></a> Перемещение поддеревьев  
 Другой распространенной операцией является перемещение поддеревьев. Описанная ниже процедура берет поддерево узла **\@oldMgr** и делает его (включая сам узел **\@oldMgr**) поддеревом узла **\@newMgr**.  
  
```sql
CREATE PROCEDURE MoveOrg(@oldMgr nvarchar(256), @newMgr nvarchar(256) )  
AS  
BEGIN  
DECLARE @nold hierarchyid, @nnew hierarchyid  
SELECT @nold = OrgNode FROM HumanResources.EmployeeDemo WHERE LoginID = @oldMgr ;  
  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE  
BEGIN TRANSACTION  
SELECT @nnew = OrgNode FROM HumanResources.EmployeeDemo WHERE LoginID = @newMgr ;  
  
SELECT @nnew = @nnew.GetDescendant(max(OrgNode), NULL)   
FROM HumanResources.EmployeeDemo WHERE OrgNode.GetAncestor(1)=@nnew ;  
  
UPDATE HumanResources.EmployeeDemo    
SET OrgNode = OrgNode.GetReparentedValue(@nold, @nnew)  
WHERE OrgNode.IsDescendantOf(@nold) = 1 ;  
  
COMMIT TRANSACTION;
END ;  
GO  
```  
  
  
## <a name="see-also"></a>См. также  
 [Справочник по методам типа данных hierarchyid](../t-sql/data-types/hierarchyid-data-type-method-reference.md)   
 [Руководство. Использование типа данных hierarchyid](../relational-databases/tables/tutorial-using-the-hierarchyid-data-type.md)   
 [hierarchyid (Transact-SQL)](../t-sql/data-types/hierarchyid-data-type-method-reference.md)  
  
