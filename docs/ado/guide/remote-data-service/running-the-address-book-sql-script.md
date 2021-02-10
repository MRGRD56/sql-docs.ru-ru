---
description: Запуск сценария SQL адресной книги
title: Выполнение скрипта SQL адресной книги | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: 409b3f8b-0ced-4867-acbe-b245dcdf6702
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e36c60e0c3e56ab0c159c362e98c2280e6c5426
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036394"
---
# <a name="running-the-address-book-sql-script"></a>Запуск сценария SQL адресной книги
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
 Для запуска включаемого скрипта SQL (Самплимп. SQL) необходимо использовать служебную программу командной строки ISQL/Query Analyzer или диспетчер SQL Server Enterprise.  
  
-   Создает новую базу данных Аддрбукдб на устройстве по умолчанию.  
  
-   Подключается к базе данных Аддрбукдб.  
  
-   Создает таблицу Employee.  
  
-   Заполняет таблицу образцом данных.  
  
-   Выполняет простую инструкцию SELECT для проверки заполнения таблицы базы данных.  
  
-   Настраивает учетную запись пользователя с именем "адкдемо" с паролем "адкдемо".  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-65"></a>Выполнение скрипта Самплимп. SQL в Microsoft SQL Server 6,5  
  
1.  Нажмите кнопку **Пуск**, укажите пункт **программы**, а затем выберите пункт **Microsoft SQL Server 6,5**. Щелкните **SQL Enterprise Manager**.  
  
2.  В меню **Сервис** выберите пункт **SQL Query Tool**.  
  
3.  Щелкните **загрузить скрипт SQL** и перейдите к к:\платформ сдк\самплес\датаакцесс\рдс\аддрессбук.  
  
4.  Выберите файл Самплимп. SQL. Нажмите кнопку **Открыть**.  
  
5.  Нажмите кнопку **выполнить запрос** (зеленая стрелка на панели инструментов).  
  
6.  После выполнения запроса закройте окна **Query** и **Enterprise Manager** .  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-70"></a>Выполнение скрипта Самплимп. SQL в Microsoft SQL Server 7,0  
  
1.  Нажмите кнопку **Пуск**, укажите пункт **программы**, а затем выберите пункт **Microsoft SQL Server 7,0**. Щелкните **Enterprise Manager**.  
  
2.  Убедитесь, что SQL Server, которые вы хотите использовать, выбраны из списка зарегистрированных серверов в Enterprise Manager.  
  
3.  В меню **Сервис** выберите **SQL Server анализатор запросов**.  
  
4.  Нажмите кнопку **загрузить скрипт SQL** (открыть папку на панели инструментов) и перейдите к папке к:\платформ сдк\самплес\датаакцесс\рдс\аддрессбук.  
  
5.  Выберите файл Самплимп. SQL. Нажмите кнопку **Открыть**.  
  
6.  Нажмите кнопку **выполнить запрос** (зеленая стрелка на панели инструментов) или **клавишу F5**.  
  
7.  После выполнения запроса закройте окно **запрос**, **анализатор запросов** и окна **Enterprise Manager** .  
  
## <a name="see-also"></a>См. также:  
 [Выполнение примера приложения адресной книги](./running-the-address-book-sample-application.md)