---
description: Сопоставление типов данных в мастере импорта и экспорта SQL Server
title: Сопоставление типов данных в мастере импорта и экспорта SQL Server | Документы Майкрософт
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 669be403-cb17-4b12-bbbf-e7a74003c4b6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 946bb57a3d821186ebcca132539713cf515ab20f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122778"
---
# <a name="data-type-mapping-in-the-sql-server-import-and-export-wizard"></a>Сопоставление типов данных в мастере импорта и экспорта SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 В мастере импорта и экспорта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно задать имя, тип данных и свойства типа данных для столбцов в новых целевых таблицах и файлах, но нельзя указать настраиваемые преобразования для значений столбцов. Поэтому важное значение имеет встроенное сопоставление типов данных из источника с типами данных в назначении.  
  
##  <a name="how-does-the-wizard-map-data-types-between-source-and-destination"></a><a name="wizardMapping"></a> Каким образом мастер выполняет сопоставление типов данных источника и назначения?
При сопоставлении типов данных из одной системы или версии базы данных с другой мастер использует файлы сопоставления, устанавливаемые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Например, он может сопоставить типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с типами данных Oracle. По умолчанию файлы сопоставления в XML-формате устанавливаются в следующие папки.
-   **C:\Program Files\Microsoft SQL Server\130\DTSMappingFiles\\** (для 64-разрядной версии)
-   **C:\Program Files (x86)\Microsoft SQL Server\130\DTSMappingFiles\\** (для 32-разрядной версии).  
  
 Если существующий файл сопоставления был изменен или в папку был добавлен новый файл сопоставления, необходимо закрыть и заново открыть мастер импорта и экспорта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или среду [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] , чтобы загрузить новые или измененные файлы.  
 
## <a name="you-can-change-an-existing-mapping-file"></a>Можно изменить существующий файл сопоставления
Если требуются различные сопоставления между типами данных, можно обновить файлы сопоставлений, чтобы изменить сопоставления, используемые мастером. Например, если при передаче данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в DB2 необходимо сопоставить тип данных  **nchar** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с типом данных **GRAPHIC** DB2, а не с типом данных **VARGRAPHIC** DB2, то в файле сопоставления **SqlClientToIBMDB2.xml** необходимо изменить сопоставление **nchar** для использования типа **GRAPHIC** вместо **VARGRAPHIC**.  
  
## <a name="you-can-add-a-new-mapping-file"></a>Можно добавить новый файл сопоставления
[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] устанавливает сопоставления между многими часто используемыми комбинациями источника и назначения. Можно также добавить новые файлы сопоставления в каталог **MappingFiles** для поддержки дополнительных источников и назначений. Новые файлы сопоставления должны быть согласованы с опубликованной XSD-схемой и выполнять сопоставления между уникальными сочетаниями, источниками и назначениями. Схема для файлов сопоставления ( **DataTypeMapping.xsd**) опубликована [здесь](https://schemas.microsoft.com/sqlserver/2008/07/IntegrationServices/DataTypeMapping/DataTypeMapping.xsd).
 
## <a name="sample-mapping-file"></a>Образец файла сопоставления
Вот фрагмент XML-файла сопоставления, который сопоставляет типы данных SQL Server (или, точнее, из типов данных, используемых поставщиком данных .Net Framework для SQL Server) с типами данных Oracle. Можно видеть, что тип данных SQL Server **int** сопоставляется с типом данных Oracle **INTEGER** .
  
```xml  
  
<dtm:DataTypeMappings  
    xmlns:dtm="https://www.microsoft.com/SqlServer/Dts/DataTypeMapping.xsd"   
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    SourceType="System.Data.SqlClient.SqlConnection"   
    MinSourceVersion="*"   
    MaxSourceVersion="*"   
    DestinationType="MSDAORA;OraOLEDB.Oracle;System.Data.OracleClient.OracleConnection"   
    MinDestinationVersion="08.*"   
    MaxDestinationVersion="*">  
  
    <!-- smallint -->  
    <dtm:DataTypeMapping >  
        <dtm:SourceDataType>  
            <dtm:DataTypeName>smallint</dtm:DataTypeName>  
        </dtm:SourceDataType>  
        <dtm:DestinationDataType>  
            <dtm:SimpleType>  
                <dtm:DataTypeName>INTEGER</dtm:DataTypeName>  
            </dtm:SimpleType>  
        </dtm:DestinationDataType>  
    </dtm:DataTypeMapping>    
  
    <!-- int -->  
    <dtm:DataTypeMapping >  
        <dtm:SourceDataType>  
            <dtm:DataTypeName>int</dtm:DataTypeName>  
        </dtm:SourceDataType>  
        <dtm:DestinationDataType>  
            <dtm:SimpleType>  
                <dtm:DataTypeName>INTEGER</dtm:DataTypeName>  
            </dtm:SimpleType>  
        </dtm:DestinationDataType>  
    </dtm:DataTypeMapping>    
  
        ...  
  
</dtm:DataTypeMappings>  
  
```  

