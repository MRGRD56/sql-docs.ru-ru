---
description: Создание политики управления на основе политик
title: Создание политики управления на основе политик | Документация Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policies
ms.assetid: b28bf963-89f9-4941-b6c1-6004fec347f1
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ccc76880687d509b59e3171f1b6dfaa9029ad6f5
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048803"
---
# <a name="create-a-policy-based-management-policy"></a>Создание политики управления на основе политик
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается создание политики управления на основе политик в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Создание политики с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется членство в роли PolicyAdministratorRole базы данных msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-create-a-policy"></a>Создание политики  
  
1.  В **обозревателе объектов** щелкните знак "плюс", чтобы развернуть сервер, на котором необходимо создать новую политику управления на основе политик.  
  
2.  Щелкните знак «плюс», чтобы развернуть папку **Управление** .  
  
3.  Щелкните знак «плюс», чтобы развернуть папку **Управление политиками**.  
  
4.  Щелкните правой кнопкой мыши папку **Политики** и выберите команду **Создать политику**.  
  
5.  В диалоговом окне **Создание новой политики** в поле **Имя** введите имя новой политики.  
  
6.  Установите флажок **Включено** , если требуется, чтобы политика была включена сразу после ее создания. Если режим вычисления имеет значение **По запросу**, флажок **Включено** недоступен.  
  
7.  В списке **Проверить условие** выберите одно из существующих условий или укажите пункт **Создать условие**. Для изменения условия выберите его и нажмите кнопку с многоточием ( **...** ). Дополнительные сведения см. в статье [Создание нового условия управления на основе политик](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md) или [Просмотр или изменение свойств условия управления на основе политик](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md).  
  
8.  Выберите один или несколько типов целевого объекта для данной политики в окне **Применить к** . Некоторые условия и аспекты можно применить только к определенным типам целей. Доступные наборы целей появляются в соответствующем окне. Чтобы выбрать условие фильтрации для некоторых типов целей, разверните **Каждый** . Если в разделе не появляются целевые объекты, убедитесь, что условие ограничено уровнем сервера.  
  
9. Выберите тип поведения данной политики в окне **Режим вычисления** . Разные условия могут иметь разные допустимые режимы вычисления. Дополнительные сведения о допустимых режимах вычисления см. в статье [Администрирование серверов с помощью управления на основе политик](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md).  
  
10. Если политика вычисляется по расписанию, установите режим вычисления в состояние **По запросу** и нажмите **Выбрать** для выбора расписания либо нажмите кнопку **Создать** для создания нового расписания.  
  
11. Чтобы ограничить политику поднабором типов целевого объекта, выберите условия в окне **Ограничение сервера** или создайте новое условие.  
  
     Дополнительные сведения о параметрах, доступных в диалоговом окне **Создание новой политики** , см. в разделах [Диалоговое окно «Создание новой политики» или «Открытие политики», страница «Общие»](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-general-page.md) и [Диалоговое окно «Создание новой политики» или «Открытие политики», страница «Описание»](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-description-page.md).  
  
12. В завершение нажмите кнопку **ОК**.  

