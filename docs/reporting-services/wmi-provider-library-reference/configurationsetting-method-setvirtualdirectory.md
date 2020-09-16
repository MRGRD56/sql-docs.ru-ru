---
description: Метод SetVirtualDirectory (WMI MSReportServer_ConfigurationSetting)
title: Метод SetVirtualDirectory (WMI MSReportServer_ConfigurationSetting) | Документы Майкрософт
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
helpviewer_keywords:
- SetVirtualDirectory method
ms.assetid: 1a25cb1d-38d5-401a-970b-87b642a780e4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8c0ef6785a38b0b9dacce50e6b52e63979f255e1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88480586"
---
# <a name="configurationsetting-method---setvirtualdirectory"></a>Метод ConfigurationSetting — SetVirtualDirectory
  Задает имя виртуального каталога для указанного приложения.  
  
## <a name="syntax"></a>Синтаксис  
  
```vb  
Public Sub SetVirtualDirectory(ByVal Application As String, _  
    ByVal VirtualDirectory As String, ByVal lcid As Int32, _  
    ByRef [Error] As String, ByRef HRESULT As Int32)  
```  
  
```csharp  
public void SetVirtualDirectory(string Application, string VirtualDirectory,   
       int Lcid,out string Error, out int HRESULT);  
```  
  
## <a name="parameters"></a>Параметры  
 *Приложение*  
 Имя приложения, для которого задается виртуальный каталог.  
  
 *VirtualDirectory*  
 Имя виртуального каталога.  
  
 *lcid*  
 Идентификатор локали для виртуального каталога.  
  
 *Error*  
 [out] Описания возникших ошибок.  
  
 *HRESULT*  
 [out] Значение, которое указывает, окончился ли вызов успехом или сбоем.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Возвращает значение *HRESULT* , являющееся признаком успешного или неуспешного завершение вызова метода. Значение 0 означает, что вызов метода завершился успешно; код ошибки означает, что произошла ошибка.  
  
## <a name="remarks"></a>Remarks  
 Для приложения можно задать только одно имя виртуального каталога для всех зарезервированных URL-адресов.  
  
 Параметр VirtualDirectory должен соответствовать контексту именования для виртуальных каталогов. Параметр VirtualDirectory не должен быть пустой строкой.  
  
 Обновляет значение элемента \Configuration\URLReservations\Application\VirtualDirectory. Выполняется успешно, даже еще не созданы зарезервированные URL-адреса.  
  
## <a name="requirements"></a>Требования  
 **Пространство имен:** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>См. также:  
 [Элементы MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
