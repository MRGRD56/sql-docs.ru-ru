---
title: Сведения о параметре (технология IntelliSense)
description: Узнайте, как использовать параметр IntelliSense "Сведения о параметре", который предоставляет сведения при вводе сведений о параметрах, необходимых для функции или хранимой процедуры.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Parameter Info option [IntelliSense]
- stored function parameter completion [Intellisense]
- language references [SQL Server]
- IntelliSense [SQL Server], Parameter Info option
ms.assetid: 56c2aac9-c65c-4679-b62c-d9f689876dde
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 622b3060df64c94d12ab1b7c5d602ccf1c602a4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270832"
---
# <a name="parameter-info-intellisense"></a>Сведения о параметре (технология IntelliSense)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Параметр [!INCLUDE[msCoName](../../includes/msconame-md.md)] IntelliSense **Сведения о параметре** позволяет открыть список параметров, содержащий сведения о количестве, именах и типах параметров функции или хранимой процедуры. При вводе функции или системной хранимой процедуры следующий обязательный параметр отображается полужирным шрифтом.  
  
 Список параметров отображается также и для вложенных функций. Если функция вводится в качестве параметра другой функции, в списке параметров перечисляются параметры для этой вложенной функции. Затем, когда список параметров вложенной функции заполнен, в списке продолжается отображение параметров внешней функции.  
  
#### <a name="to-view-parameter-info-for-functions-or-stored-procedures"></a>Просмотр сведений о параметрах для функций или хранимых процедур  
  
1.  После имени функции введите открывающую скобку, как это обычно делается для открытия списка параметров. Набрав имя хранимой процедуры, введите пробел, как это обычно делается для получения данных о параметрах процедур.  
  
     Технология IntelliSense отобразит полное объявление функции либо параметры хранимой процедуры во всплывающем окне сразу после позиции ввода. Первый параметр в списке будет выделен полужирным шрифтом.  
  
2.  По мере набора параметров выделение будет смещаться к следующему параметру, который необходимо ввести.  
  
3.  Нажав клавишу ESC, в любой момент можно закрыть список, в противном случае набор будет продолжен до заполнения функции.  
  
     Для функции ввод закрывающей скобки закроет список параметров.  
  
#### <a name="to-manually-start-parameter-info"></a>Открытие списка «Сведения о параметре» вручную  
  
1.  В меню **Правка** выберите пункт **IntelliSense** , а затем пункт **Сведения о параметре**.  
  
2.  Нажмите сочетание клавиш CTRL + SHIFT + ПРОБЕЛ.  
  
 Дополнительные сведения см. в статье [Настройка IntelliSense (среда SQL Server Management Studio)](./configure-intellisense-sql-server-management-studio.md).  
  
> [!NOTE]  
>  Параметр **Сведения о параметре** доступен только для редактора запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] и редактора XML-запросов.  
  
