---
description: Начало работы с консолью SSMA для MySQL (MySQLToSQL)
title: Начало работы с SSMA для консоли MySQL (MySQLToSQL) | Документация Майкрософт
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- MySQL Console, launching console
- MySQL Console, output conventions
ms.assetid: 218d502c-059f-4d48-9aea-61e553d74303
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4c4a7e894052ec9799039d45ff5bb9e2aa29a23b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078215"
---
# <a name="getting-started-with-ssma-for-mysql-console-mysqltosql"></a>Начало работы с консолью SSMA для MySQL (MySQLToSQL)
В этом разделе описывается процедура запуска и начала работы с консольным приложением MySQL. Здесь также перечислены соглашения, используемые в типичном окне вывода консоли SSMA.  
  
## <a name="launching-ssma-console"></a>Запуск консоли SSMA  
Чтобы запустить консольное приложение SSMA, выполните следующие действия:  
  
1.  Последовательно выберите пункты **Пуск** и **все программы**.  
  
2.  Щелкните ярлык **командной строки MySQL помощник по миграции SQL Server** .  
  
    В нем отображается меню использования консоли SSMA и `(/? Help)` , которое поможет приступить к работе с консольным приложением.  
  
## <a name="procedure-for-using-the-ssma-console"></a>Процедура использования консоли SSMA  
После успешного запуска консоли в системе Windows для работы с ней можно выполнить следующие действия.  
  
1.  Настройте консоль SSMA с помощью файлов скриптов. Дополнительные сведения об этом разделе см. в разделе [Создание файлов скриптов &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-script-files-mysqltosql.md) .  
  
2.  [Создание файлов переменных значений &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-variable-value-files-mysqltosql.md)  
  
3.  [Создание файлов подключения к серверу &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-the-server-connection-files-mysqltosql.md)  
  
4.  Запуск [консоли SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/executing-the-ssma-console-mysqltosql.md) в зависимости от потребностей проекта  
  
Дополнительные функции:  
  
1.  [Защита пароля](managing-passwords-mysqltosql.md) и экспорт или импорт на других оконных компьютерах  
  
2.  [Создание отчетов](generating-reports-mysqltosql.md) для просмотра подробных отчетов с выходом в формате XML для оценки/конверсион и переноса данных. Подробные отчеты об ошибках также могут быть созданы для команд обновления и синхронизации.  
  
## <a name="ssma-console-output-conventions"></a>Соглашения о выходных данных консоли SSMA  
После выполнения команд и параметров сценария SSMA консольная программа отображает результаты и сообщения (сведения, ошибка и т. д.) пользователю на консоли или, если необходимо, перенаправляет в выходной XML-файл. Каждый тип сообщения в выходных данных обозначается уникальным цветом. Например, текстовое сообщение в белом цвете означает команды файла сценария; зеленый цвет представляет собой запрос на ввод пользователя и т. д.  
  
![Снимок экрана, показывающий пример выходных данных MySQL консоли SSMA.](../../ssma/mysql/media/ssmaconsoleoutput_mysql.jpg "Вывод консоли SSMA_MySQL")  
  
Цветовая интерпретация выходных данных консоли в следующей таблице:  
  
|Color|Description|  
|---------|---------------|  
|Красный|Неустранимая ошибка во время выполнения|  
|Серый|Отметка даты и времени, сообщение пользователю|  
|White|Команды файла сценария, тип сообщения|  
|Желтый|Предупреждение|  
|Зеленый|Запрос на ввод данных пользователем|  
|Цвет|Начало, окончание и результат операции|  
  
## <a name="see-also"></a>См. также:  
[Установка SSMA для MySQL](installing-ssma-for-mysql-mysqltosql.md)  
  
