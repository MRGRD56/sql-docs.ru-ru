---
description: ALTER EXTERNAL LANGUAGE (Transact-SQL) — SQL Server
title: ALTER EXTERNAL LANGUAGE (Transact-SQL) — SQL Server | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2021
ms.prod: sql
ms.technology: language-extensions
ms.topic: language-reference
author: nelgson
ms.author: negust
ms.reviewer: dphansen
manager: cgronlun
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: a86741824e47c51c6a24c39e6dfc5437d12bca45
ms.sourcegitcommit: e2d25f265556af92afcc0acde662929e654bf841
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2021
ms.locfileid: "103489537"
---
# <a name="alter-external-language-transact-sql"></a>ALTER EXTERNAL LANGUAGE (Transact-SQL)
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Изменяет содержимое существующего расширения внешнего языка в базе данных.

## <a name="syntax"></a>Синтаксис

```syntaxsql
ALTER EXTERNAL LANGUAGE language_name  
[ AUTHORIZATION owner_name ]
{
    SET <file_spec>
    | ADD <file_spec>
    | REMOVE PLATFORM <platform> 
}
[ ; ]  

<file_spec> ::=  
{
    ( CONTENT = {<external_lang_specifier> | <content_bits>,
    FILE_NAME = <external_lang_file_name>
    [, PLATFORM = <platform> ]
    [, PARAMETERS = <external_lang_parameters> ]
    [, ENVIRONMENT_VARIABLES = <external_lang_env_variables> ] )
}

<external_lang_specifier> :: =  
{
    '[file_path\]os_file_name'  
}

<content_bits> :: =  
{
    varbinary_literal
   | varbinary_expression
}

<external_lang_file_name> :: =  
'extension_file_name'

<platform> :: =
{
   WINDOWS
  | LINUX
}

< external_lang_parameters > :: =  
'extension_specific_parameters'
```

### <a name="arguments"></a>Аргументы

**language_name**

Языки являются объектами в области базы данных. Имена языков должны быть уникальными в пределах базы данных.

**owner_name**

Указывает имя пользователя или роли, которым принадлежит внешний язык. Если атрибут не указан, владельцем становится текущий пользователь. В зависимости от разрешений может потребоваться предоставить другим пользователям явное разрешение на выполнение скриптов с использованием соответствующего языка.

**file_spec**

Указывает содержимое расширения языка. Для конкретного языка на отдельной платформе разрешено использовать только одну спецификацию файла. 

**external_lang_specifier**

Полный путь к файлу ZIP или TAR.GZ, содержащему код расширений. Это содержимое может быть путем к файлу ZIP (в Windows) или TAR.GZ (в Linux).

**content_bits**

Указывает содержимое языка в виде шестнадцатеричного литерала, аналогично сборкам.
Этот параметр полезен, если нужно создать язык или изменить существующий язык (и у вас есть необходимые разрешения), но файловая система на сервере ограничена, и не удается скопировать файлы библиотеки в место, доступное для сервера.

**external_lang_file_name**

Имя файла расширения DLL или SO. Этот параметр нужен для идентификации правильного файла в случаях, когда в файле ZIP и TAR.GZ <external_lang_specifier> существует несколько файлов DLL или SO.

**external_lang_parameters**

Этот параметр позволяет предоставить среде выполнения внешнего языка набор параметров. Значения параметров предоставляются внешней среде выполнения после запуска внешнего процесса. Однако переменные среды становятся доступны расширению языка до запуска внешнего процесса.

**external_lang_env_variables**

Этот параметр позволяет предоставить среде выполнения внешнего языка набор переменных среды до запуска внешнего процесса. Примером переменной среды является домашний каталог самой среды выполнения. Пример: JRE_HOME.

**platform**

Этот параметр необходим для гибридных сценариев операционной системы. В гибридной архитектуре язык требуется регистрировать один раз для каждой платформы. Имя языка и платформы будет уникальным ключом для каждого внешнего языка. Если платформа не указана, предполагается текущая операционная система.

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks

В настоящее время **PARAMETERS** и **ENVIRONMENT_VARIABLES** не поддерживаются.

## <a name="permissions"></a>Разрешения

Требуется разрешение `ALTER ANY EXTERNAL LANGUAGE`. По умолчанию любой пользователь с учетной записью **dbo**, являющийся членом роли **db_owner**, имеет разрешения на изменение внешнего языка. Другим пользователям необходимо явным образом предоставить разрешение с помощью инструкции [GRANT](./grant-database-permissions-transact-sql.md), указав ALTER ANY EXTERNAL LANGUAGE в качестве привилегии.

## <a name="examples"></a>Примеры

### <a name="alter-an-external-language-in-a-database"></a>Изменение внешнего языка в базе данных  

Следующий пример добавляет внешний язык Java в базу данных на сервере SQL Server в Windows.

```sql
ALTER EXTERNAL LANGUAGE Java 
SET (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

## <a name="see-also"></a>См. также раздел

[CREATE EXTERNAL LANGUAGE (Transact-SQL)](create-external-language-transact-sql.md)  
[DROP EXTERNAL LANGUAGE (Transact-SQL)](drop-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)
