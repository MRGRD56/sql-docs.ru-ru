---
description: Определение баз данных и таблиц для базы данных Stretch Database с помощью Data Migration Assistant
title: Выявление баз данных и таблиц
ms.date: 10/30/2017
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, identifying databases
- Stretch Database, identifying tables
- identifying databases for Stretch Database
- identifying tables for Stretch Database
ms.assetid: 81bd93d8-eef8-4572-88d7-5c37ab5ac2bf
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0535dd0dbc410444f0dd9423da6e1bf40a4026bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079895"
---
# <a name="identify-databases-and-tables-for-stretch-database-with-data-migration-assistant"></a>Определение баз данных и таблиц для базы данных Stretch Database с помощью Data Migration Assistant
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Чтобы определить базы данных и таблицы, подходящие для Stretch Database, а также возможные проблемы блокировки, скачайте и запустите Microsoft Data Migration Assistant.
  
## <a name="get-data-migration-assistant"></a>Получение Data Migration Assistant
 Скачайте и установите помощник по миграции данных [здесь](https://www.microsoft.com/download/details.aspx?id=53595). Этот инструмент не входит в комплект на установочном носителе SQL Server.  
  
## <a name="run-data-migration-assistant"></a>Запуск Data Migration Assistant  
  
1.  Запустите Microsoft Data Migration Assistant.  

2.  Создайте новый проект с типом **Оценка** и присвойте ему имя.

3.  Выберите **SQL Server** в качестве значений параметров **Тип исходного сервера** и **Тип целевого сервера**.

4.  Щелкните **Создать**. 

5. На странице **Параметры** (шаг 1) выберите **Новые рекомендуемые возможности**. При необходимости снимите флажок **Проблемы совместимости**.

6.  На странице **Выберите источники** (шаг 2) подключитесь к серверу, выберите базу данных и затем нажмите кнопку **Добавить**.

7.  Щелкните **Запустить оценку**.

## <a name="review-the-results"></a>Просмотр результатов  
  
1.  После завершения анализа на странице **Просмотр результатов** (шаг 3) выберите параметр **Рекомендуемые возможности**, а затем откройте вкладку **Хранилище**.

2.  Просмотрите рекомендации, связанные со Stretch Database. Каждая рекомендация содержит список таблиц, для которых может подходить использование Stretch Database, а также перечень возможных проблем блокировки.

## <a name="historical-note"></a>Историческое примечание
Помощник по Stretch Database был ранее доступен в качестве компонента "Помощник по обновлению SQL Server 2016". Его было необходимо выбирать и запускать как отдельное действие.

После выпуска Data Migration Assistant, который заменяет помощника по обновлению и расширяет его возможности, функциональность помощника по Stretch Database включена в это новое средство. Чтобы получить рекомендации, связанные со Stretch Database, не требуется выбирать никаких параметров. При выполнении оценки в Data Migration Assistant результаты, связанные со Stretch Database, отображаются на вкладке **Хранилище** окна **Рекомендуемые возможности**.
  
## <a name="next-step"></a>Дальнейшие действия  
 Включите Stretch Database.  
  
-   Сведения о включении базы данных Stretch для **базы данных** см. в разделе [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).  
  
-   Сведения о включении базы данных Stretch для другой **таблицы** см. в разделе [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md). 
  
## <a name="see-also"></a>См. также:  
 [Ограничения для Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md)   
 [Включение Stretch Database для базы данных](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [Настройка базы данных Stretch для таблицы](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
