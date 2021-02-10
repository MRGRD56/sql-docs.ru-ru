---
description: Создание тестовых случаев (SybaseToSQL)
title: Создание тестовых случаев (SybaseToSQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester component,Test Case Wizard
ms.assetid: b52dfd93-95af-4299-bc8f-83f2a7a6a518
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 460ea84af51b872841eb82cd405cc29ebfe59e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056689"
---
# <a name="creating-test-cases-sybasetosql"></a>Создание тестовых случаев (SybaseToSQL)
Используйте мастер тестовых случаев для создания теста. Этот мастер позволяет создавать тестовые случаи, выбрав проверенные и проверенные объекты и указав параметры тестирования.  
  
## <a name="starting-the-test-case-wizard"></a>Запуск мастера тестовых случаев  
Чтобы запустить мастер тестовых случаев, щелкните **создать тестовый случай...** в меню **Тестер** .  
  
При запуске мастер выполняет поиск базы данных ssmatester2005db или ssmatester2008db (в зависимости от типа проекта) на исходном сервере Sybase. Это схема расширения тестеров, используемая для хранения вспомогательных объектов. Если мастеру тестовых случаев не удается найти ssmatester2005db или ssmatester2008db, откроется диалоговое окно, в котором предлагается создать базу данных расширения тестеров. (Эта ситуация обычно возникает во время первого запуска тест-инженера SSMA.)  
  
Если появится диалоговое окно, нажмите кнопку **Да** , чтобы создать базу данных тестеров Sybase на исходном сервере. После этого появится новое диалоговое окно, в котором необходимо добавить одно или несколько устройств, на которых нужно разместить новую базу данных тестеров. Нажмите кнопку **Добавить** , чтобы добавить устройство. В диалоговом окне **место выделения для базы данных тестера** выберите устройство и укажите размер, используемый базой данных тестеров. Кроме того, можно задать отдельное устройство для журналов базы данных. Обратите внимание, что для создания баз данных необходимо иметь права Sybase.  
  
## <a name="overview-of-creating-test-cases-using-the-wizard"></a>Общие сведения о создании тестовых случаев с помощью мастера  
Процесс создания тестового случая состоит из пяти шагов.  
  
1.  [Инициализация тестовых случаев &#40;&#41;SybaseToSQL ](../../ssma/sybase/initializing-test-cases-sybasetosql.md).  
  
2.  [Выбор и настройка объектов для тестирования &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-objects-to-test-sybasetosql.md).  
  
3.  [Выбор и настройка затронутых объектов &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md).  
  
4.  [Настройка порядка вызовов &#40;&#41;SybaseToSQL ](../../ssma/sybase/customizing-calls-order-sybasetosql.md).  
  
5.  [Завершение подготовки тестового случая &#40;&#41;SybaseToSQL ](../../ssma/sybase/finishing-test-case-preparation-sybasetosql.md).  
  
## <a name="see-also"></a>См. также:  
[Тестирование перенесенных объектов базы данных &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
