---
description: Добавление или изменение выражение свойства
title: Добавление или изменение выражения свойства | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- expressions [Integration Services], creating
- expressions [Integration Services], property expressions
ms.assetid: cb5da499-065f-4fa6-9f6d-5bc5f385241e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3ef6580fb5a6983412343137e5cdf9e0d4ecbcf0
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123320"
---
# <a name="add-or-change-a-property-expression"></a>Добавление или изменение выражение свойства

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Пользователь может создать выражения свойств для пакетов, задач, контейнеров «цикл по каждому элементу», контейнеров «цикл по элементам», контейнеров последовательности, обработчиков событий, диспетчеров соединений на уровне пакетов и проектов и регистраторов.  
  
 Для создания или изменения выражений свойств вы можете использовать **Редактор выражений свойств** или **Построитель выражений**. К **редактору выражений свойств** можно получить доступ из пользовательских редакторов задач и контейнеров или в окне **Свойства** . К **построителю выражений** можно обращаться из **редактора выражений свойств**. Хотя выражения можно писать и в **редакторе выражений свойств** , и в **построителе выражений**, **построитель выражений** предоставляет набор графических средств, облегчающий создание сложных выражений.  
  
 Чтобы лучше изучить синтаксис, операторы и функции, которые предоставляют службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], см. статьи [Операторы (выражение служб SSIS)](../../integration-services/expressions/operators-ssis-expression.md) и [Функции (выражение служб SSIS)](../../integration-services/expressions/functions-ssis-expression.md). Разделы по каждому оператору и каждой функции включают в себя примеры использования оператора или функции в выражении. Примеры более сложных выражений см. в разделе [Использование выражений свойств в пакетах](../../integration-services/expressions/use-property-expressions-in-packages.md).  
  
### <a name="to-create-or-change-a-property-expression"></a>Создание или изменение выражения свойства  
  
1.  В среде [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте проект, содержащий необходимый пакет служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  В обозревателе решений дважды щелкните пакет, чтобы открыть его, затем выполните одно из следующих действий.  
  
    -   Если элемент является задачей или контейнером, выделите его двойным щелчком и выберите **Выражения** в редакторе.  
  
    -   Правой кнопкой мыши щелкните элемент и выберите команду **Свойства**.  
  
3.  Щелкните окно **Выражения** и нажмите кнопку с многоточием (...).  
  
4.  В **редакторе выражений свойств** выберите свойство в списке **Свойства** , затем выполните одно из следующих действий.  
  
    -   Введите или измените выражение свойства непосредственно в столбце **Выражение** и нажмите кнопку **ОК**.  
  
         -или-  
  
    -   Нажмите кнопку с многоточием (...) в строке выражения свойства, чтобы открыть **построитель выражений**.  
  
5.  В **построителе выражений** выполните одну из следующих задач (необязательно):  
  
    -   Для доступа к системным и пользовательским переменным разверните узел **Переменные**.  
  
    -   Чтобы получить доступ к функциям, приведениям и операторам языка выражений служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] , разверните узел **Математические функции**, **Строковые функции**, **Функции даты-времени**, **Функции работы со значением NULL**, **Приведения типов** и **Операторы**.  
  
    -   Чтобы создать или изменить выражение в **построителе выражений**, перетащите переменные, столбцы, функции, операторы и приведения в поле **Выражение** или введите выражение в это поле.  
  
    -   Чтобы просмотреть результат вычисления выражения, щелкните **Вычислить значение выражения** в **построителе выражений**.  
  
         Если выражение является недопустимым, появится предупреждение с описанием синтаксических ошибок данного выражения.  
  
        > [!NOTE]  
        >  Вычисление выражения не присваивает результат вычисления свойству.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>См. также  
 [Выражения служб Integration Services (SSIS)](../../integration-services/expressions/integration-services-ssis-expressions.md)   
 [Использование выражений свойств в пакетах](../../integration-services/expressions/use-property-expressions-in-packages.md)   
 [Пакеты служб Integration Services (SSIS)](../../integration-services/integration-services-ssis-packages.md)   
 [Контейнеры служб Integration Services](../../integration-services/control-flow/integration-services-containers.md)   
 [Задачи служб Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Обработчики событий в службах Integration Services (SSIS)](../../integration-services/integration-services-ssis-event-handlers.md)   
 [Соединения в службах Integration Services (SSIS)](../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Ведение журналов в службах Integration Services (SSIS)](../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
