---
title: Просмотр аспектов управления на основе политик в объекте
description: В этом разделе описано, как просмотреть все аспекты управления на основе политик, примененные к объекту SQL Server, в SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view facets
ms.assetid: 5f423b9f-a6c4-41a7-9d8d-8f4926ce1fb4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c30bbe5fdd1b88420ae22cde6053487869d126b2
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077032"
---
# <a name="view-policy-based-management-facets-on-a-sql-server-object"></a>Просмотр аспектов управления на основе политик в объекте SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как просмотреть все аспекты управления на основе политик, примененные к объекту SQL Server, в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Для просмотра всех аспектов в объекте используется:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется членство в роли PolicyAdministratorRole базы данных msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-view-all-of-the-facets-in-an-object"></a>Просмотр всех аспектов в объекте  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], объект экземпляра, базу данных или объект базы данных и выберите команду **Аспекты**.  
  
2.  В диалоговом окне **Просмотр аспектов —** _имя_объекта_ в списке **Аспект** выберите аспект, чтобы просмотреть его свойства. Дополнительные сведения о доступных параметрах данного диалогового окна см. в разделе [View Facets Dialog Box](../../relational-databases/policy-based-management/view-facets-dialog-box.md).  
  
3.  После завершения нажмите кнопку **ОК**.  
  
  
