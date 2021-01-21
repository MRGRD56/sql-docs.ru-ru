---
description: Общие сведения о консоли конструктора CDC
title: Общие сведения о консоли конструктора CDC | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 45298179-4ac1-4723-8b3c-56f5926be40a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ac65acde470d75c616e77d21f6e62ab8d1f5e4e4
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241862"
---
# <a name="the-cdc-designer-console-introduction"></a>Общие сведения о консоли конструктора CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  В этом разделе описаны процедуры установки конструктора отслеживания измененных данных для Oracle от Attunity.  
  
## <a name="installation"></a>Установка  
 В этом разделе описаны процедуры установки конструктора отслеживания измененных данных для Oracle от Attunity.  
  
 Конструктор и служба системы отслеживания измененных данных Microsoft® для Oracle от Attunity для Microsoft SQL Server® 2016 включены в пакет дополнительных компонентов SQL Server 2016. Скачать компоненты пакета дополнительных компонентов можно со страницы [Microsoft® SQL Server® 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833)(Пакет дополнительных компонентов Microsoft® SQL Server® 2016).  
  
## <a name="supported-windows-environments"></a>Поддерживаемые среды Windows  
 Консоль конструктора CDC может работать в следующих средах Windows:  
  
-   Windows 8 и 8.1  
-   Windows 10  
-   Windows Server 2012 и 2012 R2
-   Windows Server 2016

## <a name="database-prerequisites"></a>Предварительные требования базы данных  
 Конструктор отслеживания измененных данных для Oracle от Attunity работает с базой данных Oracle. Конструктор отслеживания измененных данных для Oracle от Attunity поддерживает следующие версии:  
  
### <a name="source-oracle-database"></a>Исходная база данных Oracle
  
- База данных Oracle 10g, выпуск 2
- База данных Oracle 11g, выпуск 1 и 2
- Oracle Database 12c, классическая установка (многоклиентская установка не поддерживается).  
- Oracle Database 18c, классическая установка (многоклиентская установка не поддерживается).
- Oracle Database 19c, классическая установка (многоклиентская установка не поддерживается).

### <a name="target-sql-server-database"></a>Целевая база данных SQL Server
  
-   [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с поддержкой CDC SQL Server  
  
## <a name="software-prerequisites"></a>Обязательное программное обеспечение  
 Должна использоваться 32-разрядная или 64-разрядная версия программного обеспечения клиента Oracle, соответствующая устанавливаемой версии консоли конструктора CDC Oracle.  
  
 Консоль конструктора CDC Oracle выполняет обмен данными с исходной базой данных Oracle через поставщик ODBC Oracle.  
  
## <a name="running-the-installation-program"></a>Запуск программы установки  
 В этом разделе описана установка консоли конструктора CDC.  
  
 **Установка консоли конструктора CDC**  
  
 Дважды щелкните пакет установки консоли конструктора CDC и следуйте указаниям мастера установки.  
  
## <a name="uninstalling-the-cdc-designer-console"></a>Удаление консоли конструктора CDC  
 Консоль конструктора CDC удаляется через пункт панели управления "Программы и компоненты".  
  
  
