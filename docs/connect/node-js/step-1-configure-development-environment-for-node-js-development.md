---
title: Шаг 1. Настройка среды разработки для Node.js
description: Чтобы разработать приложение с помощью драйвера Node.js для SQL Server, необходимо настроить среду разработки, учитывая необходимые условия.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 2dad01f1-fadf-4ac9-9b4d-26be3d301886
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 38337772d9ec9db2503637122d0d1b616dc6ef5f
ms.sourcegitcommit: 1a96abbf434dfdd467d0a9b722071a1ca1aafe52
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81528138"
---
# <a name="step-1--configure-development-environment-for-nodejs-development"></a>Шаг 1.  Настройка среды разработки для разработки на Node.js
Чтобы разработать приложение с помощью драйвера Node.js для SQL Server, необходимо настроить среду разработки, учитывая необходимые условия.  Наиболее распространенным способом для установки трудоемкого модуля является использование диспетчера пакетов узла (npm), однако вы также можете скачать трудоемкий модуль непосредственно в [GitHub](https://github.com/pekim/tedious).  
  
Драйвер Node.js использует протокол TDS, включенный по умолчанию в SQL Server и Базу данных SQL Azure.  Дополнительная настройка не требуется.  
  
## <a name="windows"></a>Windows  
  
1. **Установите среду выполнения Node.js и диспетчер пакетов npm.**  
а. Перейдите к [Node. js](https://nodejs.org/en/download/)  
b. Нажмите на соответствующую ссылку установщика msi Windows.   
c. После загрузки запустите msi, чтобы установить Node.js.  
  
2. **Откройте cmd.exe**  
  
3. **Создайте каталог проекта** и перейдите к нему.    
```  
> mkdir HelloWorld  
> cd HelloWorld  
```  
4. **Создайте проект Node.**  Чтобы сохранить настройки по умолчанию во время создания проекта, нажимайте клавишу ВВОД, пока проект не будет создан. В конце этого шага в каталоге проекта вы увидите файл package.json.  
```  
> npm init  
```  
  
5. **Установите модуль Tedious в проект.**  Tedious — это реализация протокола TDS, который используется для взаимодействия с SQL Server.  
```  
> npm install tedious  
```  
  
## <a name="ubuntu-linux"></a>Ubuntu Linux  
  
1.  **Откройте терминал**  
  
2. **Установите среду выполнения Node.js.**  
```  
>sudo apt-get install node  
```  
3. **Установите npm (диспетчер пакетов узла).**  
```  
> sudo apt-get install npm  
```  
4. **Создайте каталог проекта** и перейдите к нему.    
```  
> mkdir HelloWorld  
> cd HelloWorld  
```  
  
5. **Создайте проект Node.**  Чтобы сохранить настройки по умолчанию во время создания проекта, нажимайте клавишу ВВОД, пока проект не будет создан. В конце этого шага в каталоге проекта вы увидите файл package.json.  
```  
> sudo npm init  
```  
  
6. **Установите трудоемкий модуль в проект.**  Tedious — это реализация протокола TDS, который используется для взаимодействия с SQL Server.  
```  
> sudo npm install tedious  
```  
  
## <a name="macos"></a>macOS  
  
1. **Установите среду выполнения Node.js и диспетчер пакетов npm.**  
а. Перейдите к [Node. js](https://nodejs.org/en/download/)  
b. Щелкните ссылку на соответствующий установщик macOS.  
c. После загрузки запустите dmg, чтобы установить Node.js.  
  
2. **Откройте терминал**  
  
3. **Создайте каталог проекта** и перейдите к нему.    
```  
> mkdir HelloWorld  
> cd HelloWorld  
```  
  
4. **Создайте проект Node.**  Чтобы сохранить настройки по умолчанию во время создания проекта, нажимайте клавишу ВВОД, пока проект не будет создан. В конце этого шага в каталоге проекта вы увидите файл package.json.  
```  
> npm init  
```  
  
5. **Установите трудоемкий модуль в проект.**  Это реализация протокола TDS, который используется драйвером для взаимодействия с SQL Server.  
```  
> npm install tedious  
```  
