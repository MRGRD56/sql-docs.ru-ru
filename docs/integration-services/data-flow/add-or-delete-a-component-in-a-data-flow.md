---
description: Добавление или удаление компонента в потоке данных
title: Добавление или удаление компонента в потоке данных | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- adding components
- components [Integration Services], data flow
ms.assetid: d99124f9-0994-4f40-a48e-fdca6a4383e7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ec13f2360cb36f9829f26ec63e745cfca565fb85
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339221"
---
# <a name="add-or-delete-a-component-in-a-data-flow"></a>Добавление или удаление компонента в потоке данных

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Компонентами потока данных являются источники, назначения и преобразования потока данных. Чтобы добавлять компоненты к потоку данных, необходимо включить задачу потока данных в поток управления пакета.  
  
 Следующие процедуры описывают способы добавления или удаления компонента в потоке данных пакета.  
  
### <a name="to-add-a-component-to-a-data-flow"></a>Добавление компонента к потоку данных  
  
1.  В среде [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте проект служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , содержащий необходимый пакет.  
  
2.  Чтобы открыть пакет, дважды щелкните его в обозревателе решений.  
  
3.  Откройте вкладку **Поток управления** , а затем дважды щелкните задачу потока данных, содержащую поток данных, к которому необходимо добавить компонент.  
  
4.  В инструментарии разверните **Источники потока данных**, **Преобразования потока данных** или **Назначения потока данных** и перетащите элемент потока данных в область конструктора на вкладке **Поток управления** .  
  
5.  Чтобы сохранить обновленный пакет, выберите пункт **Сохранить выбранные элементы** в меню **Файл** .  
  
### <a name="to-delete-a-component-from-a-data-flow"></a>Удаление компонента из потока данных  
  
1.  В среде [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте проект служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , содержащий необходимый пакет.  
  
2.  Чтобы открыть пакет, дважды щелкните его в обозревателе решений.  
  
3.  Перейдите на вкладку **Поток управления** и дважды щелкните задачу потока данных, содержащую поток данных, из которого необходимо удалить компонент.  
  
4.  Щелкните правой кнопкой мыши компонент потока данных и выберите пункт **Удалить**.  
  
5.  Подтвердите операцию удаления.  
  
6.  Чтобы сохранить обновленный пакет, выберите пункт **Сохранить выбранные элементы** в меню **Файл** .  
  
## <a name="see-also"></a>См. также:  
 [Соединение компонентов в потоке данных](../../integration-services/data-flow/connect-components-in-a-data-flow.md)   
 [Отладка потока данных](../../integration-services/troubleshooting/debugging-data-flow.md)   
 [Поток данных](../../integration-services/data-flow/data-flow.md)  
  
  
