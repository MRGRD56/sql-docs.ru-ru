---
description: sp_describe_parameter_encryption (Transact-SQL)
title: sp_describe_parameter_encryption (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 07/27/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_parameter_encryption
- sp_describe_parameter_encryption_TSQL
- sys.sp_describe_parameter_encryption
- sys.sp_describe_parameter_encryption_TSQL
helpviewer_keywords:
- sp_describe_parameter_encryption
ms.assetid: 706ed441-2881-4934-8d5e-fb357ee067ce
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 94f794d86f6cd2ee95029b73fe58a92876e153d0
ms.sourcegitcommit: cfffd03fe39b04034fa8551165476e53c4bd3c3b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "107298840"
---
# <a name="sp_describe_parameter_encryption-transact-sql"></a>sp_describe_parameter_encryption (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Анализирует указанную [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкцию и ее параметры, чтобы определить, какие параметры соответствуют столбцам базы данных, защищенным с помощью функции Always encrypted. Возвращает метаданные шифрования для параметров, соответствующих зашифрованным столбцам.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sp_describe_parameter_encryption   
    [ @tsql = ] N'Transact-SQL_batch' ,   
    [ @params = ] N'parameters'   
[ ;]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ \@ tsql =] ' Transact-SQL_batch '  
 Одна или несколько инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)]. Transact-SQL_batch могут быть nvarchar (n) или nvarchar (max).  
  
 [ \@ params =] н'параметерс '  
 *\@ params* предоставляет строку объявления для параметров пакета Transact-SQL, который аналогичен sp_executesql. Параметры могут быть nvarchar (n) или nvarchar (max).  
  
 — Одна строка, содержащая определения всех параметров, внедренных в [!INCLUDE[tsql](../../includes/tsql-md.md)] _batch. Строка должна представлять собой константу в Юникоде либо переменную в этом же формате. Определение каждого параметра состоит из имени параметра и типа данных. *n* — это заполнитель, указывающий дополнительные определения параметров. Каждый параметр, указанный в инструкции, должен быть определен в *\@ параметре params*. Если [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкция или пакет в инструкции не содержит параметров, *\@ параметр params* не требуется. Значением по умолчанию для этого аргумента является NULL.  
  
## <a name="return-value"></a>Возвращаемое значение  
 значение 0 указывает на успешное выполнение. Что-то еще указывает на сбой.  
  
## <a name="result-sets"></a>Результирующие наборы  
 **sp_describe_parameter_encryption** возвращает два результирующих набора:  
  
-   Результирующий набор, описывающий криптографические ключи, настроенные для столбцов базы данных, параметры указанной [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкции соответствуют.  
  
-   Результирующий набор, описывающий, каким образом должны быть зашифрованы определенные параметры. Этот результирующий набор ссылается на ключи, описанные в первом результирующем наборе.  
  
 Каждая строка первого результирующего набора описывает пару ключей. ключ шифрования зашифрованного столбца и соответствующий ему главный ключ столбца.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**column_encryption_key_ordinal**|**int**|Идентификатор строки в результирующем наборе.|  
|**database_id**|**int**|Идентификатор базы данных.|  
|**column_encryption_key_id**|**int**|Идентификатор ключа шифрования столбца. Примечание. Этот идентификатор обозначает строку в [sys.column_encryption_keys &#40;представлении каталога&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md) .|  
|**column_encryption_key_version**|**int**|Зарезервировано для последующего использования. В настоящее время всегда содержит 1.|  
|**column_encryption_key_metadata_version**|**Binary (8)**|Отметка времени, представляющая время создания ключа шифрования столбца.|  
|**column_encryption_key_encrypted_value**|**varbinary (4000)**|Зашифрованное значение ключа шифрования столбца.|  
|**column_master_key_store_provider_name**|**sysname**|Имя поставщика для хранилища ключей, содержащего главный ключ столбца, который был использован для создания зашифрованного значения ключа шифрования столбца.|  
|**column_master_key_path**|**nvarchar(4000)**|Путь к главному ключу столбца, который использовался для создания зашифрованного значения ключа шифрования столбца.|  
|**column_encryption_key_encryption_algorithm_name**|**sysname**|Имя алгоритма шифрования, используемого для получения значения шифрования ключа шифрования столбца.|  
  
 Каждая строка второго результирующего набора содержит метаданные шифрования для одного параметра.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int**|Идентификатор строки в результирующем наборе.|  
|**parameter_name**|**sysname**|Имя одного из параметров, указанных в аргументе *\@ params* .|  
|**column_encryption_algorithm**|**tinyint**|Код, указывающий алгоритм шифрования, настроенный для столбца, параметр соответствует. В настоящее время поддерживаются следующие значения: 2 для **AEAD_AES_256_CBC_HMAC_SHA_256**.|  
|**column_encryption_type**|**tinyint**|Код, указывающий тип шифрования, настроенный для столбца, параметр соответствует. Поддерживаются такие значения:<br /><br /> 0 — открытый текст (столбец не зашифрован)<br /><br /> 1 — случайное шифрование<br /><br /> 2 — детерминированное шифрование.|  
|**column_encryption_key_ordinal**|**int**|Код строки в первом результирующем наборе. Строка, на которую указывает ссылка, описывает ключ шифрования столбца, настроенный для столбца, параметр соответствует.|  
|**column_encryption_normalization_rule_version**|**tinyint**|Номер версии алгоритма нормализации типа.|  
  
## <a name="remarks"></a>Remarks  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер клиента, поддерживающий Always encrypted, автоматически вызывает **sp_describe_parameter_encryption** для получения метаданных шифрования для параметризованных запросов, выданных приложением. Впоследствии драйвер использует метаданные шифрования для шифрования значений параметров, которые соответствуют столбцам базы данных, защищенным с помощью Always Encrypted, и заменяет значения параметров в виде открытого текста, переданные приложением, на значения зашифрованных параметров перед отправкой запроса ядру СУБД.  
  
## <a name="permissions"></a>Разрешения  
 Запрашивать определение **ключа шифрования столбцов** и **просматривать любые разрешения для определения главного ключа столбца** в базе данных.  
  
## <a name="examples"></a>Примеры  
  
```sql  
CREATE COLUMN MASTER KEY [CMK1]  
WITH  
(  
       KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',  
    KEY_PATH = N'CurrentUser/my/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305'  
);  
GO  
  
CREATE COLUMN ENCRYPTION KEY [CEK1]  
WITH VALUES  
(  
       COLUMN_MASTER_KEY = [CMK1],  
    ALGORITHM = 'RSA_OAEP',  
    ENCRYPTED_VALUE =   
0x016E000001630075007200720065006E00740075007300650072002F006D007  
9002F00610036003600620062003000660036006400640037003000620064006  
6006600300032006200360032006400300066003800370065003300340030003  
200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF991  
37B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA51  
7A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6  
686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015B  
DB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8  
C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE3  
74DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A3472  
3276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550E  
C5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E0  
35175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D8  
01ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B  
4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF8  
1A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98AD82155F93C58202F  
C24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B0195883360  
4707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E  
9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C  
3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085  
);  
GO  
  
CREATE TABLE t1 (  
c1 INT ENCRYPTED WITH (  
    COLUMN_ENCRYPTION_KEY = [CEK1],   
    ENCRYPTION_TYPE = Randomized,   
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL,  
);  
  
EXEC sp_describe_parameter_encryption N'INSERT INTO t1 VALUES(@c1)',  N'@c1 INT';  
```  
  
 Первый результирующий набор:  
  
|column_encryption_key_ordinal|database_id|column_encryption_key_id|column_encryption_key_version|column_encryption_key_metadata_version|column_encryption_key_encrypted_value|  
|--------------------------------------|------------------|---------------------------------|--------------------------------------|------------------------------------------------|-----------------------------------------------|  
|1|5|1|1|0x99EDA60083A50000|0x016E000001630075007200720065006E00740075007300650072002F006D0079002F006100360036006200620030006600360064006400370030006200640066006600300032006200360032006400300066003800370065003300340030003200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF99137B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA517A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015BDB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE374DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A34723276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550EC5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E035175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D801ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF81A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98 AD82155F93C58202FC24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B01958833604707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085|  
  
 (Продолжение результатов.)  
  
|column_master_key_store_provider_name|column_master_key_path|column_encryption_key_encryption_algorithm_name|  
|------------------------------------------------|-------------------------------|----------------------------------------------------------|  
|MSSQL_CERTIFICATE_STORE|CurrentUser/My/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305|RSA_OAEP|  
  
 Второй результирующий набор:  
  
|parameter_ordinal|parameter_name|column_encryption_algorithm|column_encryption_type|  
|------------------------|---------------------|-----------------------------------|------------------------------|  
|1|\@тогда|1|1|  
  
 (Продолжение результатов.)  
  
|column_encryption_key_ordinal|column_encryption_normalization_rule_version|  
|--------------------------------------|------------------------------------------------------|  
|1|1|  
  
## <a name="see-also"></a>См. также:  
 [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Разработка приложения с помощью Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
  
