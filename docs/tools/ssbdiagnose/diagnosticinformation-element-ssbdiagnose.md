---
title: Элемент DiagnosticInformation
description: В SQL Server элемент DiagnosticInformation является корневым элементом выходного XML-файла программы ssbdiagnostic.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- XML output file format [ssbdiagnose], diagnosticinformation element
- diagnosticinformation element
- ssbdiagnose
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: d77ff3145a07a72c60573c5183fa0d58da64e713
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338525"
---
# <a name="diagnosticinformation-element-ssbdiagnose"></a>Элемент DiagnosticInformation (программа ssbdiagnose)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

В элементе **DiagnosticInformation** содержатся все элементы, сообщающие о диагностических данных, обнаруженных программой. **DiagnosticInformation** — корневой элемент выходного XML-файла программы **ssbdiagnostic** .  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
<DiagnosticInformation>   
    ...   
</DiagnosticInformation>  
```  
  
## <a name="element-attributes"></a>Атрибуты элемента  
  
|attribute|Описание|  
|---------------|-----------------|  
|**None**|Недоступно|  
  
## <a name="element-characteristics"></a>Характеристики элемента  
  
|Характеристика|Описание|  
|--------------------|-----------------|  
|**Тип данных и длина**|Нет.|  
|**Значение по умолчанию**|Нет.|  
|**Наличие**|Один раз на выходной XML-файла программы **ssbdiagnose** .|  
  
## <a name="element-relationships"></a>Связи элемента  
  
|Связь|Элементы|  
|------------------|--------------|  
|**Родительский элемент**|Нет.|  
|**Дочерние элементы**|[Элемент Banner (программа ssbdiagnose)](../../tools/ssbdiagnose/banner-element-ssbdiagnose.md)<br /><br /> [Элемент Issue (программа ssbdiagnose)](../../tools/ssbdiagnose/issue-element-ssbdiagnose.md)|  
  
## <a name="remarks"></a>Remarks  
 Дополнительные сведения о пространствах имен XML см. в статье [Управление пространствами имен в XML-документе](/dotnet/standard/data/xml/managing-namespaces-in-an-xml-document).  
  
## <a name="see-also"></a>См. также:  
 [Программа ssbdiagnose (компонент Service Broker)](../../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)  
  
