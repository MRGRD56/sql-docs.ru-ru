---
title: Настройка регулятора ресурсов с помощью шаблона | Документация Майкрософт
description: Сведения о том, как настроить Resource Governor с помощью шаблона, предоставленного в SQL Server Management Studio. Необходимо иметь разрешение CONTROL SERVER.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, templates
ms.assetid: f342dec2-d1d6-483e-b44e-98eb7d168b5e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9da1652b2a4814950bbc152e4ea74eb9e4c38a17
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504871"
---
# <a name="configure-resource-governor-using-a-template"></a>Настройка регулятора ресурсов с помощью шаблона
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Можно настроить регулятор ресурсов с помощью шаблона, имеющегося в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
-   **Перед началом работы**  [Разрешения](#Permissions)  
  
-   **Создание группы рабочей нагрузки с использованием:** [шаблона](#ConfRGTemplate)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
 С помощью дополнительных шагов можно открыть и изменить шаблон, который создает пул ресурсов и группу рабочей нагрузки для этого пула. Кроме того, данный шаблон позволяет создавать определяемую пользователем функцию-классификатор, направляющую новые соединения либо в группу по умолчанию, либо в пользовательскую группу рабочей нагрузки.  
  
###  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Для выполнения инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] регулятора ресурсов, содержащихся в шаблоне, требуется разрешение CONTROL SERVER.  
  
##  <a name="configure-resource-governor-using-a-template"></a><a name="ConfRGTemplate"></a> Настройка регулятора ресурсов с помощью шаблона  
 **Настройка регулятора ресурсов с помощью шаблона в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**  
  
1.  В среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]в меню **Вид** выберите пункт **Обозреватель шаблонов**.  
  
2.  В **Обозревателе шаблонов** разверните **Регулятор ресурсов**, затем дважды щелкните значок **Настройка регулятора ресурсов**.  
  
3.  В разделе **Подключение к компоненту Database Engine** введите необходимые сведения и нажмите кнопку **ОК**. В составе редактора запросов поставляется шаблон Configure Resource Governor.sql. С помощью этого шаблона можно создать и настроить пул ресурсов, группу рабочей нагрузки и функцию-классификатор.  
  
4.  Чтобы изменить значения в шаблоне, нажмите сочетание клавиш CTRL+SHIFT+M. В окне **Задание значений для параметров шаблона** введите необходимые значения.  
  
5.  Чтобы сохранить изменения, внесенные в шаблон, нажмите кнопку **ОК**.  
  
6.  Чтобы запустить запрос, нажмите кнопку **Выполнить**.  

## <a name="see-also"></a>См. также:  
 [регулятор ресурсов](../../relational-databases/resource-governor/resource-governor.md)   
 [Активация регулятора ресурсов](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Пул ресурсов регулятора ресурсов](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Группа рабочей нагрузки регулятора ресурсов](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Функция-классификатор регулятора ресурсов](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [Просмотр свойств регулятора ресурсов](../../relational-databases/resource-governor/view-resource-governor-properties.md)   
 [CREATE RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
