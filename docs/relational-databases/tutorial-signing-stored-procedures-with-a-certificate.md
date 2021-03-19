---
description: Учебник. Подписывание хранимых процедур с помощью сертификата
title: Учебник. Подписывание хранимых процедур с помощью сертификата
ms.custom: seo-dt-2019
ms.date: 08/23/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: quickstart
helpviewer_keywords:
- signing stored procedures tutorial [SQL Server]
ms.assetid: a4b0f23b-bdc8-425f-b0b9-e0621894f47e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 91bbdaacd4765639d056fe4ccf54da4c1e0dd371
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610944"
---
# <a name="tutorial-signing-stored-procedures-with-a-certificate"></a>Учебник. Подписывание хранимых процедур с помощью сертификата
[!INCLUDE [SQL Server Azure SQL Database SQL Managed Instance](../includes/applies-to-version/sql-asdb-asdbmi.md)]
В этом учебнике демонстрируется подписание хранимых процедур с помощью сертификата, созданного [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> Для запуска кода в этом учебнике необходимо, чтобы был настроен режим смешанной безопасности. Кроме того, необходимо наличие установленной базы данных AdventureWorks2017.   
  
Подписывание хранимой процедуры с помощью сертификата полезно в том случае, если для хранимой процедуры необходимо требовать разрешения, но явно предоставлять пользователям эти права нежелательно. Хотя эту задачу можно выполнить и другими способами, такими как инструкция EXECUTE AS, использование сертификата позволяет применить трассировку, чтобы найти участника, вызвавшего хранимую процедуру. Таким образом обеспечивается высокий уровень аудита, особенно во время выполнения операций безопасности или операций языка описания данных DDL.  
  
Можно создать сертификат в базе данных master (чтобы предоставлять разрешения уровня сервера) или в любой другой пользовательской базе данных (для предоставления разрешений уровня базы данных). В этом сценарии пользователь, не обладающий правами на базовые таблицы, должен получить доступ к хранимой процедуре в базе данных AdventureWorks2017, при этом необходимо проконтролировать след доступа к объекту. Вместо того чтобы использовать другие методы цепочки владения, будет создана учетная запись пользователя сервера и базы данных без прав на базовые объекты, а также учетная запись пользователя базы данных с правами на таблицы и хранимые процедуры. Безопасность хранимой процедуры и второй учетной записи пользователя базы данных будет обеспечена сертификатом. Вторая учетная запись пользователя будет обладать доступом ко всем объектам. Она предоставляет доступ к хранимой процедуре первой учетной записи пользователя.  
  
В этом сценарии сначала создается сертификат базы данных, хранимая процедура и пользователь, затем весь процесс проверяется с помощью следующих шагов:  
  
Каждый блок кода в этом примере объясняется по порядку. Чтобы скопировать весь пример, см. раздел [Пример целиком](#CompleteExample) в конце этого учебника.  

## <a name="prerequisites"></a>предварительные требования
Для работы с этим учебником требуется среда SQL Server Management Studio, доступ к серверу SQL Server и база данных AdventureWorks.

- Установите [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Установите выпуск [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Скачайте [примеры баз данных AdventureWorks2017](../samples/adventureworks-install-configure.md).

Инструкции по восстановлению базы данных в SQL Server Management Studio см. в разделе [Восстановление базы данных](./backup-restore/restore-a-database-backup-using-ssms.md).   
  
## <a name="1-configure-the-environment"></a>1. Настройка среды  
Чтобы задать начальный контекст в этом примере, откройте в [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] новый запрос и откройте базу данных Adventureworks2017 с помощью приведенного ниже кода. Этот код изменяет контекст базы данных на `AdventureWorks2017` , затем создает новое имя входа сервера и новую учетную запись пользователя базы данных (`TestCreditRatingUser`) с использованием пароля.  
  
```sql  
USE AdventureWorks2017;  
GO  
-- Set up a login for the test user  
CREATE LOGIN TestCreditRatingUser  
   WITH PASSWORD = 'ASDECd2439587y'  
GO  
CREATE USER TestCreditRatingUser  
FOR LOGIN TestCreditRatingUser;  
GO  
```  
  
Дополнительные сведения об инструкции CREATE USER см. в разделе [CREATE USER (Transact-SQL)](../t-sql/statements/create-user-transact-sql.md). Дополнительные сведения об инструкции CREATE LOGIN см. в разделе [CREATE LOGIN (Transact-SQL)](../t-sql/statements/create-login-transact-sql.md).  
  
## <a name="2-create-a-certificate"></a>2. Создание сертификата  
Можно создавать сертификаты на сервере, использующем в качестве контекста базу данных master, базу данных пользователя или обе базы одновременно. Есть несколько вариантов обеспечения безопасности сертификата. Дополнительные сведения о сертификатах см. в разделе [CREATE CERTIFICATE (Transact-SQL)](../t-sql/statements/create-certificate-transact-sql.md).  
  
Запустите этот код, чтобы создать сертификат базы данных и защитить его паролем.  
  
```sql  
CREATE CERTIFICATE TestCreditRatingCer  
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
      WITH SUBJECT = 'Credit Rating Records Access',   
      EXPIRY_DATE = '12/31/2021';  -- Error 3701 will occur if this date is not in the future
GO  
```  
  
## <a name="3-create-and-sign-a-stored-procedure-using-the-certificate"></a>3. Создание и подписывание хранимой процедуры с помощью сертификата  
Используйте следующий код, чтобы создать хранимую процедуру, которая выбирает данные из таблицы `Vendor` в схеме базы данных `Purchasing` , ограничивая доступ и предоставляя его только для компаний с уровнем кредитоспособности 1. В первом разделе хранимой процедуры в целях демонстрации основных принципов работы выводится контекст учетной записи пользователя, с которой работает процедура. Удовлетворять требованиям не обязательно.  
  
```sql  
CREATE PROCEDURE TestCreditRatingSP  
AS  
BEGIN  
   -- Show who is running the stored procedure  
   SELECT SYSTEM_USER 'system Login'  
   , USER AS 'Database Login'  
   , NAME AS 'Context'  
   , TYPE  
   , USAGE   
   FROM sys.user_token     
  
   -- Now get the data  
   SELECT AccountNumber, Name, CreditRating   
   FROM Purchasing.Vendor  
   WHERE CreditRating = 1  
END  
GO  
```  
  
Запустите этот код, чтобы подписывать хранимую процедуру сертификатом базы данных с использованием пароля.  
  
```sql  
ADD SIGNATURE TO TestCreditRatingSP   
   BY CERTIFICATE TestCreditRatingCer  
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y';  
GO  
```  
  
Дополнительные сведения о хранимых процедурах см. в разделе [Хранимые процедуры (компонент Database Engine)](../relational-databases/stored-procedures/stored-procedures-database-engine.md).  
  
Дополнительные сведения о подписывании хранимых процедур см. в разделе [ADD SIGNATURE (Transact-SQL)](../t-sql/statements/add-signature-transact-sql.md).  
  
## <a name="4-create-a-certificate-account-using-the-certificate"></a>4. Создание учетной записи сертификата с помощью сертификата  
Запустите этот код, чтобы создать пользователя базы данных (`TestCreditRatingcertificateAccount`) из сертификата. У этой учетной записи нет имени входа сервера. Она в конечном итоге предназначена для управления доступом к базовым таблицам.  
  
```sql  
USE AdventureWorks2017;  
GO  
CREATE USER TestCreditRatingcertificateAccount  
   FROM CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
## <a name="5-grant-the-certificate-account-database-rights"></a>5. Предоставление учетной записи сертификата прав на базу данных  
Запустите этот код, чтобы предоставить учетной записи `TestCreditRatingcertificateAccount` права на базовую таблицу и хранимую процедуру.  
  
```sql  
GRANT SELECT   
   ON Purchasing.Vendor   
   TO TestCreditRatingcertificateAccount;  
GO  
  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingcertificateAccount;  
GO  
```  
  
Дополнительные сведения о предоставлении разрешений объектам см. в разделе [GRANT (Transact-SQL)](../t-sql/statements/grant-transact-sql.md).  
  
## <a name="6-display-the-access-context"></a>6. Отображение контекста доступа  
Для отображения прав, связанных с доступом хранимой процедуры, запустите следующий код, чтобы предоставить права на запуск хранимой процедуры пользователю `TestCreditRatingUser` .  
  
```sql  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingUser;  
GO  
```  
  
После этого с помощью приведенного ниже кода запустите хранимую процедуру от имени входа dbo, которое было использовано на сервере. Просмотрите вывод сведений о контексте пользователя. Учетная запись dbo будет показана как контекст со своими собственными правами, а не через членство в группе.  
  
```sql  
EXECUTE TestCreditRatingSP;  
GO  
```  
  
Запустите следующий код, чтобы с помощью инструкции `EXECUTE AS` от имени учетной записи `TestCreditRatingUser` выполнить хранимую процедуру. На этот раз будет показано, что задан контекст пользователя USER MAPPED TO CERTIFICATE. Обратите внимание, что этот параметр не поддерживается в автономной базе данных или базе данных SQL Azure или Azure Synapse Analytics.
  
```sql  
EXECUTE AS LOGIN = 'TestCreditRatingUser';  
GO  
EXECUTE TestCreditRatingSP;  
GO  
```  
  
Это означает, что подписывание хранимой процедуры сделало доступным аудит.  
  
> [!NOTE]  
> Использование EXECUTE AS для переключения контекстов в базе данных.  
  
## <a name="7-reset-the-environment"></a>7. Сброс среды  
В приведенном ниже коде с помощью инструкции `REVERT` контекст текущей учетной записи изменяется на dbo. Затем выполняется сброс среды.  
  
```sql  
REVERT;  
GO  
DROP PROCEDURE TestCreditRatingSP;  
GO  
DROP USER TestCreditRatingcertificateAccount;  
GO  
DROP USER TestCreditRatingUser;  
GO  
DROP LOGIN TestCreditRatingUser;  
GO  
DROP CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
Дополнительные сведения об инструкции REVERT см. в разделе [REVERT (Transact-SQL)](../t-sql/statements/revert-transact-sql.md).  
  
## <a name="complete-example"></a><a name="CompleteExample"></a>Пример целиком  
В этом разделе приведен полный код примера.  
  
```sql  
/* Step 1 - Open the AdventureWorks2017 database */  
USE AdventureWorks2017;  
GO  
-- Set up a login for the test user  
CREATE LOGIN TestCreditRatingUser  
   WITH PASSWORD = 'ASDECd2439587y'  
GO  
CREATE USER TestCreditRatingUser  
FOR LOGIN TestCreditRatingUser;  
GO  
  
/* Step 2 - Create a certificate in the AdventureWorks2017 database */  
CREATE CERTIFICATE TestCreditRatingCer  
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
      WITH SUBJECT = 'Credit Rating Records Access',   
      EXPIRY_DATE = '12/31/2021';   -- Error 3701 will occur if this date is not in the future
GO  
  
/* Step 3 - Create a stored procedure and  
sign it using the certificate */  
CREATE PROCEDURE TestCreditRatingSP  
AS  
BEGIN  
   -- Shows who is running the stored procedure  
   SELECT SYSTEM_USER 'system Login'  
   , USER AS 'Database Login'  
   , NAME AS 'Context'  
   , TYPE  
   , USAGE   
   FROM sys.user_token;     
  
   -- Now get the data  
   SELECT AccountNumber, Name, CreditRating   
   FROM Purchasing.Vendor  
   WHERE CreditRating = 1;  
END  
GO  
  
ADD SIGNATURE TO TestCreditRatingSP   
   BY CERTIFICATE TestCreditRatingCer  
    WITH PASSWORD = 'pGFD4bb925DGvbd2439587y';  
GO  
  
/* Step 4 - Create a database user for the certificate.   
This user has the ownership chain associated with it. */  
USE AdventureWorks2017;  
GO  
CREATE USER TestCreditRatingcertificateAccount  
   FROM CERTIFICATE TestCreditRatingCer;  
GO  
  
/* Step 5 - Grant the user database rights */  
GRANT SELECT   
   ON Purchasing.Vendor   
   TO TestCreditRatingcertificateAccount;  
GO  
  
GRANT EXECUTE  
   ON TestCreditRatingSP   
   TO TestCreditRatingcertificateAccount;  
GO  
  
/* Step 6 - Test, using the EXECUTE AS statement */  
GRANT EXECUTE   
   ON TestCreditRatingSP   
   TO TestCreditRatingUser;  
GO  
  
-- Run the procedure as the dbo user, notice the output for the type  
EXEC TestCreditRatingSP;  
GO  
  
EXECUTE AS LOGIN = 'TestCreditRatingUser';  
GO  
EXEC TestCreditRatingSP;  
GO  
  
/* Step 7 - Clean up the example */  
REVERT;  
GO  
DROP PROCEDURE TestCreditRatingSP;  
GO  
DROP USER TestCreditRatingcertificateAccount;  
GO  
DROP USER TestCreditRatingUser;  
GO  
DROP LOGIN TestCreditRatingUser;  
GO  
DROP CERTIFICATE TestCreditRatingCer;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
[Центр безопасности для ядра СУБД SQL Server и Базы данных Azure SQL](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
