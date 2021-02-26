---
title: Перемещение по коду и тексту
description: 'Узнайте, как перемещаться по документу различными методами: устанавливайте закладки, чтобы легко возвращаться к каким-либо местам, выполняйте добавочный поиск, используйте мышь и клавиатуру, а также используйте команду "Перейти к" для перехода к строке по ее номеру.'
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- searches [SQL Server Management Studio], incremental
- mouse [SQL Server Management Studio]
- bookmarks [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], navigating code
- Query Editor [SQL Server Management Studio], Go To Command
- incremental searches [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], bookmarks
- Query Editor [SQL Server Management Studio], mouse
- navigating code
- Go To command
ms.assetid: f63247ff-9751-4e99-8ee3-0772ad4009d0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f6242e7716e8de999e54d994a7e496a0a8225b12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354609"
---
# <a name="navigate-code-and-text"></a>Перемещение по коду и тексту

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

По тексту можно перемещаться с помощью:  
  
-   закладок;  
  
-   добавочного поиска;  
  
-   мыши и сочетаний клавиш;  
  
-   команды **Перейти к** .  
  
> [!NOTE]  
>  Полный список сочетаний клавиш см. в статье [Сочетания клавиш среды SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md).  
  
## <a name="navigating-with-bookmarks"></a>Перемещение с помощью закладок  
 Чтобы изменить документ в любом месте, а затем вернуться в исходное положение, добавьте закладку. Установка и перемещение по закладкам выполняются с помощью сочетаний клавиш. Закладки можно просмотреть в окне закладок.  
  
## <a name="incremental-search"></a>Добавочный поиск  
 Добавочный поиск позволяет переходить к разным местам в текущем документе по мере ввода символов при поиске. Включить добавочный поиск можно с помощью сочетаний клавиш.  
  
## <a name="navigating-with-the-mouse-and-keyboard"></a>Переход с помощью мыши и клавиатуры  
 Самым распространенным способом перемещения по тексту является использование мыши и клавиатуры.  
  
-   Для перемещения от символа к символу используются клавиши ВЛЕВО и ВПРАВО, а для перемещения от слова к слову — комбинация этих клавиш с клавишей CTRL. Клавиши ВВЕРХ и ВНИЗ перемещают на одну строку вверх или вниз.  
  
-   Щелкните место в тексте, чтобы установить на нем курсор.  
  
-   Для перемещения по тексту используется колесо прокрутки на мыши или полоса прокрутки.  
  
-   Для быстрого перемещения по тексту используются также клавиши HOME, END, PAGE UP и PAGE DOWN.  
  
-   Для перемещения положения курсора наверх или вниз окна используются клавиши CTRL + PAGE UP и CTRL + PAGE DOWN соответственно.  
  
-   Для прокручивания представления текста без перемещения положения курсора используются клавиши CTRL + ВВЕРХ и CTRL + ВНИЗ.  
  
## <a name="go-to-command"></a>команда «Перейти к»  
 Команда **GoTo** служит для перехода на определенный номер строки. Чтобы сделать видимой нумерацию строк, в диалоговом окне **Параметры** выберите **Текстовый редактор**, далее **Все языки**, **Общие** и **Номера строк**.  
  
 **Чтобы перейти на определенный номер строки:**  
  
1.  В меню **Правка** выберите пункт **Перейти к** .  
  
2.  Введите нужный номер строки.