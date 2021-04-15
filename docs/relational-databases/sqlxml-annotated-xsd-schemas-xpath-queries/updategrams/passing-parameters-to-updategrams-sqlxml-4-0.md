---
title: Передача параметров в диаграмм обновления (SQLXML)
description: Узнайте, как передавать параметры в диаграмм обновления в SQLXML 4,0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nullvalue attribute
- passing parameters [SQLXML]
- parameters [SQLXML]
- updategrams [SQLXML], passing parameters
- null values [SQLXML]
ms.assetid: 2354e6e7-1860-471f-8711-4e374c5a4ed2
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 99cbf1658de418fbbb022e315931ff3c6c727cd0
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491503"
---
# <a name="passing-parameters-to-updategrams-sqlxml-40"></a>Передача параметров для диаграмм обновления (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Диаграммы обновления представляют собой шаблоны; следовательно, им можно передавать параметры. Дополнительные сведения о передаче параметров в шаблоны см. в разделе [Диаграмма обновления Security соображения &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md).  
  
 Диаграммы обновления позволяют передавать значение NULL в качестве значения параметра. Чтобы передать значение параметра NULL, необходимо указать атрибут **NullValue** . Значение, присваиваемое атрибуту **NullValue** , указывается в качестве значения параметра. В диаграммах обновления это значение рассматривается как NULL.  
  
> [!NOTE]  
>  В **\<sql:header>** и **\<updg:header>** следует указать **NullValue** как неквалифицированный, тогда как в **\<updg:sync>** можно указать **NullValue** в качестве квалифицированного (например, **атрибута updg: NullValue**).  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы с помощью следующих примеров, необходимо выполнить требования, указанные в [требованиях к запуску примеров SQLXML](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
 При использовании примеров диаграмм обновления необходимо учитывать следующие моменты.  
  
-   В примерах используется сопоставление по умолчанию (т. е. в диаграмме обновления не задана схема сопоставления). Дополнительные примеры диаграмм обновления, в которых используются схемы сопоставления, см. [в разделе Указание схемы сопоставления с заметками в диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
### <a name="a-passing-parameters-to-an-updategram"></a>A. Передача параметров диаграмме обновления  
 В данном примере диаграмма обновления применяется для изменения фамилии сотрудника в таблице HumanResources.Shift. Диаграмма обновления передается два параметра: Шифтид, который используется для уникальной идентификации сдвига и имени.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:header>  
  <updg:param name="ShiftID"/>  
  <updg:param name="Name" />  
</updg:header>  
  <updg:sync >  
    <updg:before>  
       <HumanResources.Shift ShiftID="$ShiftID" />  
    </updg:before>  
    <updg:after>  
      <HumanResources.Shift Name="$Name" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Скопируйте приведенную выше диаграмму обновления в блокнот и сохраните как файл с именем UpdategramWithParameters.xml.  
  
2.  Подготовка тестового скрипта SQLXML 4,0 (Sqlxml4test.vbs) в [с помощью ADO для выполнения запросов sqlxml 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md) для выполнения диаграмма обновления путем добавления следующих строк после `cmd.Properties("Output Stream").Value = outStream` :  

    ```  
    cmd.NamedParameters = True  
    ' CreateParameter arguments: Name, Type, Direction, Size, Value  
    cmd.Parameters.Append cmd.CreateParameter("@ShiftID",  2, 1,  0, 1)  
    cmd.Parameters.Append cmd.CreateParameter("@Name",   200, 1, 50, "New Name")  
    ```  
  
### <a name="b-passing-null-as-a-parameter-value-to-an-updategram"></a>Б. Передача NULL в качестве значения параметра для диаграммы обновления  
 При выполнении диаграммы обновления тому параметру, для которого нужно задать значение NULL, присваивается значение «isnull». Диаграмма обновления преобразовывает значение параметра «isnull» в NULL и обрабатывает соответствующим образом.  
  
 Следующая диаграмма обновления устанавливает значение NULL для названия должности сотрудника.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:header nullvalue="isnull" >  
  <updg:param name="EmployeeID"/>  
  <updg:param name="ManagerID" />  
</updg:header>  
  <updg:sync >  
    <updg:before>  
       <HumanResources.Employee EmployeeID="$EmployeeID" />  
    </updg:before>  
    <updg:after>  
      <HumanResources.Employee ManagerID="$ManagerID" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Скопируйте приведенную выше диаграмму обновления в блокнот и сохраните как файл с именем UpdategramPassingNullvalues.xml.  
  
2.  Подготовка тестового скрипта SQLXML 4,0 (Sqlxml4test.vbs) в [с помощью ADO для выполнения запросов sqlxml 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md) для выполнения диаграмма обновления путем добавления следующих строк после `cmd.Properties("Output Stream").Value = outStream` :  
  
    ```  
    cmd.NamedParameters = True  
    ' CreateParameter arguments: Name, Type, Direction, Size, Value   
    cmd.Parameters.Append cmd.CreateParameter("@EmployeeID", 3, 1, 0, 1)  
    cmd.Parameters.Append cmd.CreateParameter("@ManagerID",  3, 1, 0, Null)  
    ```  
  
## <a name="see-also"></a>См. также  
 [Вопросы безопасности диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
