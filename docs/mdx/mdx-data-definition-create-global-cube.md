---
description: Определение данных многомерных выражений — CREATE GLOBAL CUBE
title: CREATE GLOBAL CUBE, инструкция (многомерные выражения) | Документация Майкрософт
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: a041e8dac7459e1f0322bd8492b5e5737ae80691
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92195640"
---
# <a name="mdx-data-definition---create-global-cube"></a>Определение данных многомерных выражений — CREATE GLOBAL CUBE


  Создает и заполняет значениями локально материализованный куб на основе вложенного куба из куба на сервере. Чтобы соединиться с локально материализованным кубом, соединяться с сервером необязательно. Дополнительные сведения о локальных кубах см. в разделе [Локальные кубы &#40;Analysis Services многомерных данных&#41;](/analysis-services/multidimensional-models/olap-physical/local-cubes-analysis-services-multidimensional-data).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
CREATE GLOBAL CUBE local_cube_name STORAGE 'Cube_Location'   
FROM source_cube_name (<param list>)  
  
<param list>::= <param> ,<param list> | <param>  
  
<param>::= <dims list> | <measures list>  
  
<measures list>::= <measure>[, <measures list>]   
  
<dims list>::= <dim def> [, <dims list>]  
  
<measure>::= MEASURE source_cube_name.measure_name [<visibility qualifier>] [AS measure_name]   
  
<dim def>::= <source dim def> | <derived dim def>  
  
<source dim def>::= DIMENSION source_cube_name.dimension_name [<dim flags>] [<visibility qualifier>] [AS dimension_name>] [FROM <dim from clause> ] [<dim content def>]  
  
<dim flags>::= NOT_RELATED_TO_FACTS   
  
<dim from clause>::= < dim DM from clause> | <reg dim from clause>   
  
<dim DM from clause>::= dm_model_name> COLUMN column_name   
  
<dim reg from clause>::= dimension_name  
  
<dim content def>::= ( <level list> [,<grouping list>] [,<member slice list>] [,<default member>] )  
  
<level list>::= <level def> [, <level list>]  
  
<level def>::= LEVEL level_name [<level type> ] [AS level_name] [<level content def>]  
  
<level content def>::= ( <property list> ) | NO_PROPERTIES  
  
<level type>::= GROUPING  
  
<property list>::= <property def> [, <property list>]  
  
<property def>::= PROPERTY property_name   
  
<grouping list>::= <grouping entity> [,<grouping list>]  
  
<grouping entity>::= GROUP group_level_name.group_name (<mixed list>)  
  
<grp mixed list>::= <grp mixed element> [,<grp mixed list>]  
  
<grp mixed element>::= <grouping entity> | <member def>  
  
<member slice list>::= <member list>  
  
<member list>::= <member def> [, <member list>]  
  
<member def>::= MEMBER member_name  
  
<default member>::= DEFAULT_MEMBER AS MDX_expression  
  
<visibility qualifier>::= HIDDEN   
```  
  
## <a name="syntax-elements"></a>Элементы синтаксиса  
 local_cube_name  
 Имя локального куба.  
  
 'Cube_Location'  
 Путь и имя локально материализованного куба.  
  
 source_cube_name  
 Имя куба, на котором основан локальный куб.  
  
 source_cube_name.measure_name  
 Полное имя исходной меры, включаемой в локальный куб. Вычисляемые элементы измерения «Меры» недопустимы.  
  
 measure_name  
 Имя меры в локальном кубе.  
  
 source_cube_name.dimension_name  
 Полное имя исходного измерения, включаемого в локальный куб.  
  
 dimension_name  
 Имя измерения в локальном кубе.  
  
 FROM \<dim from clause>  
 Спецификация, допустимая только для определения производного измерения.  
  
 NOT_RELATED_TO_FACTS  
 Спецификация, допустимая только для определения производного измерения.  
  
 \<level type>  
 Спецификация, допустимая только для определения производного измерения.  
  
## <a name="remarks"></a>Remarks  
 Локальный куб определяется в терминах мер и определений, определяющих его. Существует два типа измерений.  
  
-   Измерения источника — это измерения, которые были частью одного из исходных кубов.  
  
-   Производные измерения — это измерения, которые обеспечивают новые возможности анализа. Производным измерением может являться обычное измерение, определенное на основе исходного измерения, которое является горизонтальным или вертикальным срезом либо содержит пользовательское группирование элементов измерения. Производным также может быть измерение интеллектуального анализа данных, основанное на модели интеллектуального анализа данных.  
  
> [!NOTE]  
>  Ключевое слово «измерение» может относиться либо к измерениям, либо к иерархиям.  
  
 В локальном кубе можно выполнять следующие задачи:  
  
-   Исключать измерения, которые существуют в исходном кубе.  
  
-   Добавлять или исключать иерархии из измерения.  
  
-   Исключать группы мер или определенные меры.  
  
 Инструкция CREATE GLOBAL CUBE удовлетворяет следующим правилам.  
  
-   Инструкция CREATE GLOBAL CUBE автоматически копирует все команды, такие как вычисляемые меры и действия, в локальный куб. Локальный куб не может выполнить команду, если она содержит многомерное выражение, явно ссылающееся на родительский куб. Чтобы избежать этой проблемы, используйте ключевое слово **CURRENTCUBE** при определении многомерных выражений для команд. Ключевое слово **CURRENTCUBE** использует текущий контекст куба при ссылке на куб в многомерном выражении.  
  
-   Глобальный куб, созданный из существующего глобального куба в локальном файле, нельзя сохранить в тот же файл. Создайте глобальный куб SalesLocal1 и сохраните в файле C:\SalesLocal.cub. Затем выполняется подключение к файлу C:\SalesLocal.cub и создается второй глобальный куб SalesLocal2. При попытке сохранения глобального куба SalesLocal2 в файл C:\SalesLocal.cub возникнет ошибка. Глобальный куб SalesLocal2 можно сохранить только в отдельный файл локального куба.  
  
-   Глобальные кубы не поддерживают меры числа различных объектов. Поскольку кубы, которые содержат меры числа различных объектов, не являются аддитивными, инструкция CREATE GLOBAL CUBE не поддерживает создание и использование этих мер.  
  
-   При добавлении в локальный куб меры необходимо также включить по крайней мере одно измерение, связанное с добавляемой мерой.  
  
-   При добавлении иерархии типа «родители-потомки» в локальный куб уровни и фильтры иерархии типа «родители-потомки» не обрабатываются и добавляется вся иерархия целиком.  
  
-   Свойства элементов в локальных кубах не поддерживаются.  
  
-   Создать локальный куб из перспективы невозможно.  
  
-   При добавлении в локальный куб полуаддитивной меры применяются следующие правила.  
  
    -   Если свойство AggregateFunction для добавляемой меры равно ByAccount, то необходимо добавить измерение счетов.  
  
    -   Если свойство AggregateFunction для добавляемой меры равно FirstChild, LastChild, FirstNonEmpty, LastNonEmpty или AverageOfChildren, то необходимо полностью добавить измерение времени.  
  
-   Измерения интеллектуального анализа данных не могут быть добавлены в локальный куб.  
  
-   Ссылочные измерения материализуются и добавляются как обычные измерения.  
  
-   При добавлении измерения «многие ко многим» применяются следующие правила.  
  
    -   Измерение «многие ко многим» должно быть добавлено полностью.  
  
    -   Необходимо добавить промежуточную группу мер.  
  
    -   Необходимо добавить сущность всех измерений, общих для двух групп мер, включенных в связь «многие ко многим».  
  
 В следующем примере демонстрируется создание локального, сохраненного варианта куба Adventure Works, в котором содержится только мера Reseller Sales Amount и измерения Reseller и Date.  
  
```  
CREATE GLOBAL CUBE [LocalReseller]  
   Storage 'C:\LocalAWReseller1.cub'  
   FROM [Adventure Works]  
   (  
      MEASURE  [Adventure Works].[Reseller Sales Amount],  
      DIMENSION [Adventure Works].[Reseller],  
      DIMENSION [Adventure Works].[Date]  
   )  
```  
  
 В следующем примере иллюстрируется использование срезов при создании локального куба. Глобальный куб создается на основе куба Adventure Works путем среза по вертикали по элементу 2005 уровня Fiscal Year и по горизонтали по уровням Fiscal Year и Month.  
  
```  
CREATE GLOBAL CUBE [LocalReseller]  
   Storage 'C:\LocalAWReseller2.cub'  
   FROM [Adventure Works]  
   (  
      MEASURE  [Adventure Works].[Reseller Sales Amount],  
      DIMENSION [Adventure Works].[Reseller],  
      DIMENSION [Adventure Works].[Date]  
      (  
LEVEL [Fiscal Year],  
LEVEL [Month],  
MEMBER [Date].[Fiscal].[Fiscal Year].&[2005]  
      )  
   )  
```  
  
## <a name="see-also"></a>См. также:  
 [Инструкции определения данных многомерных выражений &#40;&#41;многомерных выражений ](../mdx/mdx-data-definition-statements-mdx.md)   
 [Инструкция CREATE SESSION CUBE &#40;многомерных выражений&#41;](../mdx/mdx-data-definition-create-session-cube.md)  
  
