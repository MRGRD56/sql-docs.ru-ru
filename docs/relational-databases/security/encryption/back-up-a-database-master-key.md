---
title: Создание резервной копии главного ключа базы данных | Документация Майкрософт
description: Узнайте, как создавать резервную копию главного ключа базы данных в SQL Server с помощью Transact-SQL. Этот важный ключ шифрует другие ключи и сертификаты.
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], exporting
ms.assetid: 7ad9a0a0-6e4f-4f7b-8801-8c1b9d49c4d8
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 92e8753033e37561b23f2e1d15cb06cef4b662d7
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250692"
---
# <a name="back-up-a-database-master-key"></a>Создание резервной копии главного ключа базы данных
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как выполнить резервное копирование главного ключа [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] с помощью [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Главный ключ базы данных используется для шифрования других ключей и сертификатов внутри базы данных. Если он удален или поврежден, то есть вероятность, что [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не сможет расшифровать эти ключи, в результате чего зашифрованные с их помощью данные будут безвозвратно утеряны. По этой причине необходимо создать резервную копию главного ключа базы данных и хранить ее в надежном месте.  
  
## <a name="before-you-begin"></a>Перед началом  
  
### <a name="limitations-and-restrictions"></a>Ограничения  
  
- Главный ключ должен быть открыт и, таким образом, расшифрован, прежде чем производится его резервное копирование. Если он зашифрован главным ключом службы, то его не нужно открывать явным образом. Но если главный ключ зашифрован только паролем, его явное открытие обязательно.  
  
- Рекомендуется создать резервную копию главного ключа сразу же после его создания и затем сохранить в надежном месте.  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения
Требует разрешения CONTROL для базы данных.  
  
## <a name="using-sql-server-management-studio-with-transact-sql"></a>В среде SQL Server Management Studio с помощью Transact-SQL  
  
### <a name="to-back-up-the-database-master-key"></a>Создание резервной копии главного ключа базы данных  
  
1. В среде [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]установите соединение с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , где содержится главный ключ базы данных, резервную копию которого необходимо создать.  
  
2. Выберите пароль, который будет использоваться для шифрования главного ключа базы данных на носителе данных резервной копии. Пароль проходит проверку сложности.  
  
3. Получите съемный носитель данных резервной копии для хранения главного ключа.  
  
4. Укажите каталог NTFS, в котором будет создана резервная копия главного ключа. В этом каталоге будет сохранен файл, созданный в следующем шаге. Он должен быть защищен с помощью списка управления доступом со строгими ограничениями.  
  
5. В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
6. На стандартной панели выберите пункт **Создать запрос**.  
  
7. Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```sql
    -- Creates a backup of the "AdventureWorks2012" master key. Because this master key is not encrypted by the service master key, a password must be specified when it is opened.  
    USE AdventureWorks2012;   
    GO  
    OPEN MASTER KEY DECRYPTION BY PASSWORD = 'sfj5300osdVdgwdfkli7';   
  
    BACKUP MASTER KEY TO FILE = 'c:\temp\exportedmasterkey'   
        ENCRYPTION BY PASSWORD = 'sd092735kjn$&adsg';   
    GO  
    ```  
  
    > [!NOTE]  
    > Путь к файлу ключа и пароль ключа (если он существует) будет отличаться от вышеуказанного. Убедитесь, что оба этих параметра соответствуют настройке конкретного сервера и ключа.  
  
8. Скопируйте файл на носитель данных резервных копий и проверьте правильность копирования.  
  
9. Сохраните резервную копию в надежном месте вне системы.  
  
 Дополнительные сведения см. в разделах [OPEN MASTER KEY (Transact-SQL)](../../../t-sql/statements/open-master-key-transact-sql.md) и [BACKUP MASTER KEY (Transact-SQL)](../../../t-sql/statements/backup-master-key-transact-sql.md).  
