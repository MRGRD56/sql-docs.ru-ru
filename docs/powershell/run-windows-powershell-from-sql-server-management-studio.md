---
title: Запуск Windows PowerShell из среды SQL Server Management Studio | Документация Майкрософт
description: Узнайте, как запустить сеанс Windows PowerShell из обозревателя объектов в SQL Server Management Studio с предустановленным путем к выбранному вами расположению объектов.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 1f841825-da1f-4062-9a81-3cdbab03845b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3e244789ced3150c02a07749884dc5f3f0a77b9c
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714212"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Запуск Windows PowerShell из среды SQL Server Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Можно запускать сеансы Windows PowerShell из **обозревателя объектов** в среде [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] запускает Windows PowerShell, загружает модуль **SqlServer** и задает контекст пути для связанного узла в дереве **обозревателя объектов**.  
  

> [!NOTE]
> Существует два модуля SQL Server PowerShell — **SqlServer** и **SQLPS**. Модуль **SQLPS** входит в состав установки SQL Server (для обеспечения обратной совместимости), но больше не обновляется. Самым актуальным модулем PowerShell является модуль **SqlServer**. Модуль **SqlServer** содержит обновленные версии командлетов в **SQLPS**, а также новые командлеты для поддержки последних функций SQL.  
> Предыдущие версии модуля **SqlServer** *входили* в состав среды SQL Server Management Studio (SSMS), но только с SSMS версий 16.x. Для работы PowerShell с SSMS 17.0 и более поздних версий необходимо установить модуль **SqlServer** из коллекции PowerShell.
> Сведения об установке модуля **SqlServer** см. в статье [Установка компонентов SQL Server PowerShell](download-sql-server-ps-module.md).



После указания того, что PowerShell необходимо вызвать на выполнение для некоторого объекта в **обозревателе объектов**, [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] начинает сеанс Windows PowerShell, в котором были загружены и зарегистрированы оснастки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell. Путем для сеанса становится расположение объекта, выбранного правой кнопкой мыши в обозревателе объектов. Например, если щелкнуть правой кнопкой мыши объект базы данных [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] в обозревателе объектов, а затем выбрать команду **Запустить PowerShell**, то путь Windows PowerShell будет иметь следующий вид:  
  
```  
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```  
  
## <a name="run-powershell"></a>Запуск PowerShell  
 **Запуск PowerShell из среды SQL Server Management Studio**  
  
1.  Откройте **обозреватель объектов**.  
  
2.  Перейдите к узлу для объекта, с которым выполняется работа.  
  
3.  Щелкните объект правой кнопкой мыши и выберите команду **Запустить PowerShell**.  
  
## <a name="permissions"></a>Разрешения  
 При открытии из среды [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] программа PowerShell не запускается с правами администратора, что может стать причиной того, что некоторые действия не будут выполнены, например вызовы инструментария WMI.  
  
## <a name="see-also"></a>См. также:  
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  
