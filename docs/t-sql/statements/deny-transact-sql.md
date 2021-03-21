---
description: DENY (Transact-SQL)
title: DENY (Transact-SQL)
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DENY
- DENY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- schema-level securables [SQL Server]
- security [SQL Server], denying access
- DENY statement
- permissions [SQL Server], denying
- server-level securables [SQL Server]
- inherited security settings [SQL Server]
- denying permissions [SQL Server], DENY statement
- principal security [SQL Server]
- database-level securables [SQL Server]
- denying permissions [SQL Server]
ms.assetid: c32d1e01-9ee9-4665-a516-fcfece58078e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce34d8c21230b03161b0552a7e0a569c3b03a469
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754494"
---
# <a name="deny-transact-sql"></a>DENY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Запрещает разрешение для участника. Предотвращает наследование разрешения участником через его членство в группе или роли. DENY имеет приоритет над всеми разрешениями, но не применяется к владельцам объектов или членам с предопределенной ролью сервера sysadmin.
  **Примечание по безопасности.** Членам предопределенной роли сервера sysadmin и владельцам объектов не может быть отказано в разрешениях.
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for DENY  
DENY   { ALL [ PRIVILEGES ] } 
     | <permission>  [ ( column [ ,...n ] ) ] [ ,...n ]  
    [ ON [ <class> :: ] securable ] 
    TO principal [ ,...n ]   
    [ CASCADE] [ AS principal ]  
[;]

<permission> ::=  
{ see the tables below }  
  
<class> ::=  
{ see the tables below }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DENY   
    <permission> [ ,...n ]  
    [ ON [ <class_> :: ] securable ]   
    TO principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class> ::=  
{  
      LOGIN  
    | DATABASE  
    | OBJECT  
    | ROLE  
    | SCHEMA  
    | USER  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 ALL  
 Этот параметр запрещает не все возможные разрешения. Его использование эквивалентно запрету следующих разрешений.  
  
-   Если защищаемым объектом является база данных, аргумент ALL относится к разрешениям BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE и CREATE VIEW.  
  
-   Если защищаемым объектом является скалярная функция, аргумент ALL относится к разрешениям EXECUTE и REFERENCES.  
  
-   Если защищаемым объектом является функция с табличным значением, аргумент ALL относится к разрешениям DELETE, INSERT, REFERENCES, SELECT и UPDATE.  
  
-   Если защищаемая сущность является хранимой процедурой, аргумент ALL подразумевает разрешение EXECUTE.  
  
-   Если защищаемым объектом является таблица, аргумент ALL относится к разрешениям DELETE, INSERT, REFERENCES, SELECT и UPDATE.  
  
-   Если защищаемым объектом является представление, аргумент ALL относится к разрешениям DELETE, INSERT, REFERENCES, SELECT и UPDATE.  
  
> [!NOTE]  
>  Синтаксис DENY ALL является устаревшим. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого отменяйте определенные разрешения.  
  
 PRIVILEGES  
 Включено для обеспечения совместимости с требованиями ISO. Не изменяет работу ALL.  
  
 *permission*  
 Имя разрешения. Допустимые сопоставления разрешений защищаемых объектов описаны в разделах, перечисленных ниже.  
  
 *column*  
 Указывает имя столбца в таблице, на который запрещаются разрешения. Необходимы скобки ().  
  
 *class*  
 Указывает класс защищаемого объекта, на который запрещается разрешение. Квалификатор области **::** является обязательным.  
  
 *securable*  
 Указывает защищаемый объект, на который запрещается разрешение.  
  
 TO *principal*  
 Имя участника. Участники, у которых может запрещаться разрешение на защищаемый объект, в зависимости от самого защищаемого объекта. Допустимые сочетания приведены в указанных ниже разделах по конкретным защищаемым объектам.  
  
 CASCADE  
 Обозначает, что разрешение запрещается для указанного участника и всех других участников, которым этот участник предоставил разрешение. Требуется, если участник имеет разрешение с параметром GRANT OPTION.  
  
 AS *principal*  
 Указывает участника, от которого участник, выполняющий данный запрос, получает право на запрещение разрешения.
Используйте предложение субъекта AS, чтобы указать, что субъект, записанный как объект, отзывающий разрешение, должен быть субъектом, отличным от пользователя, выполняющего инструкцию. Предположим, что пользователь Мария — это участник 12, пользователь Павел — участник 15. Мария выполняет `DENY SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. В таблице sys.database_permissions для параметра grantor_prinicpal_id инструкции отмены указано значение 15 (Павел), хотя инструкция была выполнена пользователем 12 (Мария).
  
AS в данной инструкции не дает возможность олицетворять другого пользователя.  
  
## <a name="remarks"></a>Remarks  
 Полный синтаксис инструкции DENY достаточно сложен. Предыдущая диаграмма синтаксиса была упрощена, чтобы продемонстрировать ее структуру. Полный синтаксис запрета разрешений на конкретные защищаемые объекты описан в разделах, перечисленных ниже.  
  
 Инструкция DENY завершится ошибкой, если не указан аргумент CASCADE при отзыве разрешения у участника, которому это разрешение было предоставлено с параметром GRANT OPTION.  
  
 Системная хранимая процедура sp_helprotect сообщает о разрешениях на доступ к защищаемым объектам уровня базы данных.  
  
> [!CAUTION]  
>  Запрет (DENY) уровня таблицы имеет меньший приоритет, чем разрешение (GRANT) уровня столбца. Эта несовместимость в иерархии разрешений предусмотрена в целях гарантии обратной совместимости. В будущем выпуске она будет удалена.  
  
> [!CAUTION]  
>  При запрете разрешения CONTROL на базу данных неявно запрещается разрешение CONNECT на эту базу данных. Участник, для которого запрещено разрешение CONTROL на базу данных, не сможет подключиться к этой базе данных.  
  
> [!CAUTION]  
>  При запрете разрешения CONTROL SERVER неявно запрещается разрешение CONNECT SQL на этот сервер. Участник, для которого запрещено разрешение CONTROL SERVER на сервер, не сможет подключиться к этому серверу.  
  
## <a name="permissions"></a>Разрешения  
 Участник, указанный параметром AS, должен иметь либо разрешение CONTROL, либо разрешение более высокого уровня, включающее в себя разрешение на защищаемый объект. При использовании параметра AS указанный участник должен быть владельцем защищаемого объекта, разрешение на который запрещается.  
  
 Участник, получивший разрешение CONTROL SERVER, например член предопределенной роли сервера sysadmin, может запретить любое разрешение на любой защищаемый объект сервера. Участник, получивший разрешение CONTROL на базу данных, например член предопределенной роли базы данных db_owner, может запретить любое разрешение на любой защищаемый объект в базе данных. Участник, получивший разрешение CONTROL на схему, может запретить любое разрешение на любой защищаемый объект в этой схеме. При использовании предложения AS указанный участник должен быть владельцем защищаемого объекта, разрешения на который для него запрещаются.  
  
## <a name="examples"></a>Примеры

 В следующей таблице перечислены защищаемые объекты и разделы, в которых описывается синтаксис инструкций по работе с ними.  
  
|Защищаемые объекты|Синтаксис|
|----------|------|
|Роль приложения|[DENY, запрет разрешений на участника базы данных (Transact-SQL)](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Сборка|[DENY, запрет разрешений на сборку (Transact-SQL)](../../t-sql/statements/deny-assembly-permissions-transact-sql.md)|  
|Асимметричный ключ|[DENY, запрет разрешений на асимметричный ключ (Transact-SQL)](../../t-sql/statements/deny-asymmetric-key-permissions-transact-sql.md)|  
|Группа доступности|[DENY, запрет разрешений на группу доступности (Transact-SQL)](../../t-sql/statements/deny-availability-group-permissions-transact-sql.md)|  
|Сертификат|[DENY, запрет разрешений на сертификат (Transact-SQL)](../../t-sql/statements/deny-certificate-permissions-transact-sql.md)|  
|Контракт|[DENY, запрет разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|База данных|[DENY, запрет разрешений на базу данных (Transact-SQL)](../../t-sql/statements/deny-database-permissions-transact-sql.md)|  
|Учетные данные для базы данных|[DENY, запрет разрешений на учетные данные для базы данных (Transact-SQL)](../../t-sql/statements/deny-database-scoped-credential-transact-sql.md)|  
|Конечная точка|[DENY, запрет разрешений на конечную точку (Transact-SQL)](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)|  
|Полнотекстовый каталог|[DENY, запрет разрешений на полнотекстовые объекты (Transact-SQL)](../../t-sql/statements/deny-full-text-permissions-transact-sql.md)|  
|Полнотекстовый список стоп-слов|[DENY, запрет разрешений на полнотекстовые объекты (Transact-SQL)](../../t-sql/statements/deny-full-text-permissions-transact-sql.md)|  
|Компонент|[DENY, запрет разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Имя входа|[DENY, запрет разрешений участника на уровне сервера (Transact-SQL)](../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)|  
|Тип сообщений|[DENY, запрет разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Объект|[DENY, запрет разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Очередь|[DENY, запрет разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Привязка удаленной службы|[DENY, запрет разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Роль|[DENY, запрет разрешений на участника базы данных (Transact-SQL)](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Маршрут|[DENY, запрет разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|схема|[DENY, запрет разрешений на схему (Transact-SQL)](../../t-sql/statements/deny-schema-permissions-transact-sql.md)|  
|Список свойств поиска|[DENY, запрет разрешений на список свойств поиска (Transact-SQL)](../../t-sql/statements/deny-search-property-list-permissions-transact-sql.md)|  
|Сервер|[DENY, запрет разрешений на сервере (Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md)|  
|Служба|[DENY, запрет разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)|  
|Хранимая процедура|[DENY, запрет разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Симметричный ключ|[DENY, запрет разрешений на симметричный ключ (Transact-SQL)](../../t-sql/statements/deny-symmetric-key-permissions-transact-sql.md)|  
|Синоним|[DENY, запрет разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Системные объекты|[DENY, запрет разрешений на системный объект (Transact-SQL)](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)|  
|Таблица|[DENY, запрет разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Тип|[DENY, запрет разрешений на тип (Transact-SQL)](../../t-sql/statements/deny-type-permissions-transact-sql.md)|  
|Пользователь|[DENY, запрет разрешений на участника базы данных (Transact-SQL)](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)|  
|Представление|[DENY, запрет разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)|  
|Коллекция схем XML|[DENY, запрет разрешений на коллекцию XML-схем (Transact-SQL)](../../t-sql/statements/deny-xml-schema-collection-permissions-transact-sql.md)|  
  
## <a name="see-also"></a>См. также:  
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_changedbowner (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_dropuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helprotect (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)  
  
  
