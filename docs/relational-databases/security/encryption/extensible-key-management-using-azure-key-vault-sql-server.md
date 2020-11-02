---
title: Расширенное управление ключами с помощью Azure Key Vault
description: Используйте соединитель SQL Server для расширенного управления ключами с помощью Azure Key Vault для SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/22/2016
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Extensible Key Management with key vault
- Transparent Data Encryption, using EKM and key vault
- EKM, with key vault
- TDE, EKM and key vault
- Key Management with key vault
- SQL Server Connector, about
ms.assetid: 3efdc48a-8064-4ea6-a828-3fbf758ef97c
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: a70ae767aa0f9ed079b616402f3857e03fc3d9dc
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92679065"
---
# <a name="extensible-key-management-using-azure-key-vault-sql-server"></a>Расширенное управление ключами с помощью хранилища ключей Azure (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Соединитель [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для хранилища ключей [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Azure позволяет функции шифрования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] использовать службу хранилища ключей Azure в качестве поставщика [расширенного управления ключами &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md), чтобы защитить ключи шифрования [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 В этой статье описывается Соединитель [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в статьях [Этапы настройки расширенного управления ключами с использованием хранилища ключей Azure](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md), [Использование соединителя SQL Server с компонентами шифрования SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)и [Соединитель SQL Server, приложение](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).  
  
##  <a name="what-is-extensible-key-management-ekm-and-why-use-it"></a><a name="Uses"></a> Что такое расширенное управление ключами (EKM) и для чего оно нужно?  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] предоставляет несколько видов шифрования для защиты конфиденциальных данных, включая [прозрачное шифрование данных (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md), [шифрование на уровне столбцов (CLE)](../../../t-sql/functions/cryptographic-functions-transact-sql.md) и [шифрование резервной копии](../../../relational-databases/backup-restore/backup-encryption.md). Во всех этих случаях в рамках этой традиционной иерархии ключей данные шифруются с помощью симметричного ключа шифрования (DEK). Симметричный ключ шифрования затем шифруется иерархией ключей, хранящихся в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Альтернативой этой модели является модель поставщика расширенного управления ключами. Архитектура поставщика расширенного управления ключами позволяет [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] защитить ключи шифрования данных с помощью асимметричного ключа, который хранится за пределами [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] во внешнем поставщике служб шифрования. Эта модель добавляет дополнительный уровень безопасности и разделяет управление ключами и данными.  
   
 На следующем рисунке традиционная управляемая службой иерархия ключей сравнивается с хранилищем ключей Azure.  
  
 ![Схема, на которой традиционная управляемая службой иерархия ключей сравнивается с системой Azure Key Vault.](../../../relational-databases/security/encryption/media/ekm-key-hierarchy-traditional.png "ekm-key-hierarchy-traditional")  
  
   
 Соединитель [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] выступает в качестве моста между [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и хранилищем ключей Azure, поэтому [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] может использовать масштабируемость, высокий уровень производительности и высокую доступность службы хранилища ключей Azure. На следующем рисунке показано, как иерархия ключей работает в архитектуре поставщика расширенного управления ключами с хранилищем ключей Azure и Соединителем [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
  Хранилище ключей можно использовать с установками [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на виртуальных машинах [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Azure и для локальных серверов. Служба хранилища ключей также предоставляет возможность использовать жестко контролируемые и отслеживаемые аппаратные модули безопасности (HSM) для обеспечения более высокого уровня защиты асимметричных ключей шифрования. Дополнительные сведения о хранилище ключей см. в статье [Хранилище ключей Azure](/azure/key-vault/general/basic-concepts).  
  
 На следующем изображении представлен поток процесса расширенного управления ключами с использованием хранилища ключей. (Номера шагов процесса на изображении не обязательно соответствуют номерам шагов установки, указанным после изображения.)  
  
 ![Расширенное управление ключами SQL Server с использованием Azure Key Vault](../../../relational-databases/security/encryption/media/ekm-using-azure-key-vault.png "Расширенное управление ключами SQL Server с использованием Azure Key Vault")  

> [!NOTE]  
>  Версии 1.0.0.440 и старше были заменены и больше не поддерживаются в рабочих средах. Выполните обновление до версии 1.0.1.0 или более поздней, посетив [Центр загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=45344) и используя инструкции на странице [Обслуживание соединителя SQL Server и устранение неполадок](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md) в разделе "Обновление соединителя SQL Server".
  
 Следующий шаг см. в статье [Этапы настройки расширенного управления ключами с использованием хранилища ключей Azure](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md).  
  
 Сценарии использования см. в статье [Использование Соединителя SQL Server с компонентами шифрования SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).  
  
## <a name="see-also"></a>См. также:  
 [Соединитель SQL Server, приложение](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)  
  
