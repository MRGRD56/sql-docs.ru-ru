---
description: Создание пользовательской задачи
title: Создание пользовательской задачи | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom tasks [Integration Services], creating
ms.assetid: 42965c09-1782-4cdb-9ce1-216af4c23e0a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0fa0570ab854d9faf319fc87279e8fcd48afa452
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337672"
---
# <a name="creating-a-custom-task"></a>Создание пользовательской задачи

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Шаги, необходимые для создания пользовательской задачи, аналогичны шагам, необходимым для создания любого другого пользовательского объекта служб [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
-   Создайте новый класс, наследующий базовый класс. Для каждой задачи используется базовый класс [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task).  
  
-   Примените к классу атрибут, определяющий тип объекта. Атрибутом для задачи является атрибут <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>.  
  
-   Переопределите реализацию методов и свойств базового класса. Для задачи это методы <xref:Microsoft.SqlServer.Dts.Runtime.Task.Validate%2A> и <xref:Microsoft.SqlServer.Dts.Runtime.Task.Execute%2A>.  
  
-   При необходимости разработайте настраиваемый пользовательский интерфейс. Для задачи необходим класс, реализующий интерфейс <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>.  
  
## <a name="getting-started-with-a-custom-task"></a>Приступая к работе с пользовательской задачей  
  
### <a name="creating-projects-and-classes"></a>Создание проектов и классов  
 Поскольку все управляемые задачи являются наследниками базового класса [Microsoft.SqlServer.Dts.Runtime.Task](/dotnet/api/microsoft.sqlserver.dts.runtime.task), первым шагом при создании пользовательской задачи является создание проекта библиотеки классов на управляемом языке программирования по выбору и класса, который наследует от этого базового класса. В этом производном классе будут переопределены методы и свойства базового класса, реализующие пользовательские функциональные возможности.  
  
 Создайте в том же решении второй проект библиотеки классов для собственного пользовательского интерфейса. Для простоты развертывания рекомендуется создать для пользовательского интерфейса отдельную сборку, поскольку это позволит обновлять и повторно развертывать диспетчер соединений и пользовательский интерфейс независимо друг от друга.  
  
 Настройте в обоих проектах подписывание сборок, которые будут создаваться во время построения, с помощью файла ключа для строгого имени.  
  
### <a name="applying-the-dtstask-attribute"></a>Применение атрибута DtsTask  
 Примените атрибут <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> к созданному классу для идентификации его в качестве задачи. Этот атрибут предоставляет информацию времени разработки, такую как имя, описание и тип задачи.  
  
 Используйте свойство <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A> для связывания задачи с пользовательским интерфейсом. Чтобы получить токен открытого ключа, необходимый для этого свойства, можно использовать команду **sn.exe -t**, отображающую токен открытого ключа из SNK-файла пары ключей, который предназначен для подписывания сборки пользовательского интерфейса.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.SSIS.Samples  
{  
  [DtsTask  
  (  
   DisplayName = "MyTask",  
   IconResource = "MyTask.MyTaskIcon.ico",  
   UITypeName = "My Custom Task," +  
   "Version=1.0.0.0," +  
   "Culture = Neutral," +  
   "PublicKeyToken = 12345abc6789de01",  
   TaskType = "PackageMaintenance",  
   TaskContact = "MyTask; company name; any other information",  
   RequiredProductLevel = DTSProductLevel.None  
   )]  
  public class MyTask : Task  
  {  
    // Your code here.  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsTask(DisplayName:="MyTask", _  
 IconResource:="MyTask.MyTaskIcon.ico", _  
 UITypeName:="My Custom Task," & _  
 "Version=1.0.0.0,Culture=Neutral," & _  
 "PublicKeyToken=12345abc6789de01", _  
 TaskType:="PackageMaintenance", _  
 TaskContact:="MyTask; company name; any other information", _  
 RequiredProductLevel:=DTSProductLevel.None)> _  
Public Class MyTask  
  Inherits Task  
  
  ' Your code here.  
  
End Class 'MyTask  
```  
  
## <a name="building-deploying-and-debugging-a-custom-task"></a>Построение, развертывание и отладка пользовательской задачи  
 Шаги по построению, развертыванию и отладке пользовательской задачи в службах [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] аналогичны шагам, необходимым для других типов пользовательских объектов. Дополнительные сведения см. в разделе [Сборка, развертывание и отладка пользовательских объектов](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).  
  
## <a name="see-also"></a>См. также:  
 [Создание пользовательской задачи](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)   
 [Создание кода пользовательской задачи](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [Разработка пользовательского интерфейса для пользовательской задачи](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  
