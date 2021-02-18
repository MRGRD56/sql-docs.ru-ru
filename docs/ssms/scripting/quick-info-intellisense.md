---
title: Краткие сведения (технология IntelliSense)
description: Узнайте, как использовать параметр "Краткие сведения" в IntelliSense для отображения полного объявления любого идентификатора в коде. В SQL Server Management Studio этот параметр доступен в редакторе ядра СУБД и редакторе XML-запросов.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Quick Info option [IntelliSense]
- declarations [IntelliSense]
- IntelliSense [SQL Server], Quick Info
- identifier declarations [IntelliSense]
ms.assetid: 3c8b59f4-1922-4bde-844f-5f2306514d96
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff2473620822f915deab34f67339cec6e4d0ea49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270841"
---
# <a name="quick-info-intellisense"></a>Краткие сведения (технология IntelliSense)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  При установленном параметре [!INCLUDE[msCoName](../../includes/msconame-md.md)] Краткие сведения **в технологии** IntelliSense отображается полное объявление декларации любого идентификатора в коде. При наведении указателя мыши на идентификатор его объявление отобразится в желтом всплывающем окне. В среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**Краткие сведения** доступны для компонента Database Engine и редактора XML-запросов.  
  
## <a name="transact-sql-quick-info"></a>Краткие сведения по Transact-SQL  
 Модуль **Краткие сведения** отображает в редакторе запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] два типа сведений. Во всех режимах, кроме режима отладки, модуль **Quick Info** отображает объявление выражения. В режиме отладки модуль **Краткие сведения** отображает имя выражения и его текущее значение.  
  
 В редакторе запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)]**Краткие сведения** доступны только для поддерживаемых технологией IntelliSense частей синтаксиса [!INCLUDE[tsql](../../includes/tsql-md.md)] . Например, если навести указатель мыши на идентификатор объекта, который имеет тип данных, не поддерживаемый технологией IntelliSense, то во всплывающем окне **Краткие сведения** появится сообщение о том, что этот тип данных не поддерживается.  
  
## <a name="see-also"></a>См. также:  
 [Синтаксис языка Transact-SQL с поддержкой технологии IntelliSense](./transact-sql-syntax-supported-by-intellisense.md)  
  
