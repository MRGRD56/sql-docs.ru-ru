---
title: Внедрение кода SQL | Документация Майкрософт
description: Узнайте, как работают атаки путем внедрения кода SQL. Такие атаки устраняются путем проверки входных данных и кода для внедрения кода SQL в SQL Server.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Injection
ms.assetid: eb507065-ac58-4f18-8601-e5b7f44213ab
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2678b0eda377060e45a30cf47d4fa7e780e4a595
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739834"
---
# <a name="sql-injection"></a>внедрение кода SQL;
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Внедрение кода SQL — это атака, во время которой вредоносный код вставляется в строки, которые позже будут переданы на экземпляр SQL Server для анализа и выполнения. Любая процедура, создающая инструкции SQL, должна рассматриваться на предмет уязвимости к внедрению кода, так как SQL Server выполняет все получаемые синтаксически правильные запросы. Даже параметризованные данные могут стать предметом манипуляций опытного злоумышленника.  
  
## <a name="how-sql-injection-works"></a>Принцип действия атаки путем внедрения кода SQL  
 Основная форма атаки SQL Injection состоит в прямой вставке кода в пользовательские входные переменные, которые объединяются с командами SQL и выполняются. Менее явная атака внедряет небезопасный код в строки, предназначенные для хранения в таблице или в виде метаданных. Когда впоследствии сохраненные строки объединяются с динамической командой SQL, происходит выполнение небезопасного кода.  
  
 Атака осуществляется посредством преждевременного завершения текстовой строки и присоединения к ней новой команды. Поскольку к вставленной команде перед выполнением могут быть добавлены дополнительные строки, злоумышленник заканчивает внедряемую строку меткой комментария «--». Весь последующий текст во время выполнения не учитывается.  
  
 Следующий скрипт показывает простую атаку SQL Injection. Скрипт формирует SQL-запрос, выполняя объединение жестко запрограммированных строк со строкой, введенной пользователем:  
  
```csharp
var Shipcity;  
ShipCity = Request.form ("ShipCity");  
var sql = "select * from OrdersTable where ShipCity = '" + ShipCity + "'";  
```  
  
 Пользователю выводится запрос на ввод названия города. Если пользователь вводит `Redmond`, то запрос, построенный с помощью скрипта, выглядит приблизительно так:  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond'  
```  
  
 Предположим, однако, что пользователь вводит следующее:  
  
```sql
Redmond'; drop table OrdersTable--  
```  
  
 В этом случае запрос, построенный скриптом, будет следующим:  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond';drop table OrdersTable--'  
```  
  
 Точка с запятой «;» обозначает конец одного запроса и начало другого. А последовательность двух дефисов (--) означает, что остальная часть текущей строки является комментарием и не должна обрабатываться. Если измененный код будет синтаксически правилен, то он будет выполнен сервером. Когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет обрабатывать эту инструкцию, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] прежде всего отберет все записи в `OrdersTable` , где `ShipCity` является `Redmond`. Затем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] удалит `OrdersTable`.  
  
 Если вставленный код SQL синтаксически верен, искаженные данные нельзя выявить программно. Поэтому необходимо проверять правильность всех вводимых пользователями данных, а также внимательно просматривать код, выполняющий созданные с помощью SQL команды на сервере. Рекомендуемые приемы программирования описываются в следующих подразделах этого раздела.  
  
## <a name="validate-all-input"></a>Проверка достоверности всех вводимых данных  
 Всегда проверяйте все данные, вводимые пользователем, выполняя проверку типа, длины, формата и диапазона данных. При реализации мер предосторожности, направленных против злонамеренного ввода данных, учитывайте архитектуру и сценарии развертывания приложения. Помните, что программы, созданные для работы в безопасной среде, могут быть скопированы в небезопасную среду. Рекомендуется следующая стратегия:  
  
-   Не делайте никаких предположений о размере, типе или содержимом данных, получаемых приложением. Например, рекомендуется оценить следующее.  
  
    -   Как приложение будет вести себя, если пользователь по ошибке или по злому умыслу вставит MPEG-файл размером 10 МБ там, где приложение ожидает ввод почтового индекса?  
  
    -   Как приложение будет вести себя, если в текстовое поле будет внедрена инструкция `DROP TABLE` ?  
  
-   Проверьте размер и тип вводимых данных и установите соответствующие ограничения. Это поможет предотвратить преднамеренное переполнение буфера.  
  
-   Проверяйте содержимое строковых переменных и допускайте только ожидаемые значения. Отклоняйте записи, содержащие двоичные данные, управляющие последовательности и символы комментария. Это поможет предотвратить внедрение скрипта и защитит от некоторых приемов атаки, использующих переполнение буфера.  
  
-   При работе с XML-документами проверяйте все вводимые данные на соответствие схеме.  
  
-   Никогда не создавайте инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] непосредственно из данных, вводимых пользователем.  
  
-   Для проверки вводимых пользователем данных используйте хранимые процедуры.  
  
-   В многоуровневых средах перед передачей в доверенную зону должны проверяться все данные. Данные, не прошедшие процесс проверки, следует отклонять и возвращать ошибку на предыдущий уровень.  
  
-   Внедрите многоэтапную проверку достоверности. Меры предосторожности, предпринятые против случайных пользователей-злоумышленников, могут оказаться неэффективными против организаторов преднамеренных атак. Рекомендуется проверять данные, вводимые через пользовательский интерфейс, и далее во всех последующих точках пересечения границ доверенной зоны.   
    Например, проверка данных в клиентском приложении может предотвратить простое внедрение скрипта. Однако если следующий уровень предполагает, что вводимые данные уже были проверены, то любой злоумышленник, которому удастся обойти клиентскую систему, сможет получить неограниченный доступ к системе.  
  
-   Никогда не объединяйте введенные пользователем данные без проверки. Объединение строк является основной точкой входа для внедрения скрипта.  
  
-   Не допускайте использование в полях следующих строк, из которых могут быть созданы имена файлов: AUX, CLOCK$, COM1–COM8, CON, CONFIG$, LPT1–LPT8, NUL и PRN.  
  
 По возможности отклоняйте вводимые данные, содержащие следующие символы:  
  
|Входной символ|Значение в языке Transact-SQL|  
|---------------------|------------------------------|  
|**;**|Разделитель запросов.|  
|**'**|Разделитель строк символьных данных.|  
|**--**|Разделитель однострочного комментария. Текст после **--** и до конца этой строки не обрабатывается сервером.|  
|**/\**_ ... _*\*/**|Разделители комментариев. Сервер не обрабатывает текст между знаками **/\* *_ и _* \*/** .|  
|**xp_**|Используется в начале имени расширенных хранимых процедур каталога, например `xp_cmdshell`.|  
  
### <a name="use-type-safe-sql-parameters"></a>Использование SQL-параметров безопасных типов  
 Коллекция **Parameters** в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обеспечивает проверку длины и контроль соответствия типов. Если используется коллекция **Parameters** , то вводимые данные обрабатываются как буквенное значение, а не исполняемый код. Дополнительное преимущество использования коллекции **Parameters** состоит в том, что можно использовать принудительные проверки типа и длины данных. Если значение выходит за рамки диапазона, будет вызвано исключение. В следующем фрагменте кода демонстрируется использование коллекции **Parameters** :  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter("AuthorLogin", conn);  
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",  
     SqlDbType.VarChar, 11);  
parm.Value = Login.Text;  
```  
  
 В этом примере параметр `@au_id` обрабатывается как буквенное значение, а не исполняемый код. Это значение проверяется по типу и длине. Если значение `@au_id` не соответствует указанным ограничениям типа и длины, то будет вызвано исключение.  
  
### <a name="use-parameterized-input-with-stored-procedures"></a>Использование параметризованного ввода с хранимыми процедурами  
 Хранимые процедуры могут быть подвержены атакам SQL Injection, если они используют нефильтрованные входные данные. Например, следующий код является уязвимым:  
  
```csharp
SqlDataAdapter myCommand =   
new SqlDataAdapter("LoginStoredProcedure '" +   
                               Login.Text + "'", conn);  
```  
  
 Если используются хранимые процедуры, то в качестве их входных данных следует использовать параметры.  
  
### <a name="use-the-parameters-collection-with-dynamic-sql"></a>Использование коллекции Parameters с динамическим SQL  
 Если невозможно использовать хранимые процедуры, сохраняется возможность использования параметров, как показано в следующем примере кода:  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter(  
"SELECT au_lname, au_fname FROM Authors WHERE au_id = @au_id", conn);  
SQLParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",   
                        SqlDbType.VarChar, 11);  
Parm.Value = Login.Text;  
```  
  
### <a name="filtering-input"></a>Фильтрация ввода  
 Для защиты от атак SQL injection посредством удаления escape-символов можно также использовать фильтрацию ввода. Однако этот метод защиты не является надежным в связи с тем, что проблемы может создавать большое число символов. В следующем примере производится поиск разделителей символьных строк:  
  
```csharp
private string SafeSqlLiteral(string inputSQL)  
{  
  return inputSQL.Replace("'", "''");  
}  
```  
  
### <a name="like-clauses"></a>Предложения LIKE  
 Обратите внимание, что при использовании предложения `LIKE` подстановочные знаки по-прежнему нужно выделять escape-символами:  
  
```csharp
s = s.Replace("[", "[[]");  
s = s.Replace("%", "[%]");  
s = s.Replace("_", "[_]");  
```  
  
## <a name="reviewing-code-for-sql-injection"></a>Просмотр кода на предмет возможности атаки SQL Injection  
 Необходимо просматривать все фрагменты кода, вызывающие инструкции `EXECUTE`, `EXEC`или `sp_executesql`. Чтобы выявить процедуры, содержащие эти инструкции, можно использовать запросы, подобные следующему. Этот запрос проверяет наличие 1, 2, 3 или 4 пробелов после слов `EXECUTE` и `EXEC`.  
  
```sql
SELECT object_Name(id) FROM syscomments  
WHERE UPPER(text) LIKE '%EXECUTE (%'  
OR UPPER(text) LIKE '%EXECUTE  (%'  
OR UPPER(text) LIKE '%EXECUTE   (%'  
OR UPPER(text) LIKE '%EXECUTE    (%'  
OR UPPER(text) LIKE '%EXEC (%'  
OR UPPER(text) LIKE '%EXEC  (%'  
OR UPPER(text) LIKE '%EXEC   (%'  
OR UPPER(text) LIKE '%EXEC    (%'  
OR UPPER(text) LIKE '%SP_EXECUTESQL%';  
```  
  
### <a name="wrapping-parameters-with-quotename-and-replace"></a>Упаковка параметров с помощью функций QUOTENAME() и REPLACE()  
 Убедитесь, что в каждой выбранной хранимой процедуре все используемые в динамическом Transact-SQL переменные обрабатываются правильно. Данные, поступающие через входные параметры хранимой процедуры или считываемые из таблицы, должны быть помещены в функции QUOTENAME() или REPLACE(). Помните, что значение @variable, передаваемое функции QUOTENAME(), принадлежит к типу sysname и имеет ограничение длины в 128 символов.  
  
|\@переменная|Рекомендуемый упаковщик|  
|---------------|-------------------------|  
|Имя защищаемого объекта|`QUOTENAME(@variable)`|  
|Строка, содержащая не более 128 знаков.|`QUOTENAME(@variable, '''')`|  
|Строка из > 128 символов|`REPLACE(@variable,'''', '''''')`|  
  
 При использовании этого метода инструкция SET может быть исправлена следующим образом:  
  
```sql
-- Before:  
SET @temp = N'SELECT * FROM authors WHERE au_lname ='''   
 + @au_lname + N'''';  
  
-- After:  
SET @temp = N'SELECT * FROM authors WHERE au_lname = '''   
 + REPLACE(@au_lname,'''','''''') + N'''';  
```  
  
### <a name="injection-enabled-by-data-truncation"></a>Атака Injection, проводимая с помощью усечения данных  
 Любое присваивАՐܐސпеременной диݐАܐؑǐՑPڐސзначение [!INCLUDE[tsql](../../includes/tsql-md.md)] усекается, если оно не вмещается в буфер, назначенный для этой переменной. Если организатор атаки способен обеспечить усечение инструкции, передавая хранимой процедуре непредвиденно длинные строки, он получает возможность манипулировать результатом. Так, хранимая процедура, создаваемая с помощью следующего скрипта, уязвима для атаки Injection, проводимой методом усечения.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
@loginname sysname,  
@old sysname,  
@new sysname  
AS  
-- Declare variable.  
-- Note that the buffer here is only 200 characters long.   
DECLARE @command varchar(200)  
-- Construct the dynamic Transact-SQL.  
-- In the following statement, we need a total of 154 characters   
-- to set the password of 'sa'.   
-- 26 for UPDATE statement, 16 for WHERE clause, 4 for 'sa', and 2 for  
-- quotation marks surrounded by QUOTENAME(@loginname):  
-- 200 - 26 - 16 - 4 - 2 = 154.  
-- But because @new is declared as a sysname, this variable can only hold  
-- 128 characters.   
-- We can overcome this by passing some single quotation marks in @new.  
SET @command= 'update Users set password='   
    + QUOTENAME(@new, '''') + ' where username='   
    + QUOTENAME(@loginname, '''') + ' AND password = '   
    + QUOTENAME(@old, '''')  
  
-- Execute the command.  
EXEC (@command)  
GO  
```  
  
 Передавая 154 знака в буфер, рассчитанный на 128 знаков, злоумышленник может установить новый пароль для sa, не зная старого пароля.  
  
```sql
EXEC sp_MySetPassword 'sa', 'dummy',   
'123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012'''''''''''''''''''''''''''''''''''''''''''''''''''   
```  
  
 Поэтому для переменной команды следует использовать большой буфер или непосредственно выполнять динамический [!INCLUDE[tsql](../../includes/tsql-md.md)] внутри инструкции `EXECUTE` .  
  
### <a name="truncation-when-quotenamevariable--and-replace-are-used"></a>Усечение при использовании функций QUOTENAME(@variable, '''') и REPLACE()  
 Если строки, возвращаемые функциями QUOTENAME() и REPLACE(), не умещаются в выделенном пространстве, они усекаются без взаимодействия с пользователем. Хранимая процедура, создаваемая в следующем примере, показывает, что может произойти.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, the data stored in temp variables  
-- will be truncated because the buffer size of @login, @oldpassword,  
-- and @newpassword is only 128 characters, but QUOTENAME() can return  
-- up to 258 characters.  
    SET @login = QUOTENAME(@loginname, '''')  
    SET @oldpassword = QUOTENAME(@old, '''')  
    SET @newpassword = QUOTENAME(@new, '''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, then @newpassword will be '123... n  
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated,   
-- it can be made to look like the following statement:  
-- UPDATE Users SET password ='1234. . .[127] WHERE username=' -- other stuff here  
    SET @command = 'UPDATE Users set password = ' + @newpassword   
     + ' where username =' + @login + ' AND password = ' + @oldpassword;  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Таким образом, следующая инструкция установит для паролей всех пользователей значение, переданное предыдущим фрагментом кода.  
  
```sql
EXEC sp_MyProc '--', 'dummy', '12345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678'  
```  
  
 Возможно выполнить принудительное усечение строки, для чего нужно превысить выделенное для буфера пространство при использовании функции REPLACE(). Хранимая процедура, создаваемая в следующем примере, показывает, что может произойти.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, data will be truncated because   
-- the buffers allocated for @login, @oldpassword and @newpassword   
-- can hold only 128 characters, but QUOTENAME() can return   
-- up to 258 characters.   
    SET @login = REPLACE(@loginname, '''', '''''')  
    SET @oldpassword = REPLACE(@old, '''', '''''')  
    SET @newpassword = REPLACE(@new, '''', '''''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, @newpassword will be '123...n   
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated, it  
-- can be made to look like the following statement:  
-- UPDATE Users SET password='1234...[127] WHERE username=' -- other stuff here   
    SET @command= 'update Users set password = ''' + @newpassword + ''' where username='''   
     + @login + ''' AND password = ''' + @oldpassword + '''';  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Как и в случае с функцией QUOTENAME(), усечения строки с помощью функции REPLACE() можно избежать, объявив временные переменные, достаточно большие для всех случаев. По возможности функции QUOTENAME() или REPLACE() следует вызывать непосредственно внутри динамического [!INCLUDE[tsql](../../includes/tsql-md.md)]. Или же необходимый размер буфера можно рассчитать следующим образом. Для `@outbuffer = QUOTENAME(@input)`размер буфера переменной `@outbuffer` должен составлять `2*(len(@input)+1)`. При использовании функции `REPLACE()` и двойных кавычек, как в предыдущем примере, достаточно буфера размером `2*len(@input)` .  
  
 Следующий расчет применим ко всем случаям.  
  
```sql
WHILE LEN(@find_string) > 0, required buffer size =  
ROUND(LEN(@input)/LEN(@find_string),0) * LEN(@new_string)   
 + (LEN(@input) % LEN(@find_string))  
```  
  
### <a name="truncation-when-quotenamevariable--is-used"></a>Усечение при использовании функции QUOTENAME(@variable, ']')  
 Усечение может произойти, когда имя защищаемого объекта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] передается инструкциям, которые используют форму функции `QUOTENAME(@variable, ']')`. В следующем примере приведена иллюстрация этого.  
  
```sql
CREATE PROCEDURE sp_MyProc  
    @schemaname sysname,  
    @tablename sysname,  
AS  
  
-- Declare a variable as sysname. The variable will be 128 characters.  
-- But @objectname actually must allow for 2*258+1 characters.   
DECLARE @objectname sysname  
SET @objectname = QUOTENAME(@schemaname)+'.'+ QUOTENAME(@tablename)   
-- Do some operations.  
GO  
```  
  
 При сцеплении значений типа sysname рекомендуется использовать временные переменные достаточно больших размеров, чтобы они могли вмещать до 128 знаков на одно значение. По возможности функцию `QUOTENAME()` следует вызывать непосредственно внутри динамического [!INCLUDE[tsql](../../includes/tsql-md.md)]. Или же необходимый размер буфера можно рассчитать, как это показано в предыдущем разделе.  
  
## <a name="see-also"></a>См. также:  
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)   
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)   
 [sp_executesql (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)   
 [Обеспечение безопасности SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
  
