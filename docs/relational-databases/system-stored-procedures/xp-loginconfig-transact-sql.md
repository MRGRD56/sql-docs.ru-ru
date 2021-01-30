---
description: xp_loginconfig (Transact-SQL)
title: xp_loginconfig (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_loginconfig_TSQL
- xp_loginconfig
dev_langs:
- TSQL
helpviewer_keywords:
- xp_loginconfig
ms.assetid: d380e799-2857-408a-bcbf-5e73a8e6aa5a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: da53f9c3666a4d4d1e75136dd81caa45c9aaee81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124416"
---
# <a name="xp_loginconfig-transact-sql"></a>xp_loginconfig (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о конфигурации безопасности входа в систему для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
xp_loginconfig ['config_name']  
```  
  
## <a name="arguments"></a>Аргументы  
 **"** *config_name* **"**  
 Отображаемое значение конфигурации. Если *config_name* не указан, выводятся все значения конфигурации. Аргумент *config_name* имеет тип **sysname**, значение по умолчанию NULL и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**режим входа**|Режим безопасности имени входа в систему. Возможные значения: **Mixed** и **Windows Authentication**.<br /><br /> Заменяется на:<br /><br /> `SELECT SERVERPROPERTY('IsIntegratedSecurityOnly'); GO`|  
|**default login**|Имя идентификатора входа сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию для авторизованных пользователей доверенных подключений (для пользователей без соответствующего имени входа). Имя входа по умолчанию — **Guest**. Данное значение предоставляется для обеспечения обратной совместимости.|  
|**Домен по умолчанию**|Имя домена Windows по умолчанию для сетевых пользователей доверенных подключений. Доменом по умолчанию является домен компьютера Windows, на котором выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Данное значение предоставляется для обеспечения обратной совместимости.|  
|**уровень аудита**|Уровень аудита. Возможные значения: **None**, **Success**, **Failure** и **ALL**. Результаты аудита записываются в журнал ошибок и в окно просмотра событий Windows.|  
|**set hostname**|Указывает, заменяется ли имя узла из учетной записи клиента на сетевое имя пользователя Windows. Возможными значениями являются **true** или **false**. Если этот параметр задан, имя пользователя сети отображается в выходных данных **sp_who**.|  
|**map _**|Сообщает, какие специальные символы Windows поставлены в соответствие допустимому символу подчеркивания (_) сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Возможные значения: **Разделитель домена** (по умолчанию), **пробел**, **null** или любой отдельный символ. Данное значение предоставляется для обеспечения обратной совместимости.|  
|**Map $**|Сообщает, какие специальные символы Windows поставлены в соответствие допустимому символу знака доллара ($) сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Возможные значения: **Разделитель домена**, **пробел**, **значение NULL** или любой отдельный символ. Значение по умолчанию — **Space**. Данное значение предоставляется для обеспечения обратной совместимости.|  
|**Таблица #**|Сообщает, какие специальные символы Windows поставлены в соответствие допустимому символу знака «решетка» (#) сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Возможные значения: **Разделитель домена**, **пробел**, **значение NULL** или любой отдельный символ. Значение по умолчанию — дефис (-). Данное значение предоставляется для обеспечения обратной совместимости.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Параметр конфигурации|  
|**config value**|**sysname**|Значение параметра конфигурации|  
  
## <a name="remarks"></a>Замечания  
 **xp_loginconfig** нельзя использовать для установки значений конфигурации.  
  
 Определение режима входа и уровня аудита производится в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение CONTROL для базы данных **master** .  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-how-to-report-all-configuration-values"></a>A. Отчет со значениями всех параметров конфигурации  
 В следующем примере выводятся все текущие значения параметров конфигурации.  
  
```  
EXEC xp_loginconfig;  
GO  
```  
  
### <a name="b-how-to-report-a-specific-configuration-value"></a>Б. Отчет со значением конкретного параметра конфигурации  
 В следующем примере выводится значение режима входа в систему.  
  
```  
EXEC xp_loginconfig 'login mode';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Хранимая процедура sp_denylogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [Хранимая процедура sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Хранимая процедура sp_revokelogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [xp_logininfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-logininfo-transact-sql.md)  
  
  
