---
title: Настройка IntelliSense (среда SQL Server Management Studio)
description: Большинство параметров технологии IntelliSense по умолчанию включено. Узнайте, как отключить отдельные параметры IntelliSense и вместо этого выполнять соответствующие действия посредством команды меню или сочетания клавиш.
ms.custom: seo-lt-2019
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Options [SQL Server Management Studio], IntelliSense
- modifying IntelliSense options
- IntelliSense [SQL Server], modifying options
ms.assetid: 3ffc9f31-4efa-4c1a-a033-ed1dc48b065f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: afcd910170f612289be3e8f49d3f732c39f9da67
ms.sourcegitcommit: 6d53ecfdc463914f045c20eda96da39dec22acca
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/26/2020
ms.locfileid: "88901985"
---
# <a name="configure-intellisense-sql-server-management-studio"></a>Настройка IntelliSense (среда SQL Server Management Studio)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Большинство параметров технологии [!INCLUDE[msCoName](../../includes/msconame-md.md)] IntelliSense по умолчанию включено. Отдельные параметры IntelliSense можно отключить и вместо этого выполнять соответствующие действия посредством команды меню или сочетания клавиш.  
  
> [!IMPORTANT]  
>  Некоторые изменения вступают в силу только после перезапуска редактора.  Чтобы увидеть изменения, откройте новый сеанс редактора Transact-SQL.
  
### <a name="to-turn-statement-completion-options-off-by-default"></a>Чтобы отключить параметры заполнения инструкций по умолчанию  

> [!NOTE]
> Хранилище данных SQL не поддерживает IntelliSense.
>
>
  
1.  В меню **Сервис** выберите команду **Параметры**.  
  
2.  Раскройте список **Редактор текстов**, затем список **Все языки**, **Transact-SQL**или **XML**, после чего выберите пункт **Общие**.  
  
3.  Снимите флажки параметров заполнения инструкций, которые не требуются, а затем нажмите кнопку **ОК**.  
  
### <a name="to-modify-transact-sql-intellisense-options"></a>Изменение параметров технологии Transact-SQL IntelliSense  
  
1.  В меню **Сервис** выберите команду **Параметры**.  
  
2.  Раскройте список **Текстовый редактор**, затем список **Transact-SQL**, после чего выберите **IntelliSense**.  
  
3.  Снимите флажки параметров IntelliSense, которые не требуются.  
  
4.  Чтобы изменить размер скрипта, при котором отключаются функции IntelliSense, выберите размер из списка **Максимальный размер скрипта** .  
  
5.  Чтобы изменить правила учета регистра, применяемые к именам функций в списках завершения, выберите спецификацию регистра в списке **Регистр для имен встроенных функций** .  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
  
