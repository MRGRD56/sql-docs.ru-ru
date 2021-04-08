---
description: Узнайте, как устанавливать безопасные каналы обмена данными с помощью шифрования TLS с подключениями к базе данных SQL.
title: Использование шифрования
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 8e566243-2f93-4b21-8065-3c8336649309
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 85407549aba05de1acae7108ab2fc14212c954cf
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2021
ms.locfileid: "106176936"
---
# <a name="using-encryption"></a>Использование шифрования

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

TLS-шифрование позволяет передавать по сети зашифрованные данные между экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и клиентским приложением.  
  
Шифрование TLS ― это протокол для установления безопасного канала связи и предотвращения перехвата критических или конфиденциальных сведений в сети и по другим каналам связи. Шифрование TLS позволяет клиенту и серверу выполнить взаимную проверку подлинности. После проверки подлинности шифрование TLS обеспечивает зашифрованное соединение между участниками канала для безопасной передачи сообщений.  
  
Драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] обеспечивает инфраструктуру для активации и деактивации шифрования на определенных подключениях в зависимости от заданных пользователям свойств, а также параметров сервера и клиента. Пользователь может указать место хранения сертификата и пароль, имя узла, используемого для проверки сертификата, а также когда следует шифровать канал.  
  
Шифрование TLS повышает защищенность обмена данными по сети между экземплярами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и приложениями. Включение шифрования не снижает производительности.  
  
В этой статье показано, как [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] поддерживает шифрование TLS, включая новые свойства подключений, и как можно настроить доверенное хранилище на стороне клиента.  
  
> [!NOTE]  
> Рекомендуется использовать свойство подключения **hostNameInCertificate** для проверки сертификата TLS.  

## <a name="in-this-section"></a>В этом разделе  

| Статья | Описание |
| ----- | ----------- |
| [Основные сведения о поддержке шифрования](understanding-ssl-support.md) | Описание поддержки TLS-шифрования в [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. |
| [Подключение с шифрованием](connecting-with-ssl-encryption.md) | Узнайте, как подключиться к базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с использованием новых свойств подключения, относящихся к TLS. |
| [Настройка шифрования на клиенте](configuring-the-client-for-ssl-encryption.md) | Описывает настройку доверенного хранилища по умолчанию на стороне клиента и импорт личного сертификата в доверенное хранилище клиентского компьютера. |

## <a name="see-also"></a>См. также раздел

[Защита приложений JDBC Driver](securing-jdbc-driver-applications.md)
