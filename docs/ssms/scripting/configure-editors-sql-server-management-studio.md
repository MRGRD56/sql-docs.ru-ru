---
title: Настройка редакторов (среда SQL Server Management Studio)
description: Узнайте, как настроить работу редакторов SQL Server Management Studio, установив параметры в диалоговом окне "Параметры".
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: e7c7a8ef-f561-4258-a7b6-c445dba69f87
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 99fb2ac932b3fde03c024d1ecce06de2ef37d5b1
ms.sourcegitcommit: 9e1f1c6ee8f5a10d18a2599bfd9f3eb6081829e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2020
ms.locfileid: "89093508"
---
# <a name="configure-editors-sql-server-management-studio"></a>Настройка редакторов (среда SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Параметры работы редакторов [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] настраиваются для каждого редактора отдельно.  
  
## <a name="setting-editor-options"></a>Настройка параметров редактора  
 Большинство параметров редакторов задаются в диалоговом окне **Параметры**. Чтобы открыть это окно, выберите в меню **Сервис** пункт **Параметры…** . В диалоговом окне **Параметры** откройте узел **Текстовый редактор** в левой панели, чтобы задать параметры редактирования кода и текста. Вложенные узлы в узле «Текстовый редактор» относятся к определенным редакторам:  
  
1.  **Все языки** — параметры, заданные с помощью этого узла, относятся ко всем редакторам [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Можно переопределить эти настройки с помощью других узлов, задав другие параметры для определенного редактора.  
  
2.  **Простой текст** — параметры, заданные с помощью этого узла, относятся к редакторам MDX, DMX и текстовым редакторам.  
  
3.  **Transact-SQL** — параметры, заданные с помощью этого узла, относятся к редактору запросов ядра СУБД.  
  
4.  **XML** — параметры, заданные с помощью этого узла, относятся к редактору XML для аналитики.  
  
 Откройте узел **Выполнение запросов** или **Результаты запросов** , чтобы настроить выполнение запросов и способ отображения результатов.  
  
## <a name="editor-configuration-tasks"></a>Задачи конфигурации редакторов  
  
|Описание задачи|Раздел|  
|----------------------|-----------|  
|Описывается, каким образом можно настроить открытие редактора двойным щелчком файла с указанным расширением в проводнике Windows.|[Связывание расширения файла с редактором кода](../../relational-databases/scripting/associate-file-extensions-to-a-code-editor.md)|  
|Описывается настройка шрифтов для улучшения отображения кода и текста.|[Изменение цвета, размера и стиля шрифта](../../relational-databases/scripting/change-font-color-size-and-style.md)|  
|Описывается способ просмотра свойств.|[Использование окна «Свойства» в среде Management Studio](../../relational-databases/scripting/use-the-properties-window-in-management-studio.md)|  
|Расположение страниц справки F1 в диалоговых окнах параметров редактора.|[Справка F1 страниц параметров запросов](https://docs.microsoft.com/sql/ssms/f1-help/f1-help-for-server-connections-sql-server-management-studio)|