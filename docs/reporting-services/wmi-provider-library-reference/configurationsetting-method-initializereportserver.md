---
description: Метод InitializeReportServer (WMI MSReportServer_ConfigurationSetting)
title: Метод InitializeReportServer (WMI MSReportServer_ConfigurationSetting) | Документы Майкрософт
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- InitializeReportServer (WMI MSReportServer_ConfigurationSetting Class)
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- InitializeReportServer method
ms.assetid: 0304acc2-1fd7-437b-94d9-1c1073dd3ca4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e38c90a85039fe8f4da6098faff4c8224de64ef1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423188"
---
# <a name="configurationsetting-method---initializereportserver"></a>Метод ConfigurationSetting — InitializeReportServer
  Инициализирует указанный экземпляр службы отчетов.  
  
## <a name="syntax"></a>Синтаксис  
  
```vb  
Public Sub InitializeReportServer(ByVal InstallationID As String, _  
    ByRef HRESULT As Int32, ByRef ExtendedErrors() As String)  
```  
  
```csharp  
public void InitializeReportServer(string InstallationID,   
    out Int32 HRESULT, out string[] ExtendedErrors);  
```  
  
## <a name="parameters"></a>Параметры  
 *InstallationID*  
 Строка, используемая для шифрования ключа шифрования перед его возвращением.  
  
 *HRESULT*  
 [out] Значение, которое указывает, окончился ли вызов успехом или сбоем.  
  
 *ExtendedErrors[]*  
 [out] Массив строк, содержащий дополнительные ошибки, возвращенные в результате вызова.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Возвращает значение *HRESULT* , являющееся признаком успешного или неуспешного завершение вызова метода. Значение 0 указывает, что вызов метода завершился успешно. Ненулевое значение указывает, что произошла ошибка.  
  
## <a name="remarks"></a>Remarks  
 При вызове этого метода ключ шифрования, который обращается к защищенным сведениям в базе данных сервера отчетов, зашифровывается с помощью открытого ключа сервера отчетов, определенного параметром *InstallationID*.  
  
 Указанный открытый ключ сервера отчетов должен быть предварительно записан в базу данных сервера отчетов.  
  
 Метод *InitializeReportServer* должен вызываться для сервера отчетов, который уже имеет доступ к защищенным сведениям, и поэтому может расшифровать ключ шифрования. Полученный зашифрованный ключ шифрования сохраняется в базе данных сервера отчетов.  
  
 Если свойство [IsInitialized](../../reporting-services/wmi-provider-library-reference/configurationsetting-property-isinitialized.md) сервера отчетов имеет значение **true** при вызове метода InitializeReportServer, то метод завершается успешно без попытки зашифровать ключ шифрования.  
  
## <a name="requirements"></a>Требования  
 **Пространство имен:** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>См. также:  
 [Элементы MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
