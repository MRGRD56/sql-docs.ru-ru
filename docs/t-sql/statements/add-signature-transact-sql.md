---
description: ADD SIGNATURE (Transact-SQL)
title: ADD SIGNATURE (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ADD SIGNATURE
- ADD_SIGNATURE_TSQL
helpviewer_keywords:
- ADD SIGNATURE statement
- adding digital signatures
- signatures [SQL Server]
- digital signatures [SQL Server]
ms.assetid: 64d8b682-6ec1-4e5b-8aee-3ba11e72d21f
author: VanMSFT
ms.author: vanto
ms.reviewer: ''
ms.custom: ''
ms.date: 06/10/2020
ms.openlocfilehash: 86513345502531da670b870b5ecf70de9270f18f
ms.sourcegitcommit: ece104654ac14e10d32e59f45916fa944665f4df
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2021
ms.locfileid: "102474908"
---
# <a name="add-signature-transact-sql"></a>ADD SIGNATURE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Добавляет цифровую подпись для хранимой процедуры, функции, сборки или DML-триггера. Добавляет также подпись другой стороны для хранимой процедуры, функции, сборки или DML-триггера.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Синтаксис

```syntaxsql
ADD [ COUNTER ] SIGNATURE TO module_class::module_name
    BY <crypto_list> [ ,...n ]
  
<crypto_list> ::=  
    CERTIFICATE cert_name  
    | CERTIFICATE cert_name [ WITH PASSWORD = 'password' ]
    | CERTIFICATE cert_name WITH SIGNATURE = signed_blob
    | ASYMMETRIC KEY Asym_Key_Name  
    | ASYMMETRIC KEY Asym_Key_Name [ WITH PASSWORD = 'password'.]
    | ASYMMETRIC KEY Asym_Key_Name WITH SIGNATURE = signed_blob
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*module_class*  
Класс модуля, к которому добавляется подпись. По умолчанию для модулей области схемы это класс OBJECT.  
  
 *module_name*  
 Имя хранимой процедуры, функции, сборки или триггера, для которых будет добавлена подпись или скрепляющая подпись.  
  
 CERTIFICATE *cert_name*  
 Имя сертификата, который будет использован для добавления подписи или скрепляющей подписи для хранимой процедуры, функции, сборки или триггера.  
  
 WITH PASSWORD ='*password*'  
 Пароль, необходимый для расшифровки закрытого ключа сертификата или асимметричного ключа. Это предложение необходимо, только когда закрытый ключ не защищен главным ключом базы данных.  
  
 SIGNATURE =*signed_blob*  
 Указывает подписанный большой двоичный объект (BLOB) модуля. Это предложение полезно, если нужно передать модуль без передачи закрытого ключа. При использовании этого предложения необходимы только модуль, подпись и открытый ключ, чтобы добавить подписанный большой двоичный объект в базу данных. Аргумент *signed_blob* является самым большим двоичным объектом в шестнадцатеричном формате.  
  
 ASYMMETRIC KEY *Asym_Key_Name*  
 Имя ассиметричного ключа, который будет использован для добавления подписи или скрепляющей подписи для хранимой процедуры, функции, сборки или триггера.  
  
## <a name="remarks"></a>Remarks

Подписываемый модуль или модуль, на который ставится скрепляющая подпись, а также сертификат или асимметричный ключ, используемые для подписания, должны существовать. Каждый символ в модуле включен в вычисление подписи. Сюда включены начальные символы возврата каретки и перевода строки.  
  
 Подписи и скрепляющие подписи для модуля могут добавляться любым числом сертификатов и асимметричных ключей.  
  
 Подпись модуля удаляется при изменении модуля.  
  
 Если модуль содержит предложение EXECUTE AS, это значит, что идентификатор безопасности (SID) участника также является частью процесса подписи.  
  
> [!CAUTION]
> Подписывание модуля следует использовать только для предоставления разрешений, но не для их запрещения или отмены.  
  
 Триггеры языка описания данных (DDL) и встроенные функции с табличным значением подписывать нельзя.  
  
 Сведения о подписях содержатся в представлении каталога sys.crypt_properties.  
  
> [!WARNING]
>  При воссоздании процедуры для подписи все инструкции в исходном пакете должны соответствовать повторно созданному пакету. Если какая-либо часть пакета будет отличаться, даже количеством пробелов или комментариями, будет получена другая подпись.  
  
## <a name="countersignatures"></a>Скрепляющая подпись  
 Во время выполнения модуля с обычной подписью эта подпись временно добавляется в токен SQL, но, если этот модуль выполняет другой модуль или выполнение модуля прерывается, подпись теряется. Скрепляющая подпись — это особая форма подписи. Сама по себе скрепляющая подпись не предоставляет никаких разрешений, но позволяет сохранить подписи, поставленные тем же самым сертификатом или асимметричным ключом, на все время вызова объекта, на котором стоит скрепляющая подпись.  
  
 Предположим, что Алиса вызывает процедуру ProcForAlice, которая вызывает ProcSelectT1, выбирающую данные из таблицы T1. У Алисы есть разрешение EXECUTE на процедуры ProcForAlice и ProcSelectT1, но нет разрешения SELECT на таблицу T1, и во всей этой цепочке нет никаких цепочек владения. Алиса не имеет доступа к таблице T1 — ни прямого, ни с помощью процедур ProcForAlice и ProcSelectT1. Поскольку мы хотим, чтобы Алиса всегда использовала для обращения к таблице процедуру ProcForAlice, мы не хотим предоставлять ей разрешение на выполнение процедуры ProcSelectT1. Что делать в этом случае?  
  
-   Если подписать процедуру ProcSelectT1 так, чтобы она имела доступ к таблице T1, Алиса сможет напрямую вызывать процедуру ProcSelectT1 и ей не нужно будет вызывать процедуру ProcForAlice.  
  
-   Можно не давать Алисе разрешения EXECUTE на процедуру ProcSelectT1, но в этом случае Алиса не сможет вызывать процедуру ProcSelectT1 через процедуру ProcForAlice.
  
-   Подписание процедуры ProcForAlice также не будет работать само по себе, так как подпись будет потеряна при вызове процедуры ProcSelectT1.  
  
Однако при добавлении подписи другой стороны к ProcSelectT1 с помощью того же сертификата, который использовался для подписывания процедуры ProcForAlice, подпись [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет сохранена по всей цепочке вызова и обеспечит доступ к таблице T1. Если Алиса захочет напрямую вызвать процедуру ProcSelectT1, она не сможет получить доступ к таблице T1, так как подпись другой стороны не предоставляет никаких прав. В приведенном ниже примере В показан код [!INCLUDE[tsql](../../includes/tsql-md.md)] для этого примера.  
  
## <a name="permissions"></a>Разрешения  

Требует разрешения ALTER на объект и разрешение CONTROL на сертификат или асимметричный ключ. Если соответствующий закрытый ключ защищен паролем, то у пользователя также должен быть этот пароль.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-signing-a-stored-procedure-by-using-a-certificate"></a>A. Подписание хранимой процедуры с помощью сертификата

 В следующем примере хранимая процедура `HumanResources.uspUpdateEmployeeLogin` подписывается сертификатом `HumanResourcesDP`.  
  
```sql  
USE AdventureWorks2012;  
ADD SIGNATURE TO HumanResources.uspUpdateEmployeeLogin   
    BY CERTIFICATE HumanResourcesDP;  
GO  
```  
  
### <a name="b-signing-a-stored-procedure-by-using-a-signed-blob"></a>Б. Подписание хранимой процедуры с помощью подписанного большого двоичного объекта

В следующем примере создается новая база данных и сертификат для использования в примере. В этом примере будет создана и подписана простая хранимая процедура и выполнена выборка большого двоичного объекта сигнатуры из `sys.crypt_properties`. Подпись удаляется, затем добавляется еще раз. Процедура подписывается с помощью конструкции WITH SIGNATURE.  
  
```sql  
CREATE DATABASE TestSignature ;  
GO  
USE TestSignature ;  
GO  
-- Create a CERTIFICATE to sign the procedure.  
CREATE CERTIFICATE cert_signature_demo   
    ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
    WITH SUBJECT = 'ADD SIGNATURE demo';  
GO  

-- Create a simple procedure.  
CREATE PROC [sp_signature_demo]  
AS  
    PRINT 'This is the content of the procedure.' ;  
GO  
-- Sign the procedure.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]   
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y' ;  
GO  

-- Get the signature binary BLOB for the sp_signature_demo procedure.  
SELECT cp.crypt_property  
    FROM sys.crypt_properties AS cp  
    JOIN sys.certificates AS cer  
        ON cp.thumbprint = cer.thumbprint  
    WHERE cer.name = 'cert_signature_demo' ;  
GO  
```  
  
 Подпись `crypt_property`, возвращаемая этой инструкции, будет изменяться каждый раз при создании процедуры. Запишите результат для использования далее в ходе примера. Для этого примера выдается следующий результат: `0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373`.  
  
```sql  
-- Drop the signature so that it can be signed again.  
DROP SIGNATURE FROM [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo];  
GO  

-- Add the signature. Use the signature BLOB obtained earlier.  
ADD SIGNATURE TO [sp_signature_demo]   
    BY CERTIFICATE [cert_signature_demo]  
    WITH SIGNATURE = 0x831F5530C86CC8ED606E5BC2720DA835351E46219A6D5DE9CE546297B88AEF3B6A7051891AF3EE7A68EAB37CD8380988B4C3F7469C8EABDD9579A2A5C507A4482905C2F24024FFB2F9BD7A953DD5E98470C4AA90CE83237739BB5FAE7BAC796E7710BDE291B03C43582F6F2D3B381F2102EEF8407731E01A51E24D808D54B373 ;  
GO  
```  
  
### <a name="c-accessing-a-procedure-using-a-countersignature"></a>В. Обращение к процедуре с помощью скрепляющей подписи

В следующем примере показано, как скрепляющая подпись помогает контролировать доступ к объекту.  
  
```sql  
-- Create tesT1 database  
CREATE DATABASE testDB;  
GO  
USE testDB;  
GO  
-- Create table T1  
CREATE TABLE T1 (c VARCHAR(11));  
INSERT INTO T1 VALUES ('This is T1.');  
  
-- Create a TestUser user to own table T1  
CREATE USER TestUser WITHOUT LOGIN;  
ALTER AUTHORIZATION ON T1 TO TestUser;  
  
-- Create a certificate for signing  
CREATE CERTIFICATE csSelectT  
  ENCRYPTION BY PASSWORD = 'SimplePwd01'  
  WITH SUBJECT = 'Certificate used to grant SELECT on T1';  
CREATE USER ucsSelectT1 FROM CERTIFICATE csSelectT;  
GRANT SELECT ON T1 TO ucsSelectT1;  
  
-- Create a principal with low privileges  
CREATE LOGIN Alice WITH PASSWORD = 'SimplePwd01';  
CREATE USER Alice;  
  
-- Verify Alice cannoT1 access T1;  
EXECUTE AS LOGIN = 'Alice';  
    SELECT * FROM T1;  
REVERT;  
  
-- Create a procedure that directly accesses T1  
CREATE PROCEDURE procSelectT1 AS  
BEGIN  
    PRINT 'Now selecting from T1...';  
    SELECT * FROM T1;  
END;  
GO  
GRANT EXECUTE ON ProcSelectT1 to public;  
  
-- Create special procedure for accessing T1  
CREATE PROCEDURE  ProcForAlice AS  
BEGIN  
   IF USER_ID() <> USER_ID('Alice')  
    BEGIN  
        PRINT 'Only Alice can use this.';  
        RETURN  
    END  
   EXEC ProcSelectT1;  
END;  
GO;  
GRANT EXECUTE ON ProcForAlice TO PUBLIC;  
  
-- Verify procedure works for a sysadmin user  
EXEC ProcForAlice;  
  
-- Alice still can't use the procedure yet  
EXECUTE AS LOGIN = 'Alice';  
    EXEC ProcForAlice;  
REVERT;  
  
-- Sign procedure to grant it SELECT permission  
ADD SIGNATURE TO ProcForAlice BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Counter sign ProcSelectT1, to make this work  
ADD COUNTER SIGNATURE TO ProcSelectT1 BY CERTIFICATE csSelectT   
WITH PASSWORD = 'SimplePwd01';  
  
-- Now the proc works.   
-- Note that calling ProcSelectT1 directly still doesn't work  
EXECUTE AS LOGIN = 'Alice';  
    EXEC ProcForAlice;  
    EXEC ProcSelectT1;  
REVERT;  
  
-- Cleanup  
USE master;  
GO  
DROP DATABASE testDB;  
DROP LOGIN Alice;  
```  
  
## <a name="see-also"></a>См. также:

- [sys.crypt_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)
- [DROP SIGNATURE (Transact-SQL)](../../t-sql/statements/drop-signature-transact-sql.md)
