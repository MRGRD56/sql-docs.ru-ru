---
title: Преобразование схем XDR с заметками в XSD (SQLXML)
description: Сведения о преобразовании схемы XDR с заметками в эквивалентную схему XSD с помощью средства преобразования XDR в XSD в SQLXML 4,0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XDR schemas, converting schemas
- annotated XSD schemas, converting schemas
- XDR to XSD Converter tool [SQLXML]
- XDR schemas [SQLXML], converting
- converting annotated schemas
- mapping schema [SQLXML], conversions
- XSD schemas [SQLXML], converting schemas
ms.assetid: 151c94a8-66d3-4c46-a5ff-a22df456940a
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5537c25a87d0bcfe9ec0fe0d882053c828cb4c31
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491390"
---
# <a name="converting-annotated-xdr-schemas-to-equivalent-xsd-schemas-sqlxml-40"></a>Преобразование схем XDR с заметками в эквивалентные схемы XSD (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Язык определения схем XML (XSD) пришел на смену языку определения схем с сокращенными XML-данными (XDR). После ввода в действие поддержки XSD в [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 предполагается, что новые схемы с заметками должны создаваться при помощи XSD. SQLXML 4.0 включает средство преобразования XDR в XSD, которое создано для предоставления помощи пользователю в преобразовании существующих схем XDR с заметками в равнозначные схемы XSD.  
  
> [!IMPORTANT]  
>  Это средство рекомендуется использовать только для преобразования схем XDR в XSD в целях их применения с SQLXML 4.0. Это не следует считать средством преобразования XDR в XSD общего назначения. Преобразованные схемы XSD могут не действовать в полном соответствии с исходными схемами XDR, будучи используемыми в других средах.  
  
 Если входной XDR-файл задает в XML-декларации кодировку, она становится кодировкой создаваемого выходного XSD-файла.  
  
 Средство преобразования (Cvtschema.exe) установлено в папке Program Files\SQLXML 4.0\bin и исполняется в командной строке.  
  
 Далее представлен общий синтаксис:  
  
```  
cvtschema XDRFileName, [-y], [-w] [-?]  
```  
  
 Где:  
  
 XDRFileName  
 Имя XDR-файла, который необходимо преобразовать в XSD. Это средство читает входной XDR-файл и создает выходной XSD-файл в текущем рабочем каталоге. Если текущий файл имеет расширение XDR или XML, выходной XSD-файл создается с тем же именем, но с расширением XSD. Если расширение имени входного файла отличается от. XML или. XDR (или если расширение отсутствует), выходной файл создается с тем же именем, а расширение XSD добавляется к имени входного файла. Например, если именем входного XDR-файла является SampleFile.abc, то полученный XSD-файл сохраняется как SampleFile.abc.xsd.  
  
 -y  
 Перезаписывает существующий XSD-файл XSD-файлом, созданным средством преобразования (необязательно). Если этот флаг не указан, в этом средстве происходит вывод приглашения для пользователя, которое позволяет указать, следует ли переписать существующий XSD-файл и изменить имя выходного файла.  
  
 -w  
 Возвращает некритичные предупреждения, сформированные этим средством в процессе преобразования (необязательно). По умолчанию это средство отображает только сообщения, относящиеся к неустранимым ошибкам.  
  
 -?  
 Возвращает список параметров, которые можно указать с помощью **cvtschema**, а также объяснение.  
  
## <a name="see-also"></a>См. также  
 [Сопоставление типов данных XSD с типами данных XPath &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md)   
 [Аннотации XSD &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/xsd-annotations-sqlxml-4-0.md)  
  
  
