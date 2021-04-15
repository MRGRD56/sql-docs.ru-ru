---
title: Конфигурация "common criteria compliance enabled"
description: Сведения о том, какие критерии в SQL Server активируются параметром "common criteria compliance enabled". Узнайте, как обеспечить соответствие стандарту Common Criteria заданного уровня. Для получения сертификации EUCC. Международное обязательство по соответствию нормативным требованиям в регулируемых отраслях и органах.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- common criteria compliance
helpviewer_keywords:
- CC (common criteria) [Database Engine]
- common criteria compliance [Database Engine]
- Risidual Information Protection [Database Engine]
- RIP (Residual Information Protection)
author: markingmyname
ms.author: maghan
ms.reviewer: wopeter
ms.custom: ''
ms.date: 04/07/2021
ms.openlocfilehash: 8d601d227ebfc63007cb0af9cd859316d5c93357
ms.sourcegitcommit: 09122d02fc3d86c6028366653337c083da8a3f4c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2021
ms.locfileid: "107072426"
---
# <a name="common-criteria-compliance-enabled-server-configuration"></a>Конфигурация сервера "common criteria compliance enabled"

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Параметр common criteria compliance включает следующие элементы, необходимые для поддержки стандарта [Общие критерии оценки защищенности информационных технологий](https://www.commoncriteriaportal.org). Требование для международного обязательства по соответствию нормативным требованиям в регулируемых отраслях и органах.

| Критерии | Описание |
|----------|-------------|
| Защита остаточных данных (RIP) | Критерий RIP требует, чтобы выделяемая память была перезаписана известным битовым шаблоном, прежде чем она будет перераспределена для другого ресурса. Соответствие стандарту RIP может повысить безопасность, однако может привести к снижению производительности. После включения параметра common criteria compliance enabled производится перезапись памяти. |
|Возможность просматривать статистику имени входа | После включения параметра common criteria compliance enabled включается аудит входа. </br></br></br> Время входа, доступное для каждого сеанса при каждом успешном входе пользователя на SQL Server: </br> – сведения о времени последнего успешного входа, </br> – время последнего неудачного входа, </br> – число попыток между последним успешным входом и текущим входом. </br></br></br> Чтобы увидеть статистику входа, запросите динамическое административное представление [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) . |
|Этот столбец `GRANT` не должен переопределять таблицу `DENY`. | После включения параметра common criteria compliance enabled запрет `DENY` на уровне таблицы имеет больший приоритет, чем разрешение `GRANT` на уровне столбца. Если этот параметр выключен, то `GRANT` на уровне столбца имеет больший приоритет, чем `DENY` на уровне таблицы. |

Параметр common criteria compliance enabled является дополнительным. Общие условия оцениваются и сертифицируются только для выпусков Enterprise и Datacenter. Последнее состояние сертификации общих условий см. на сайте [Стандарт Common Criteria для Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkId=616319).

> [!IMPORTANT]
> Помимо включения параметра common criteria compliance enabled необходимо скачать и выполнить скрипт, завершающий настройку SQL Server для соответствия стандарту Common Criteria уровня 4+ (EAL4+). Этот скрипт можно скачать с сайта [Стандарт Common Criteria для Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkId=616319).

Если для изменения значения параметра используется системная хранимая процедура `sp_configure`, изменить параметр common criteria compliance enabled можно только в том случае, если параметр show advanced options имеет значение 1. Установка параметра вступает в силу после перезапуска сервера. Возможные значения — 0 и 1.

- Значение 0 указывает, что соответствие стандарту Common Criteria не включено (по умолчанию).

- Значение 1 указывает, что соответствие стандарту Common Criteria включено.

## <a name="examples"></a>Примеры

Следующий пример включает соответствие стандарту Common Criteria.

```sql
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'common criteria compliance enabled', 1;
GO
RECONFIGURE WITH OVERRIDE;
GO
```

Перезапуск SQL Server.

## <a name="next-steps"></a>Дальнейшие шаги

- [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
