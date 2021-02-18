---
title: Свойства NS$&lt;имя службы&gt; (вкладка "Вход в систему")
description: Сведения о вкладке "Вход в систему" в диалоговом окне свойств служб Notification Services в SQL Server. Узнайте, как указать учетную запись, а также запустить или остановить работу службы.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 5e6816ec-d4c5-4429-8033-b97427584890
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d24e1c207027b5f15bfc14c7fbfd92e40cccd8c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349739"
---
# <a name="nsltservice-namegt-properties-log-on-tab"></a>Свойства NS$&lt;имя службы&gt; (вкладка "Вход в систему")
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Используйте вкладку **Вход** диалогового окна **Свойства служб Notification Services** для указания учетной записи, используемой службами [!INCLUDE[ssNS](../../includes/ssns-md.md)] , а также для запуска и остановки службы.  
  
## <a name="options"></a>Параметры  
 **Локальная системная учетная запись**  
 Укажите локальную системную учетную запись, для которой не требуется пароль. Однако локальная системная учетная запись может ограничить взаимодействие служб с другими серверами, это зависит от прав доступа, предоставленных этой учетной записи.  
  
 **Эта учетная запись**  
 Укажите локальную или доменную учетную запись пользователя, использующую проверку подлинности [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. [!INCLUDE[msCoName](../../includes/msconame-md.md)] рекомендует использовать доменную учетную запись пользователя с минимальными правами. Дополнительные сведения о выборе учетной записи см. в разделе «Настройка учетных записей служб Windows» электронной документации.  
  
 **Account Name** (Имя учетной записи)  
 Укажите имя локальной или доменной учетной записи.  
  
 **Пароль**  
 Введите пароль для учетной записи.  
  
 **Подтверждение пароля**  
 Введите пароль для учетной записи еще раз.  
  
 **Начало**  
 Запускает службу.  
  
 **Остановить**  
 Остановить службу.  
  
 **Приостановить**  
 Приостановить службу.  
  
 **Возобновить**  
 Возобновить приостановленную работу службы.  
  
  
