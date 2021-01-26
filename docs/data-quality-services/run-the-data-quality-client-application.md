---
description: Запуск клиентского приложения DQS
title: Запуск клиентского приложения DQS
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.browseforservers.f1
- sql13.dqs.connecttoserver.f1
ms.assetid: 0b2aa202-7ab2-4c9d-b0f1-802588053a1e
author: swinarko
ms.author: sawinark
ms.openlocfilehash: f83334dbbcc50642c463681614db5f23e10f6807
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765643"
---
# <a name="run-the-data-quality-client-application"></a>Запуск клиентского приложения DQS

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  Запустите [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]и выполните вход на [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)].  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
 Необходимо, чтобы установка сервера [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] , запускаемая с помощью файла DQSInstaller.exe, была завершена. Дополнительные сведения см. в разделе [Запуск файла DQSInstaller.exe для завершения установки сервера служб DQS](../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Для входа на сервер [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]необходимо иметь одну из трех ролей DQS (dqs_adminstrator, dqs_kb_editor или dqs_kb_operator) в базе данных DQS_MAIN.  
  
##  <a name="run-data-quality-client"></a><a name="Run"></a> Запустить Data Quality Client  
 Чтобы запустить [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] на компьютере, где он установлен, выполните следующие действия:  
  
1.  В меню **Пуск** выберите **[!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]** **Data Quality Client**.  
  
2.  В диалоговом окне **Подключение к серверу** выполните указанные ниже действия.  
  
    1.  Укажите сервер, к которому требуется подключить приложение [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] . Выберите **(LOCAL)** для подключения к [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] на локальном компьютере. Можно также щелкнуть стрелку вниз и выбрать **\<Browse network for more servers>** для подключения к другому серверу (или для подключения к локальному серверу по имени). Открывается диалоговое окно **Обзор серверов** . Вы можете выбрать сервер на вкладках **Локальные серверы** или **Сетевые серверы** .  
  
    2.  Чтобы зашифровать передачу данных между сервером [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] и клиентом [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)], щелкните **Параметры**, а затем установите флажок **Шифровать соединение** .  
  
3.  Нажмите кнопку **Соединить**.  
  
 Открывается на главном экране клиента [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] . Дополнительные сведения см. в статье [Главный экран клиента DQS](../data-quality-services/data-quality-client-home-screen.md).  
  
  
