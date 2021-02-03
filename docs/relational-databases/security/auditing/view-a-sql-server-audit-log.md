---
title: Просмотр журнала подсистемы аудита SQL Server | Документация Майкрософт
description: Узнайте, как просматривать журнал аудита в SQL Server с помощью SQL Server Management Studio. Для просмотра необходимо разрешение CONTROL SERVER.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- audits [SQL Server], viewing logs
- viewing audit logs
- logs [SQL Server], audit
ms.assetid: e8feaca0-7852-422b-895a-319b965d8d9b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 869cffd7abd1aef1dd494b82ae8969bdfd58e68a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250357"
---
# <a name="view-a-sql-server-audit-log"></a>Просмотр журнала подсистемы аудита SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как просмотреть журнал аудита SQL Server в [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Просмотр журнала подсистемы аудита SQL Server с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется разрешение **CONTROL SERVER** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-view-a-sql-server-audit-log"></a>Просмотр журнала подсистемы аудита SQL Server  
  
1.  В обозревателе объектов раскройте папку **Безопасность** .  
  
2.  Разверните папку **Аудит** .  
  
3.  Щелкните правой кнопкой мыши журнал аудита, который необходимо просмотреть, и выберите пункт **Просмотр журналов аудита**. Открывается диалоговое окно **Просмотр файла журнала —** _имя\_сервера_. Дополнительные сведения см. в статье [Log File Viewer F1 Help](../../../relational-databases/logs/log-file-viewer-f1-help.md).  
  
4.  После завершения нажмите кнопку **Закрыть**.  

 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] рекомендует просматривать журнал аудита, используя средство просмотра файлов журнала. Однако при создании автоматизированной системы мониторинга информацию в файле аудита можно просматривать напрямую с помощью функции [sys.fn_get_audit_file (Transact-SQL)](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md). При чтении файла напрямую данные возвращаются в несколько ином (необработанном) формате. Дополнительные сведения см. в статье **sys.fn_get_audit_file** .  
  
## <a name="see-also"></a>См. также:  
 [Подсистема аудита SQL Server (компонент Database Engine)](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md)   
 [Запись событий подсистемы аудита SQL Server в журнал безопасности](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md)  
  
  
