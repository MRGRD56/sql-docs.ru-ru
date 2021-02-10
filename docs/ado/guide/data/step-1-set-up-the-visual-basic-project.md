---
description: Шаг 1. Настройка проекта Visual Basic
title: Шаг 1. Настройка проекта Visual Basic | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 77d3bfa5-fc9f-4a72-93b4-790c7d227988
author: rothja
ms.author: jroth
ms.openlocfilehash: b80f44160e3542147158f0bece7c87adb3ba7e4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032446"
---
# <a name="step-1-set-up-the-visual-basic-project"></a>Шаг 1. Настройка проекта Visual Basic
В этом сценарии предполагается, что у вас есть Microsoft Visual Basic 6,0, ADO 2,5 или более поздней версии и поставщик Microsoft OLE DB для публикации в Интернете в системе. Сначала необходимо создать новый проект, а затем добавить некоторые элементы управления в форму по умолчанию в проекте.  
  
### <a name="to-create-an-ado-project"></a>Чтобы создать проект ADO, выполните следующие действия.  
  
1.  В Microsoft Visual Basic создайте новый проект EXE.  
  
2.  В меню Проект выберите пункт ссылки.  
  
3.  Выберите "Microsoft объекты данных ActiveX 2,5 Library" и нажмите кнопку "ОК".  
  
### <a name="to-insert-controls-on-the-main-form"></a>Чтобы вставить элементы управления в главную форму, сделайте следующее:  
  
1.  Добавьте элемент управления ListBox в форму Form1. Присвойте свойству Name значение **лстмаин**.  
  
2.  Добавьте еще один элемент управления ListBox в форму Form1. Присвойте свойству Name значение **лстдетаилс**.  
  
3.  Добавьте элемент управления TextBox в форму Form1. Присвойте свойству Name значение **ткстдетаилс**.  
  
## <a name="see-also"></a>См. также:  
 [Сценарий публикации в Интернете](../../../ado/guide/data/internet-publishing-scenario.md)   
 [Шаг 2. Инициализация главного списка](../../../ado/guide/data/step-2-initialize-the-main-list-box.md)
