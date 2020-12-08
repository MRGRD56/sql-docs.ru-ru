---
title: Удаление группы рабочей нагрузки | Документация Майкрософт
description: Сведения о том, как удалить группу рабочей нагрузки или пул ресурсов с помощью SQL Server Management Studio или Transact-SQL. Необходимо иметь разрешение CONTROL SERVER.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- workload groups [SQL Server], delete
- Resource Governor, workload group delete
ms.assetid: d5902c46-5c28-4ac1-8b56-cb4ca2b072d0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 27708e9a4b5a6d0a2863595e7ee25dccf77e5d2f
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506608"
---
# <a name="delete-a-workload-group"></a>Удаление группы рабочей нагрузки
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Группу рабочей нагрузки или пул ресурсов можно удалить в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] либо с помощью Transact-SQL.  
  
-   **Перед началом работы**  [ограничения](#LimitationsRestrictions), [разрешения](#Permissions)  
  
-   **Удаление группы рабочей нагрузки с помощью**:  [обозревателя объектов](#DelWGObjEx), [свойств Resource Governor](#DelWGRGProp), [Transact-SQL](#DelWGTSQL)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
 Группу рабочей нагрузки нельзя удалить, если она содержит активные сеансы.  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Ограничения  
 Если группа рабочей нагрузки содержит активные сеансы, то удалить или переместить ее в другой пул ресурсов путем вызова инструкции ALTER RESOURCE GOVERNOR RECONFIGURE для применения изменений не удастся. Во избежание этой проблемы можно предпринять одно из следующих действий.  
  
-   Подождать, пока все сеансы затронутых групп завершатся, и заново выполнить инструкцию ALTER RESOURCE GOVERNOR RECONFIGURE.  
  
-   Явно остановить сеанс в затронутой группе, используя команду KILL, и затем заново выполнить инструкцию ALTER RESOURCE GOVERNOR RECONFIGURE. Если сеансы не следует прерывать принудительно, то используется кнопка **Удалить** , но перед этим необходимо остановить активные сеансы, повторно создать группу с первоначальным именем и переместить ее в первоначальный пул ресурсов.  
  
-   Перезапустите сервер. После завершения процесса перезапуска удаленная группа не будет создана, а перемещенная группа будет использовать новое назначение пула ресурсов.  
  
###  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Для удаления группы рабочей нагрузки требуется разрешение CONTROL SERVER.  
  
##  <a name="delete-a-workload-group-using-object-explorer"></a><a name="DelWGObjEx"></a> Удаление группы рабочей нагрузки с помощью обозревателя объектов  
 **Удаление группы рабочей нагрузки с помощью обозревателя объектов**  
  
1.  В среде[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]откройте обозреватель объектов и рекурсивно разверните узел **Управление** вплоть до узла **Пулы ресурсов**.  
  
2.  Рекурсивно разверните узел **Пулы ресурсов** вплоть до узла **Группы рабочей нагрузки** в пуле ресурсов, содержащем группу рабочей нагрузки, которая подлежит удалению.  
  
3.  Правой кнопкой мыши щелкните группу рабочей нагрузки и выберите пункт **Удалить**.  
  
4.  В окне **Удаление объекта** эта группа рабочей нагрузки будет указана в списке **Объект для удаления** . Чтобы удалить группу рабочей нагрузки, нажмите кнопку **ОК**.  
  
##  <a name="delete-a-workload-group-using-resource-governor-properties"></a><a name="DelWGRGProp"></a> Удаление группы рабочей нагрузки с помощью свойств регулятора ресурсов  
 **Удаление группы рабочей нагрузки на странице «Свойства регулятора ресурсов»**  
  
1.  В обозревателе объектов разверните узел **Управление** и далее узлы, включая узел **Пулы ресурсов**.  
  
2.  Щелкните правой кнопкой мыши пул ресурсов, который содержит группу рабочей нагрузки, подлежащую удалению, а затем выберите команду **Свойства**. Открывается страница **Свойства регулятора ресурсов** .  
  
3.  В окне **Группы рабочей нагрузки для пула ресурсов** щелкните строку, которая относится к группе рабочей нагрузки, подлежащей удалению, затем щелкните правой кнопкой мыши стрелку вправо в начало строки и выберите команду **Удалить**.  
  
4.  Чтобы удалить группу рабочей нагрузки, нажмите кнопку **ОК**.  
  
##  <a name="delete-a-workload-group-using-transact-sql"></a><a name="DelWGTSQL"></a> Удаление группы рабочей нагрузки с помощью Transact-SQL  
 **Удаление группы рабочей нагрузки с помощью Transact-SQL**  
  
1.  Выполните инструкцию **DROP WORKLOAD GROUP** , указав имя группы рабочей нагрузки, подлежащей удалению.  
  
2.  Перед выполнением инструкции **ALTER RESOURCE GOVERNOR RECONFIGURE** убедитесь, что в удаляемой группе рабочей нагрузки нет активных запросов. Если активные запросы есть, то инструкция **ALTER RESOURCE GOVERNOR** не будет выполнена успешно. Во избежание этой ошибки можно предпринять одно из следующих действий:  
  
    -   Дождитесь отсоединения всех сеансов от группы рабочей нагрузки.  
  
    -   Явным образом остановите сеансы в группе рабочей нагрузки с помощью команды **KILL** .  
  
    -   Перезапустите сервер. Группа рабочей нагрузки не будет создана повторно.  
  
    -   Если при выполнении сценария с инструкцией **DROP WORKLOAD GROUP** решено не останавливать сеанс явно для применения изменений, то можно создать заново группу с тем же именем, которое она имела до объявления инструкции DROP, и затем переместить группу в исходный пул ресурсов.  
  
3.  Выполните инструкцию **ALTER RESOURCE GOVERNOR RECONFIGURE** .  
  
### <a name="example-transact-sql"></a>Пример (Transact-SQL)  
 В следующем примере удаляется группа рабочей нагрузки с именем `groupAdhoc`.  
  
```  
DROP WORKLOAD GROUP groupAdhoc;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [регулятор ресурсов](../../relational-databases/resource-governor/resource-governor.md)   
 [Создание пула ресурсов](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [Создание группы рабочей нагрузки](../../relational-databases/resource-governor/create-a-workload-group.md)   
 [Удаление пула ресурсов](../../relational-databases/resource-governor/delete-a-resource-pool.md)   
 [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
