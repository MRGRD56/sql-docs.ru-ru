---
description: Инструкция GRANT (Transact-SQL)
title: Инструкция GRANT (Transact-SQL)
ms.custom: ''
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GRANT_TSQL
- GRANT
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], GRANT statement
- schema-level securables [SQL Server]
- GRANT statement
- cross-database permissions
- GRANT statement, about GRANT statement
- server-level securables [SQL Server]
- database-level securables [SQL Server]
- permissions [SQL Server], granting
ms.assetid: a760c16a-4d2d-43f2-be81-ae9315f38185
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: efdce09afb69773f068735356c83b88d5f61508d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740084"
---
# <a name="grant-transact-sql"></a>Инструкция GRANT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Предоставляет разрешения на защищаемый объект участнику.  Общая концепция заключается в GRANT \<some permission> ON \<some object> TO \<some user, login, or group>. Общее описание разрешений см. в разделе [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md).  
  
 ![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for GRANT  
GRANT { ALL [ PRIVILEGES ] }  
      | permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      [ ON [ class :: ] securable ] TO principal [ ,...n ]   
      [ WITH GRANT OPTION ] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
GRANT
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class_type> ::=  
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
Этот параметр устарел и сохранен только для поддержки обратной совместимости. Он не предоставляет все возможные разрешения. Предоставление разрешения ALL эквивалентно предоставлению следующих разрешений:

  - Если защищаемым объектом является база данных, аргумент ALL относится к разрешениям BACKUP DATABASE, BACKUP LOG, CREATE DATABASE, CREATE DEFAULT, CREATE FUNCTION, CREATE PROCEDURE, CREATE RULE, CREATE TABLE и CREATE VIEW.  
  
  - Если защищаемым объектом является скалярная функция, аргумент ALL относится к разрешениям EXECUTE и REFERENCES.  
  
  - Если защищаемым объектом является функция с табличным значением, аргумент ALL относится к разрешениям DELETE, INSERT, REFERENCES, SELECT и UPDATE.  
  
  - Если защищаемая сущность является хранимой процедурой, аргумент ALL подразумевает разрешение EXECUTE.  
  
  - Если защищаемым объектом является таблица, аргумент ALL относится к разрешениям DELETE, INSERT, REFERENCES, SELECT и UPDATE.  
  
  - Если защищаемым объектом является представление, аргумент ALL относится к разрешениям DELETE, INSERT, REFERENCES, SELECT и UPDATE.  

PRIVILEGES  
Включено для обеспечения совместимости с требованиями ISO. Не изменяет работу ALL.  
  
*permission*  
Имя разрешения. Допустимые сопоставления разрешений защищаемых объектов описаны в разделах, перечисленных ниже.  
  
*column*  
Указывает имя столбца таблицы, на который предоставляется разрешение. Необходимы скобки ().  
  
*class*  
Указывает класс защищаемого объекта, для которого предоставляется разрешение. Квалификатор области **::** является обязательным.  
  
*securable*  
Указывает защищаемый объект, на который предоставляется разрешение.  
  
TO *principal*  
Имя участника. Состав участников, которым можно предоставлять разрешения, меняется в зависимости от защищаемого объекта. Допустимые сочетания описаны в разделах, перечисленных ниже.  
  
GRANT OPTION  
Показывает, что получающему разрешению будет также дана возможность предоставлять указанное разрешение другим участникам.  
  
AS *principal*  
Используйте предложение AS principal, чтобы указать, что участник, записанный как предоставляющий разрешение, должен быть участником, отличным от пользователя, выполняющего инструкцию. Предположим, что пользователь Мария — это участник 12, пользователь Павел — участник 15. Мария выполняет `GRANT SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. В таблице sys.database_permissions для параметра grantor_prinicpal_id будет указано значение 15 (Павел), хотя инструкция была выполнена пользователем 12 (Мария).  

Использовать предложение AS обычно не рекомендуется, за исключением необходимости явного определения цепочки разрешения. Дополнительные сведения см. в подразделе **Сводка по алгоритму проверки разрешений** раздела [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md).

AS в данной инструкции не дает возможность олицетворять другого пользователя.

## <a name="remarks"></a>Remarks

Полное описание синтаксиса инструкции GRANT сложно. Предыдущая диаграмма синтаксиса была упрощена, чтобы продемонстрировать ее структуру. Полное описание синтаксиса предоставления разрешений на конкретные защищаемые объекты приведено в статьях, перечисленных ниже.  

Инструкция REVOKE может использоваться для удаления уже выданных прав доступа, а инструкция DENY может использоваться, чтобы предотвратить получение участником определенного разрешения посредством инструкции GRANT.  

Предоставление разрешения удаляет DENY или REVOKE для этого разрешения на данный защищаемый объект. Если то же разрешение запрещено для более высокой области действия, которая содержит данный защищаемый объект, то DENY имеет более высокий приоритет. Но отмена разрешения, выданного в более высокой области действия, не получает такого приоритета.  

Разрешения уровня базы данных выдаются в пределах области указанной базы данных. Если пользователю нужны разрешения на объекты в другой базе данных, необходимо создать для него учетную запись в этой базе данных или предоставить ему разрешение на доступ как к текущей, так и к другим базам данных.  

> [!CAUTION]  
> Запрет (DENY) уровня таблицы имеет меньший приоритет, чем разрешение (GRANT) уровня столбца. Эта несовместимость в иерархии разрешений предусмотрена в целях гарантии обратной совместимости. В будущем выпуске она будет удалена.  

Системная хранимая процедура sp_helprotect сообщает о разрешениях на доступ к защищаемым объектам уровня базы данных.  

## <a name="with-grant-option"></a>WITH GRANT OPTION

Инструкция **GRANT** ... **WITH GRANT OPTION**  указывает, что получающему разрешение участнику безопасности дана возможность предоставления определенных разрешений другим учетным записям безопасности. Если участник, получивший данное разрешение, является ролью или группой Windows, то для последующего предоставления разрешения объекта пользователям, которые не являются членами этой группы или роли, необходимо использовать предложение **AS**. Поскольку именно пользователь, а не группа или роль, может исполнить инструкцию **GRANT**, при предоставлении разрешения определенный член группы или роли должен использовать предложение **AS** для явного вызова членства в роли или группе. В следующем примере рассматривается порядок использования разрешения **WITH GRANT OPTION**, предоставленного роли или группе Windows.  
  
```sql
-- Execute the following as a database owner  
GRANT EXECUTE ON TestProc TO TesterRole WITH GRANT OPTION;  
EXEC sp_addrolemember TesterRole, User1;  
-- Execute the following as User1  
-- The following fails because User1 does not have the permission as the User1  
GRANT EXECUTE ON TestMe TO User2;  
-- The following succeeds because User1 invokes the TesterRole membership  
GRANT EXECUTE ON TestMe TO User2 AS TesterRole;  
```

## <a name="chart-of-sql-server-permissions"></a>Диаграмма разрешений SQL Server

Схему плакатного размера всех разрешений компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] в формате PDF см. по ссылке [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster).  

## <a name="permissions"></a>Разрешения

Объект, предоставляющий разрешение (или участник, указанный параметром AS), должен иметь либо само разрешение, выданное с помощью параметра GRANT OPTION, либо разрешение более высокого уровня, которое неявно включает предоставляемое. Если используется параметр AS, налагаются дополнительные ограничения. Дополнительные сведения см. в статьях, посвященных конкретным защищаемым объектам.

Владельцы объектов могут предоставлять разрешения на объекты, которыми они владеют. Участники, имеющие разрешение CONTROL на защищаемый объект, могут предоставлять разрешение на этот защищаемый объект.

Участники, которым предоставлено разрешение CONTROL SERVER, такие как члены предопределенной роли сервера &lt;legacyBold&gt;sysadmin&lt;/legacyBold&gt;, могут предоставлять любое разрешение на любой защищаемый объект сервера. Участники, получившие разрешение CONTROL на базу данных, такие как члены предопределенной роли базы данных db_owner, могут предоставлять любое разрешение на любой защищаемый объект в базе данных. Владельцы разрешения CONTROL, связанного со схемой, могут предоставлять любые разрешения на работу с любыми объектами, содержащимися в данной схеме.

## <a name="examples"></a>Примеры

В приведенной ниже таблице перечислены защищаемые объекты и статьи, в которых описывается синтаксис инструкций по работе с ними.  

| Защищаемый объект | Синтаксис GRANT|
| ---------| ------ |
|Роль приложения|[GRANT, предоставление разрешений на участника базы данных (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Сборка|[GRANT, предоставление разрешений на сборку (Transact-SQL)](../../t-sql/statements/grant-assembly-permissions-transact-sql.md)|  
|Асимметричный ключ|[GRANT, предоставление разрешений на асимметричный ключ (Transact-SQL)](../../t-sql/statements/grant-asymmetric-key-permissions-transact-sql.md)|  
|Группа доступности|[GRANT, предоставление разрешений на группу доступности (Transact-SQL)](../../t-sql/statements/grant-availability-group-permissions-transact-sql.md)|  
|Сертификат|[GRANT, предоставление разрешений на сертификат (Transact-SQL)](../../t-sql/statements/grant-certificate-permissions-transact-sql.md)|  
|Контракт|[GRANT, предоставление разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|База данных|[GRANT, предоставление разрешений на базу данных (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)|
|Учетные данные для базы данных|[GRANT, предоставление разрешений на учетные данные для базы данных (Transact-SQL)](../../t-sql/statements/grant-database-scoped-credential-transact-sql.md)|  
|Конечная точка|[GRANT, предоставление разрешений на конечную точку (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)|  
|Полнотекстовый каталог|[GRANT, предоставление разрешений на полнотекстовые объекты (Transact-SQL)](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|Полнотекстовый список стоп-слов|[GRANT, предоставление разрешений на полнотекстовые объекты (Transact-SQL)](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|Компонент|[GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Имя входа|[GRANT, предоставление разрешений участникам на уровне сервера (Transact-SQL)](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)|  
|Тип сообщений|[GRANT, предоставление разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Объект|[GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Очередь|[GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Привязка удаленной службы|[GRANT, предоставление разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Роль|[GRANT, предоставление разрешений на участника базы данных (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Маршрут|[GRANT, предоставление разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|схема|[GRANT, предоставление разрешений на схему (Transact-SQL)](../../t-sql/statements/grant-schema-permissions-transact-sql.md)|  
|Список свойств поиска|[GRANT, предоставление разрешений на список свойств поиска (Transact-SQL)](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)|  
|Сервер|[GRANT, предоставление разрешений на сервер (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)|  
|Служба|[GRANT, предоставление разрешений на Service Broker (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|Хранимая процедура|[GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Симметричный ключ|[GRANT, предоставление разрешений на симметричный ключ (Transact-SQL)](../../t-sql/statements/grant-symmetric-key-permissions-transact-sql.md)|  
|Синоним|[GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Системные объекты|[GRANT, предоставление разрешения на системный объект (Transact-SQL)](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)|  
|Таблица|[GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Тип|[GRANT, предоставление разрешений на тип (Transact-SQL)](../../t-sql/statements/grant-type-permissions-transact-sql.md)|  
|Пользователь|[GRANT, предоставление разрешений на участника базы данных (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Представление|[GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|Коллекция схем XML|[GRANT, предоставление разрешений на коллекцию XML-схем (Transact-SQL)](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)|  

## <a name="see-also"></a>См. также:

- [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)
- [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)
- [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)
- [sp_adduser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)
- [sp_changedbowner (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)
- [sp_dropuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)
- [sp_helprotect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)
- [sp_helpuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)
