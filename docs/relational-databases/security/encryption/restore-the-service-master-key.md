---
title: Восстановление главного ключа службы | Документация Майкрософт
description: Узнайте, как восстановить главный ключ службы в SQL Server с помощью Transact-SQL. Главный ключ службы является корнем иерархии шифрования SQL Server.
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- service master key [SQL Server], importing
- service master key [SQL Server], restoring
ms.assetid: 14bdbbbe-d384-4692-b670-4243d2466fe1
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 8df8f8a8a01b8ade5372147f274d329baf346ffd
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250175"
---
# <a name="restore-the-service-master-key"></a>Восстановление главного ключа службы
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается, как восстановить главный ключ службы [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] при помощи [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
> [!WARNING]  
> Маловероятно, что когда-нибудь потребуется восстановление главного ключа службы, Но если все же придется это делать, эту операцию следует выполнять предельно внимательно. Дополнительные сведения см. в статье [Back Up the Service Master Key](../../../relational-databases/security/encryption/back-up-the-service-master-key.md).  
  
## <a name="before-you-begin"></a>Перед началом  
  
### <a name="limitations-and-restrictions"></a>Ограничения  
  
- При восстановлении главного ключа службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] расшифровывает все ключи и секретные коды, зашифрованные вместе с текущим главным ключом службы, а затем зашифровывает их вместе с восстановленным ключом из файла резервной копии.  
  
- Если во время расшифровки любого объекта произойдет ошибка, восстановление будет прервано. Чтобы пропускать ошибки, можно использовать параметр FORCE, но это приведет к потере всех данных, которые не удается расшифровать.  
  
- Повторное формирование иерархии шифрования — ресурсоемкая операция. Ее нужно назначить на период низкой нагрузки на сервер.  
  
> [!CAUTION]  
> Главный ключ службы является корнем иерархии шифрования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Этот ключ явно или неявно защищает все остальные ключи в дереве. Если зависящий от него ключ не может быть расшифрован, но восстановление продолжено, то данные, защищенные этим ключом, будут утеряны.  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
Необходимо разрешение CONTROL SERVER на сервер.  
  
## <a name="using-transact-sql"></a>Использование Transact-SQL  
  
### <a name="to-restore-the-service-master-key"></a>Восстановление главного ключа службы  
  
1. Сохраните резервную копию главного ключа службы с физического носителя данных резервных копий или из папки локальной файловой системы.  
  
2. В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
3. На стандартной панели выберите пункт **Создать запрос**.  
  
4. Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```sql
    -- Restores the service master key from a backup file.  
    RESTORE SERVICE MASTER KEY   
        FROM FILE = 'c:\temp_backups\keys\service_master_key'   
        DECRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';  
    GO  
    ```  
  
    > [!NOTE]  
    > Путь к файлу ключа и пароль ключа (если он существует) будет отличаться от вышеуказанного. Убедитесь, что оба этих параметра соответствуют настройке конкретного сервера и ключа.
