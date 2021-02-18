---
title: Использование программы sqlcmd с переменными скрипта
description: Сведения о том, как использовать переменные скриптов для создания скрипта, который может применяться в нескольких сценариях.
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- scripts [SQL Server], sqlcmd utility
- variables [SQL Server], scripts
- scripting variables [SQL Server]
- sqlcmd utility, scripts
- setvar command
ms.assetid: 793495ca-cfc9-498d-8276-c44a5d09a92c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 799bc2eca98002343d8ff755782d8f3b71d2af77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342640"
---
# <a name="sqlcmd---use-with-scripting-variables"></a>sqlcmd — использование с переменными скрипта
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Переменные, которые используются в скриптах, называются переменными скрипта. Переменные скрипта позволяют использовать один скрипт в различных обстоятельствах. Например, если нужно выполнить один скрипт на нескольких серверах, вместо изменения скрипта для каждого сервера можно указать для имени сервера соответствующую переменную скрипта. Изменяя имя сервера, указываемое в переменной скрипта, можно выполнять один и тот же скрипт на разных серверах.  
  
 Переменные сценариев можно определить явно с помощью команды **setvar** или неявно с помощью параметра **sqlcmd-v**.  
  
 Кроме того, этот раздел содержит примеры определения переменных среды в командной строке Cmd.exe с помощью инструкции **SET**.  
  
## <a name="setting-scripting-variables-by-using-the-setvar-command"></a>Настройка переменных скрипта с помощью команды setvar  
 Команда **setvar** используется для определения переменных сценария. Переменные, определенные с помощью команды **setvar** , предназначены для внутреннего хранения. Переменные сценария не следует путать с переменными среды, которые определяются в командной строке с помощью инструкции **SET**. Если сценарий ссылается на переменную, которая не является переменной среды или не определена с помощью команды **setvar**, то возвращается сообщение об ошибке, и выполнение сценария останавливается. Дополнительные сведения см. в разделе о параметре **-b** статьи [Служебная программа sqlcmd](../../tools/sqlcmd-utility.md).  
  
## <a name="variable-precedence-low-to-high"></a>Приоритет переменных (от низкого к высокому)  
 Если несколько переменных имеют одинаковое имя, то используется переменная с более высоким приоритетом.  
  
1.  Системные переменные среды.  
  
2.  Пользовательские переменные среды.  
  
3.  Командная оболочка (**SET X=Y**), заданная в командной строке перед запуском **sqlcmd**  
  
4.  **sqlcmd -v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Чтобы просмотреть переменные среды, на **панели управления** откройте компонент **Система** и перейдите на вкладку **Дополнительно** .  
  
## <a name="implicitly-setting-scripting-variables"></a>Неявное задание переменных скрипта  
 При запуске программы **sqlcmd** с параметром, с которым связана переменная **sqlcmd** , переменной **sqlcmd** неявно присваивается значение, заданное для этого параметра. В следующем примере программа `sqlcmd` запускается с параметром `-l` . Этим неявно определяется переменная SQLLOGINTIMEOUT.  
  
```
c:\> sqlcmd -l 60
```
 
Кроме того, переменные сценария, которые в нем содержатся, задаются с помощью параметра **-v** . В следующем скрипте (имя файла `testscript.sql`) `ColumnName` является переменной скрипта.  
 
```
USE AdventureWorks2012;

SELECT x.$(ColumnName)
FROM Person.Person x
WHERE x.BusinessEntityID < 5;
```

Кроме того, с помощью параметра `-v` можно указать имя возвращаемого столбца:  
 
```
sqlcmd -v ColumnName ="FirstName" -i c:\testscript.sql
```

Чтобы возвратить другой столбец с помощью того же скрипта, измените значение переменной скрипта `ColumnName` .  
  
```
sqlcmd -v ColumnName ="LastName" -i c:\testscript.sql
```

## <a name="guidelines-for-scripting-variable-names-and-values"></a>Правила выбора имен и значений переменных скрипта  
 Присваивая имена переменным скрипта, необходимо придерживаться следующих правил.  
  
-   Имена переменных не должны содержать символов пробела и кавычек.  
  
-   Имена переменных не должны иметь такую же форму, как у выражений переменных *$(var)* .  
  
-   В переменных скрипта регистр не учитывается.  
  
    > [!NOTE]  
    >  Если переменной среды **sqlcmd** не присвоено никакое значение, то она удаляется. При использовании **:setvar VarName** без значения переменная очищается.  
  
 Присваивая значения переменным скрипта, необходимо придерживаться следующих правил.  
  
-   Переменную, заданную с помощью параметра **setvar** или **-v** , необходимо заключать в кавычки, если строковое значение содержит пробелы.  
  
-   Если кавычки — часть значения переменной, то их необходимо экранировать. Например,`setvar MyVar "spac""e"`.  
  
## <a name="guidelines-for-cmdexe-set-variable-values-and-names"></a>Правила присваивания значений и имен переменным с помощью команды Cmd.exe SET  
 Переменные, заданные с помощью инструкции SET, являются частью среды Cmd.exe, и на них может ссылаться программа **sqlcmd**. Ознакомьтесь со следующими рекомендациями:  
  
-   Имена переменных не должны содержать символов пробела и кавычек.  
  
-   Значения переменных могут содержать символы пробела и кавычки.  
  
## <a name="sqlcmd-scripting-variables"></a>Переменные скрипта sqlcmd  
 Переменные, которые определяются программой **sqlcmd** , называются переменными сценария. В следующей таблице приведен список переменных сценария программы **sqlcmd** .  
  
|        Переменная         | Связанный параметр | Чтение-запись |         По умолчанию         |
| ----------------------- | -------------- | --- | ----------------------- |
| SQLCMDUSER\*             | -U             | R   | ""                      |
| SQLCMDPASSWORD\*         | -P             | --  | ""                      |
| SQLCMDSERVER\*           | -S             | R   | "DefaultLocalInstance"  |
| SQLCMDWORKSTATION       | -H             | R   | "ComputerName"          |
| SQLCMDDBNAME            | -d             | R   | ""                      |
| SQLCMDLOGINTIMEOUT      | -l             | Чтение-запись | "8" (секунд)           |
| SQLCMDSTATTIMEOUT       | -T             | Чтение-запись | "0" = неограниченное время ожидания |
| SQLCMDHEADERS           | -H             | Чтение-запись | "0"                     |
| SQLCMDCOLSEP            | -S             | Чтение-запись | " "                     |
| SQLCMDCOLWIDTH          | -w             | Чтение-запись | "0"                     |
| SQLCMDPACKETSIZE        | -a             | R   | "4096"                  |
| SQLCMDERRORLEVEL        | -M             | Чтение-запись | "0"                     |
| SQLCMDMAXVARTYPEWIDTH   | -y             | Чтение-запись | «256»                   |
| SQLCMDMAXFIXEDTYPEWIDTH | -y             | Чтение-запись | "0" = неограниченное время ожидания         |
| SQLCMDEDITOR            |                | Чтение-запись | "edit.com"              |
| SQLCMDINI               |                | R   | ""                      |

Значения переменных SQLCMDUSER, SQLCMDPASSWORD и SQLCMDSERVER задаются при использовании команды **:Connect** .  

Пометка «Чтение» означает, что значение может быть задано только один раз в процессе инициализации программы.  
  
Пометка "Чтение и запись" означает, что переменная может быть изменена командой **setvar** , и все последующие команды будут использовать новое значение.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-the-setvar-command-in-a-script"></a>A. Использование команды setvar в скрипте  
 Команда **setvar** позволяет управлять многими параметрами программы **sqlcmd** в сценарии. В следующем примере создается скрипт `test.sql` , в котором переменной `SQLCMDLOGINTIMEOUT` присваивается значение `60` , а другой переменной скрипта, `server`, присваивается значение `testserver`. Следующий код входит в скрипт `test.sql`.  

```
:setvar SQLCMDLOGINTIMEOUT 60
:setvar server "testserver"
:connect $(server) -l $(SQLCMDLOGINTIMEOUT)

USE AdventureWorks2012;

SELECT FirstName, LastName
FROM Person.Person;
```

Затем скрипт вызывается с помощью sqlcmd:

```
sqlcmd -i c:\test.sql
```
  
### <a name="b-using-the-setvar-command-interactively"></a>Б. Интерактивное использование команды setvar  
 В следующем примере показана интерактивная установка переменной сценария с помощью команды `setvar` .  

```
sqlcmd
:setvar  MYDATABASE AdventureWorks2012
USE $(MYDATABASE);
GO
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Changed database context to 'AdventureWorks2012'
1>
```
  
### <a name="c-using-command-prompt-environment-variables-within-sqlcmd"></a>В. Использование переменных среды командной строки в программе sqlcmd  
 В следующем примере устанавливаются четыре переменные среды `are` , которые затем вызываются из `sqlcmd`.  

```
C:\>SET tablename=Person.Person
C:\>SET col1=FirstName
C:\>SET col2=LastName
C:\>SET title=Ms.
C:\>sqlcmd -d AdventureWorks2012
1> SELECT TOP 5 $(col1) + ' ' + $(col2) AS Name
2> FROM $(tablename)
3> WHERE Title ='$(title)'
4> GO
```
  
### <a name="d-using-user-level-environment-variables-within-sqlcmd"></a>Г. Использование переменных среды уровня пользователя в программе sqlcmd  
 В следующем примере пользовательская переменная среды `%Temp%` устанавливается в командной строке и передается входному файлу `sqlcmd` . Чтобы просмотреть пользовательские переменные среды, на **панели управления** дважды щелкните компонент **Система**. Перейдите на вкладку **Дополнительно** и щелкните **Переменные среды**.  
  
 Следующий программный код является частью входного файла `c:\testscript.txt`:

```
:OUT $(MyTempDirectory)
USE AdventureWorks2012;

SELECT FirstName
FROM AdventureWorks2012.Person.Person
WHERE BusinessEntityID` `< 5;
```

Следующий программный код вводится в командной строке:

```
C:\ >SET MyTempDirectory=%Temp%\output.txt
C:\ >sqlcmd -i C:\testscript.txt
```

 Следующий результат передается выходному файлу C:\Documents and Settings\\<пользователь\>\Local Settings\Temp\output.txt.  

```
Changed database context to 'AdventureWorks2012'.
FirstName
--------------------------------------------------
Gustavo
Catherine
Kim
Humberto

(4 rows affected)
```

### <a name="e-using-a-startup-script"></a>Д. Использование скрипта запуска  
 Сценарий запуска **sqlcmd** выполняется при запуске программы **sqlcmd** . В следующем примере задается переменная среды `SQLCMDINI`. Ниже приведено содержимое сценария `init.sql.`  

```
SET NOCOUNT ON
GO

DECLARE @nt_username nvarchar(128)
SET @nt_username = (SELECT rtrim(convert(nvarchar(128), nt_username))
FROM sys.dm_exec_sessions WHERE spid = @@SPID)
SELECT  @nt_username + ' is connected to ' +
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('servername'))) +
' (' +`  
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('productversion'))) +
')'
:setvar SQLCMDMAXFIXEDTYPEWIDTH 100
SET NOCOUNT OFF
GO

:setvar SQLCMDMAXFIXEDTYPEWIDTH
```

 Таким образом файл `init.sql` вызывается при запуске `sqlcmd` .  
  
```
c:\> SET sqlcmdini=c:\init.sql
>1 Sqlcmd
```

 Результат.  

```
>1 < user > is connected to < server > (9.00.2047.00)
```

  
> [!NOTE]  
>  Параметр **-X** отключает функцию сценария запуска.  
  
### <a name="f-variable-expansion"></a>Е. Расширение переменной  
 В следующем примере показана работа с данными в форме переменной **sqlcmd** .  

```
USE AdventureWorks2012;
CREATE TABLE AdventureWorks2012.dbo.VariableTest
(
Col1 nvarchar(50)
);
GO
```

 Вставляет одну строку в столбец `Col1` объекта базы данных `dbo.VariableTest` , содержащего значение `$(tablename)`.  

```
INSERT INTO AdventureWorks2012.dbo.VariableTest(Col1)
VALUES('$(tablename)');
GO
```
  
 В командной строке `sqlcmd` , если ни одна переменная не равна `$(tablename)`, следующие инструкции возвращают эту строку.  
  
```
C:\> sqlcmd
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>2 GO
>3 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>4 GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
>1 Col1
>2 ------------------
>3 $(tablename)
>4
>5 (1 rows affected)
```

 При условии, что переменная `MyVar` равна `$(tablename)`.  

```
>6 :setvar MyVar $(tablename)
```

 Эти инструкции возвращают строку и сообщение «Переменная сценария "tablename" не определена».  

```
>6 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>7 GO

>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>2 GO
```

 Эти инструкции возвращают строку.  

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(MyVar)';
>2 GO
```

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(MyVar)';
>2 GO
```
  
## <a name="see-also"></a>См. также:  
 [Использование программы sqlcmd](./sqlcmd-use-the-utility.md)   
 [sqlcmd Utility](../../tools/sqlcmd-utility.md)   
 [Справочник по программе командной строки (Database Engine)](../../tools/command-prompt-utility-reference-database-engine.md)  
  
