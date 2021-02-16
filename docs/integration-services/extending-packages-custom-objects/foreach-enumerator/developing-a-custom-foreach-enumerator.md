---
description: Разработка пользовательского перечислителя по каждому элементу
title: Разработка пользовательского перечислителя по каждому элементу | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom foreach enumerators [Integration Services]
- custom foreach enumerators [Integration Services], about custom foreach enumerators
- foreach enumerators [Integration Services]
ms.assetid: bffe26e0-1b9a-47ad-bae6-6b708cb4cf4f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f86fb73d17a36a0ace8accd4ca494cfc449f67af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350277"
---
# <a name="developing-a-custom-foreach-enumerator"></a>Разработка пользовательского перечислителя по каждому элементу

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Для итерации по элементам коллекции и выполнения одинаковых задач для каждого элемента службы [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] используют перечислители по каждому элементу. Службы [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] содержат ряд различных перечислителей по каждому элементу, которыми поддерживается большинство наиболее часто используемых коллекций, например, все файлы в папке, все таблицы в базе данных или все элементы в списке, хранящемся в переменной пакета. Если предлагаемый выбор перечислителей по каждому элементу и коллекций не отвечает потребностям пользователя, можно создать пользовательский перечислитель по каждому элементу.  
  
 Для создания пользовательского перечислителя по каждому элементу необходимо создать класс, наследующий от базового класса <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator>, применить атрибут <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> к новому классу и переопределить важные методы и свойства базового класса, в том числе метод <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A>.  
  
## <a name="in-this-section"></a>В этом разделе  
 В этом разделе описывается, как создавать, настраивать и кодировать пользовательский перечислитель по каждому элементу и, при необходимости, его пользовательский интерфейс.  
  
 [Создание пользовательского перечислителя по каждому элементу](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/creating-a-custom-foreach-enumerator.md)  
 Описывает, как создать классы для проекта пользовательского перечислителя по каждому элементу.  
  
 [Написание кода пользовательского перечислителя по каждому элементу](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/coding-a-custom-foreach-enumerator.md)  
 Описывает, как реализовать пользовательский перечислитель по каждому элементу путем переопределения методов и свойств базового класса.  
  
 [Разработка пользовательского интерфейса для пользовательского перечислителя по каждому элементу](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
 Описывает, как реализовать класс пользовательского интерфейса и форму, используемую для настройки пользовательского перечислителя по каждому элементу.  
  
## <a name="related-topics"></a>Связанные разделы  
  
### <a name="information-common-to-all-custom-objects"></a>Общие сведения для всех пользовательских объектов  
 Сведения, общие для всех типов пользовательских объектов, которые можно создавать в службах [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], см. в следующих разделах.  
  
 [Разработка пользовательских объектов для служб Integration Services](../../../integration-services/extending-packages-custom-objects/developing-custom-objects-for-integration-services.md)  
 Описывает основные шаги по реализации всех типов пользовательских объектов для служб [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 [Сохранение пользовательских объектов](../../../integration-services/extending-packages-custom-objects/persisting-custom-objects.md)  
 Описывает пользовательский механизм сохраняемости, при необходимости приводя пояснения.  
  
 [Сборка, развертывание и отладка пользовательских объектов](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)  
 Описывает методы построения, подписывания, развертывания и отладки пользовательских объектов.  
  
### <a name="information-about-other-custom-objects"></a>Сведения о других пользовательских объектах  
 Сведения о других типах пользовательских объектов, которые можно создавать в службах [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], см. в следующих разделах.  
  
 [Разработка пользовательской задачи](../../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)  
 Описывает программирование пользовательских задач.  
  
 [Разработка пользовательского диспетчера соединений](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md)  
 Описывает вопросы программирования пользовательских диспетчеров соединений.  
  
 [Разработка пользовательского регистратора](../../../integration-services/extending-packages-custom-objects/log-provider/developing-a-custom-log-provider.md)  
 Описывает вопросы программирования пользовательских регистраторов.  
  
 [Разработка пользовательского компонента потока данных](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md)  
 Описывает вопросы программирования пользовательских источников, преобразований и назначений потока данных.  
 
