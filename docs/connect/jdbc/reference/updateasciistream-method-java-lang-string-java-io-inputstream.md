---
description: Метод updateAsciiStream (java.lang.String, java.io.InputStream)
title: Метод updateAsciiStream (java.lang.String, java.io.InputStream)
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 747b0308-1ce6-4eba-bdfc-af29c21c18cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5f42bda7c4938d1dc0a4ed3fad9bf446c63bad2d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88478516"
---
# <a name="updateasciistream-method-javalangstring-javaioinputstream"></a>Метод updateAsciiStream (java.lang.String, java.io.InputStream)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Обновляет значение ASCII-потока в указанном столбце.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public void updateAsciiStream(java.lang.String columnLabel,  
                              java.io.InputStream x)  
```  
  
#### <a name="parameters"></a>Параметры  
 *columnLabel*  
  
 Значение типа **String**, содержащее метку столбца.  
  
 *x*  
  
 Объект InputStream.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод updateAsciiStream задается с помощью метода updateAsciiStream в интерфейсе java.sql.ResultSet.  
  
 Этот метод передает ASCII-символы (байты) из объекта InputStream в столбцы символьных значений, поддерживающие преобразование, то есть содержащие символы Юникода в диапазоне ASCII (от 0x00 до 0x7F) и соответствующие кодовым страницам 874, 932, 936, 949, 950 и с 1250 по 1258. Этот метод выполняет преобразование на целевую страницу параметров сортировки. Попытка обновления целевого столбца, не поддерживающего преобразование, приведет к возникновению исключения. В столбцах двоичных значений передаются необработанные байты.  
  
 Использование этого метода при работе с типами данных **image**, **text** и **ntext**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] может повлиять на производительность.  
  
## <a name="see-also"></a>См. также:  
 [Метод updateAsciiStream &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)   
 [Элементы SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Класс SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
