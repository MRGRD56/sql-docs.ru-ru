---
title: сравнить и синхронизировать данные из двух баз данных
description: Сведения о том, как сравнить и синхронизировать данные из двух баз данных. Сведения о настройке сравнения, просмотре различий и обновлении целевого объекта.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.datacompare.connection.datasources.f1
- sql.data.tools.datacompare.watermark.f1
- sql.data.tools.datacompare.connection.objectselection.f1
ms.assetid: 2148e517-ed42-41c6-b753-1ac625f594c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 03/22/2021
ms.openlocfilehash: d869aaf652cf3c5d9baa389ab26303ba46049b71
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890721"
---
# <a name="how-to-compare-and-synchronize-the-data-of-two-databases"></a>Руководство. сравнить и синхронизировать данные из двух баз данных

Предусмотрена возможность сравнивать данные, содержащиеся в двух базах данных. Сравниваемые базы данных принято называть *базой данных-источником* и *целевой базой данных*.  
  
> [!NOTE]  
> При сравнении данных *проекты баз данных*, а также пакеты DACPAC или BACPAC не могут рассматриваться как база данных-источник или целевая база данных.  
  
По мере сравнения данных формируется скрипт *языка для обработки данных (DML)*, который можно использовать для синхронизации различающихся баз данных, обновив некоторые или все данные в целевой базе данных. После сравнения данных результаты открываются в окне "Сравнение данных" в Visual Studio.  
  
После того как сравнение заканчивается, можно перейти к выполнению следующих шагов:  
  
-   Просмотр различий между двумя базами данных. Дополнительные сведения см. в разделе [Просмотр различий данных](#ViewDifferences).  
  
-   Обновление всей или части целевой базы данных для согласования с исходной базой данных. Дополнительные сведения см. в разделе [Синхронизация данных базы данных](#Synchronize).  
  
Дополнительные сведения см. в статье [Сравнение и синхронизация данных из одной или нескольких таблиц с данными из эталонной базы данных](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
> [!NOTE]  
> Можно также сравнивать *схемы* двух баз данных или две версии одной и той же базы данных. Дополнительные сведения см. в разделе [Как использовать сравнение схем для сопоставления различных определений баз данных](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md).  
  
## <a name="comparing-database-data"></a><a name="CompareDatabaseData"></a>Сравнение данных базы данных  
  
#### <a name="to-compare-data-by-using-the-new-data-comparison-wizard"></a>Сравнение данных с использованием мастера создания сравнения данных  
  
1.  В меню **SQL** выберите **Сравнение данных**, затем щелкните **Создать сравнение данных**.  
  
    Произойдет запуск мастера создания сравнения данных. Также откроется окно "Сравнение данных" и Visual Studio автоматически назначит сравнению имя, например DataCompare1.  
  
2.  Укажите базу данных-источник и целевую базы данных.  
  
    Если список **База данных-источник** или **Целевая база данных** пуст, щелкните **Создать соединение**. В диалоговом окне **Свойства соединения** укажите сервер, на котором находится база данных, и тип аутентификации, применяемой при подключении к базе данных. Затем нажмите кнопку **ОК**, чтобы закрыть диалоговое окно **Свойства соединения** и вернуться к мастеру сравнения данных.  
  
    На первой странице в мастере сравнения данных проверьте правильность сведений о каждой базе данных, укажите, какие записи требуется включить в результаты, а затем щелкните **Далее**. Откроется вторая страница мастера сравнения данных с отображением иерархического списка таблиц и представлений в базе данных.  
  
3.  Отметьте флажки, относящиеся к таблицам и представлениям, которые должны быть включены в сравнение. При желании можно также развертывать узлы, относящиеся к объектам базы данных, затем отмечать флажки, которые относятся к столбцам этих объектов, подлежащим сравнению.  
  
    > [!NOTE]  
    > Для включения в список таблицы и представления должны соответствовать двум условиям. Во-первых, в базе данных-источнике и целевой базе данных должны согласовываться схемы объектов. Во-вторых, в списке появляются только таблицы и представления, имеющие первичный ключ, уникальный ключ, уникальный индекс или ограничение уникальности. Если таблицы или представления, соответствующие обоим условиям, отсутствуют, список остается пустым.  
  
4.  При наличии более одного ключа можно воспользоваться столбцом **Ключ сравнения**, чтобы указать ключ, на котором должно быть основано сравнение данных. Например, можно указать, должно основываться сравнение на столбце первичного ключа или на другом (однозначно определяемом) ключевом столбце.  
  
5.  Нажмите кнопку **Готово**.  
  
    Начинается сравнение.  
  
    > [!NOTE]  
    > Можно остановить выполняемую операцию сравнения данных. Откройте меню **SQL**, а затем последовательно выберите **Сравнение данных** и **Остановить сравнение данных**.  
  
    После завершения сравнения можно просмотреть различия данных между этими двумя базами данных. Можно также обновить часть или все данные в целевой базе данных для согласования с данными в базе данных-источнике.  
  
#### <a name="to-compare-data-by-using-the-visual-studio-automation-model"></a>Сравнение данных с использованием модели автоматизации Visual Studio  
  
1.  В меню **Вид** выберите пункт **Другие окна** и щелкните **Окно команд**.  
  
2.  Введите в окне команд следующую команду:  
  
    ```  
    Tools.NewDataComparison /SrcServerName sServerName /SrcDatabaseName sDatabaseName /SrcUserName sUserName /SrcPassword sPassword /SrcDisplayName sDisplayName /TargetServerName tServerName /TargetDatabaseName tDatabaseName /TargeUserName tUserName /TargetPassword tPassword /TargetDisplayName tDisplayName  
    ```  
  
    Замените заполнители (*sServerName*, *sDatabaseName*, *sUserName*, *sPassword*, *sDisplayName*, *tServerName*, *tDatabaseName*, *tUserName*, *tPassword* и *tDisplayName*) значениями для ваших базы данных-источника и целевой базы данных.  
  
    Если база данных-источник и целевая база данных не указаны, откроется диалоговое окно **Новое сравнение данных**. Дополнительные сведения о параметрах для команды Tools.NewDataComparison см. в [справочнике по командам автоматизации для функций баз данных в Visual Studio Team System](/previous-versions/visualstudio/visual-studio-2010/dd470565(v=vs.100)).  
  
    Сравнение данных в указанных базах данных, исходной и целевой, выполнено. Результаты отображаются в сеансе «Сравнение данных». Дополнительные сведения о просмотре результатов и синхронизации данных см. в разделах [Просмотр различий данных](#ViewDifferences) и [Синхронизация данных базы данных](#Synchronize).  
  
## <a name="viewing-data-differences"></a><a name="ViewDifferences"></a>Просмотр различий данных  
После сравнения данных в двух базах данных в окне "Сравнение данных" отображаются все сравниваемые *объекты баз данных* и сведения об их состоянии. Можно также просматривать результаты, относящиеся к записям в каждом объекте, сгруппированные по состояниям. Дополнительные сведения об обозначениях состояния см. в статье [Сравнение и синхронизация данных из одной или нескольких таблиц с данными из эталонной базы данных](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
После просмотра различий можно обновить целевую базы данных для согласования с исходной базой данных применительно к некоторым или всем различающимся, недостающим или новым объектам или записям. Дополнительные сведения см. в разделе [Синхронизация данных базы данных](#Synchronize).  
  
#### <a name="to-view-data-differences"></a>Просмотр различий данных  
  
1.  Сравните данные в базе данных-источнике и целевой базе данных. Дополнительные сведения см. в разделе [Сравнение данных базы данных](#CompareDatabaseData).  
  
2.  (Необязательно) Выполните одно или оба следующих действия.  
  
    -   По умолчанию отображаются результаты для всех объектов независимо от их состояния. Чтобы отображались только объекты с определенным состоянием, щелкните один из вариантов в списке **Фильтр**.  
  
    -   Для просмотра результатов, относящихся к записям определенного объекта, щелкните объект в основной области результатов, затем откройте вкладку на панели просмотра записей. На каждой вкладке отображаются все записи данного объекта, имеющие определенное состояние: «различающиеся», «только в базе данных-источнике», «только в целевой базе данных» и «идентичные». Данные отображаются по записям и столбцам.  
  
## <a name="synchronizing-database-data"></a><a name="Synchronize"></a>Синхронизация данных базы данных  
После сравнения данных в двух базах данных их можно синхронизировать, обновляя всю или часть целевой базы данных для согласования с исходной базой данных. Предусмотрена возможность сравнивать данные в объектах базы данных двух типов: таблицы и представления.  
  
#### <a name="to-update-target-data-by-using-the-write-updates-command"></a>Обновление данных целевой базы данных с использованием команды записи обновлений  
  
1.  Сравните данные в базе данных-источнике и целевой базе данных. Дополнительные сведения см. в разделе [Сравнение данных базы данных](#CompareDatabaseData).  
  
    После завершения сравнения в окне «Сравнение данных» отображаются результаты, относящиеся к сравниваемым объектам. Сведения об объектах, позволяющие судить об их идентичности, представлены в четырех столбцах (именуемых «Различные записи», «Только в базе данных-источнике», «Только в целевой базе данных» и «Идентичные записи»). Для каждого такого объекта в этих столбцах приведены данные о том, сколько различающихся записей было обнаружено и сколько записей должно быть изменено в операции обновления. Вначале эти два числа совпадают, но в шаге 4 можно изменить состав обновляемых объектов.  
  
    Дополнительные сведения см. в статье [Сравнение и синхронизация данных из одной или нескольких таблиц с данными из эталонной базы данных](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  Щелкните строку таблицы в окне «Сравнение данных».  
  
    В области сведений появятся результаты, относящиеся к записям в объекте базы данных, на котором сделан щелчок. Записи группируются по состояниям на вкладках, которые можно использовать для указания данных, подлежащих распространению из исходной базы данных в целевую.  
  
3.  В области сведений откройте вкладку, в имени которой содержится число, отличное от (0).  
  
    Столбец **Обновление** таблицы **данных, присутствующих только в целевой базе данных**, содержит флажки, которые можно использовать для выбора строк, подлежащих обновлению. По умолчанию выбраны все флажки.  
  
4.  Очистите флажки, относящиеся к записям в целевой базе данных, которые не должны быть обновлены данными из исходной базы данных.  
  
    Очистка каждого флажка приводит к уменьшению числа обновляемых записей, и содержимое окна изменяется, отражая ваши действия. Это число появляется в строке состояния области сведений и в соответствующем столбце в основной области результатов, как описано на шаге 1.  
  
5.  Щелкните **Создать скрипт** (необязательно).  
  
    Откроется окно редактора Transact\-SQL с отображением скрипта для *языка обработки данных*  (DML), который предназначен для обновления целевой базы данных.  
  
6.  Чтобы синхронизировать различающиеся, недостающие или новые записи, щелкните **Обновить целевой объект**.  
  
    > [!NOTE]  
    > Выполняемую операцию обновления целевой базы данных можно отменить. Щелкните **Остановить запись в целевой объект**.  
  
    Данные в выбранных записях целевой базы данных обновляются данными из соответствующих записей исходной базы данных.  
  
    > [!NOTE]  
    > При обновлении индексированных представлений операция **Обновить целевой объект** может окончиться неудачей, если это действие приводит к вставке повторяющихся ключей в одну и ту же таблицу.  
  
#### <a name="to-update-target-data-by-using-a-transact-sql-script"></a>Обновление данных целевой базы данных с использованием скрипта Transact\-SQL  
  
1.  Сравните данные в базе данных-источнике и целевой базе данных. Дополнительные сведения см. в разделе [Сравнение данных базы данных](#CompareDatabaseData).  
  
    После завершения сравнения в окне «Сравнение данных» отображаются сравниваемые объекты. Дополнительные сведения см. в статье [Сравнение и синхронизация данных из одной или нескольких таблиц с данными из эталонной базы данных](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  (Необязательно) В области сведений очистите флажки, относящиеся к записям целевой базы данных, которые не должны быть обновлены, как описано в предыдущей процедуре.  
  
3.  Щелкните **Создать скрипт**.  
  
    Откроется окно с отображением скрипта Transact\-SQL, который должен распространить изменения, необходимые для согласования данных целевой базы данных с данными базы данных-источника. Этому окну присваивается имя наподобие **DataUpdate_Database_1.sql**.  
  
    В данном скрипте отражаются изменения, внесенные в области сведений. Например, предположим, что на странице «Только в целевой базе данных» очищен флажок, относящийся к какой-то строке таблицы [dbo].[Shippers]. В этом случае скрипт не обновит эту строку.  
  
4.  Измените скрипт в окне **DataUpdate_Database_1.sql** (необязательно).  
  
5.  (Необязательно, но рекомендовано) Создайте резервную копию целевой базы данных.  
  
6.  Щелкните **Выполнить**, чтобы обновить целевую базу данных.  
  
    Укажите соединение с целевой базой данных, которую необходимо обновить.  
  
    > [!IMPORTANT]  
    > По умолчанию обновление происходит в рамках транзакции. При возникновении ошибок можно выполнить откат всего обновления. Это поведение можно изменить.  
  
    Данные в выбранных записях целевой базы данных обновляются данными из соответствующих записей исходной базы данных.  
  
## <a name="see-also"></a>См. также:  
[Сравнение и синхронизация данных из одной или нескольких таблиц с данными из эталонной базы данных](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)  
