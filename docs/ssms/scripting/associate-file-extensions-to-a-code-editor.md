---
title: Связывание расширения файла с редактором кода
description: Сведения о том, как связать расширение файла с конкретным редактором кода, чтобы при двойном щелчке файла с таким расширением он открывался в соответствующем редакторе.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- file extensions [SQL Server]
- associating file extensions [SQL Server]
- Query Editor [SQL Server Management Studio], associating file extensions
ms.assetid: 193630f4-93de-4950-8f36-68702531f925
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4196519b2e743449864cd171d6da3c7863475157
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354803"
---
# <a name="associate-file-extensions-to-a-code-editor"></a>Связывание расширения файла с редактором кода

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Связь расширений файлов с конкретным редактором кода позволяет открывать файл соответствующим редактором кода среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] двойным щелчком файла в проводнике Windows. Расширения, обычно используемые средой [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], такие как SQL и MDX, задаются во время установки. Новые расширения файлов должны быть связаны со средой [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] в файловой системе. Эту возможность можно использовать для открытия файлов, созданных в других редакторах, или для открытия переименованных файлов, таких как резервные копии SQL-файлов, переименованных в файлы с расширением BAK.  
  
 Этот процесс включает два шага. Вначале нужно связать расширение со средой [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], а затем — с конкретным редактором кода.  
  
### <a name="to-associate-a-new-file-extension-with-sql-server-management-studio"></a>Связывание нового расширения файла со средой SQL Server Management Studio  
  
1.  В меню **Пуск** последовательно укажите **Все программы**, **Стандартные**, **Проводник**.  
  
2.  В проводнике Windows в меню **Сервис** выберите **Свойства папки**.  
  
3.  В диалоговом окне **Свойства папки** на вкладке **Типы файлов** нажмите кнопку **Создать**.  
  
4.  В диалоговом окне **Создание нового расширения** в поле **Расширение** введите новое расширение файла, которое нужно задать, и нажмите кнопку **ОК**. Не ставьте перед расширением точку.  
  
5.  В списке **Зарегистрированные типы файлов** выберите новое расширение и нажмите **Изменить**.  
  
6.  В диалоговом окне **Открыть с помощью** выберите пункт **SSMS — среда SQL Server Management Studio** и нажмите кнопку **ОК**.  
  
7.  Нажмите **Закрыть** , чтобы закрыть диалоговое окно **Свойства папки** , и закройте проводник Windows.  
  
### <a name="to-associate-a-new-file-extension-with-a-code-editor-in-sql-server-management-studio"></a>Связывание нового расширения файла с редактором кода в среде SQL Server Management Studio  
  
1.  В среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]в меню **Сервис** выберите **Параметры**.  
  
2.  В диалоговом окне **Параметры** щелкните **Текстовый редактор**, а затем — **Расширение файла**.  
  
3.  В поле **Расширение** введите новое расширение файла.  
  
4.  В поле **Редактор** выберите редактор кода, который хотите использовать для открытия данного типа файлов, нажмите кнопку **Добавить**, а затем кнопку **ОК**.  
  
## <a name="see-also"></a>См. также:  
 [Программа SSMS](../ssms-utility.md)  
  
