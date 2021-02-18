---
title: Установка документации по SQL Server для просмотра в автономном режиме
description: Сведения об установке автономной документации для SQL Server 2019, 2017, 2016, 2014 и 2012. Используйте SQL Server Management Studio (SSMS) для просмотра автономного содержимого.
ms.prod: sql
ms.technology: install
ms.topic: conceptual
ms.assetid: 51f8a08c-51d0-41d8-8bc5-1cb4d42622fb
author: markingmyname
ms.author: maghan
ms.date: 08/12/2020
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: ab5ee55bf91e1a9287f22584da83ca7112dadece
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336776"
---
# <a name="install-sql-server-documentation-to-view-offline-in-ssms"></a>Установка документации по SQL Server для просмотра в автономном режиме в SSMS

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

В этой статье описывается загрузка и просмотр автономного содержимого SQL Server в [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md). Автономное содержимое позволяет получить доступ к документации без подключения к Интернету (хотя изначально для загрузки требуется подключение к Интернету).

Автономная документация доступна для SQL Server 2012 и более поздних версий. Хотя вы можете [просматривать содержимое для предыдущих версий в Интернете](/previous-versions/sql/), параметр автономного режима обеспечивает удобный способ доступа к старому содержимому.

- [SQL Server 2016 и более поздние версии](#sql-server-2016-and-later-offline-content)
- [SQL Server 2014](#sql-server-2014-offline-content)
- [SQL Server 2012](#sql-server-2012-offline-content)

## <a name="sql-server-2016-and-later-offline-content"></a>Автономное содержимое SQL Server 2016 и более поздних версий

Ниже описаны действия по загрузке автономного содержимого для SQL Server 2016 и более поздних версий.

1. В SSMS в меню "Справка" выберите пункт **Добавление и удаление содержимого справки**.

   ![Добавление и удаление содержимого справки](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   В окне справки откроется вкладка "Управление содержимым".

2. Чтобы найти новейшее содержимое справки для SQL Server 2016 и более поздних версий, на вкладке **Управление содержимым** в источнике установки выберите **В сети**, а затем введите в строке поиска *sql server*.

   ![Поиск книг по SQL Server](../sql-server/media/sql-server-offline-documentation/sql-online-search.png)

   > [!Note]
   > На вкладке "Управление содержимым" в поле Local store path (Путь к локальному хранилищу) отображается место установки содержимого на локальном компьютере. Чтобы изменить расположение, щелкните **Переместить**, в поле **Куда** введите путь к другой папке, а затем нажмите кнопку **OK**. Если не удается установить справку после изменения пути к локальному хранилищу, закройте и снова откройте окно справки. Убедитесь, что новое расположение есть в пути к локальному хранилищу, а затем повторите попытку установки.

3. Чтобы установить новейшее содержимое справки для SQL Server 2016 и более поздних версий, выберите **Добавить** рядом с каждым пакетом содержимого (книгой), который необходимо установить, а затем выберите **Обновить** в правом нижнем углу.

   ![Добавление и обновление электронных книг по SQL Server](../sql-server/media/sql-server-offline-documentation/sql-add-update.png)

   > [!NOTE]
   > Если окно справки перестает отвечать на запросы во время добавления содержимого, измените значение в строке Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" в файле %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings или HlpViewer_VisualStudiox_en-US.settings на дату в будущем. Сведения об этой проблеме см. в разделе [Окно справки Visual Studio зависает](/visualstudio/welcome-to-visual-studio).

4. Проверить загрузку содержимого SQL Server 2016 и более поздних версий можно, выполнив поиск в области содержимого, введя *sql server 2016*.

   ![Автоматическое обновление документации SQL Server 2016](../sql-server/media/sql-server-offline-documentation/sql-2016-content.png)

## <a name="sql-server-2014-offline-content"></a>Автономное содержимое для SQL Server 2014

> [!IMPORTANT]
> Содержимое для SQL 2014 Transact-SQL доступно только в автономном режиме.

Ниже описаны действия по загрузке автономного содержимого для SQL Server 2014.

1. Скачайте содержимое [документации по продукту Microsoft SQL Server 2014 для сред с брандмауэром и прокси-сервером](https://www.microsoft.com/download/details.aspx?id=42557) из центра загрузки и сохраните его в папке.

2. Распакуйте файл, чтобы просмотреть файл *MSHA*.

   ![Файл установки справочной документации по SQL Server 2014](../sql-server/media/sql-server-offline-documentation/sql-2014-help-content-setup-msha.png)

3. В SSMS в меню "Справка" выберите пункт **Добавление и удаление содержимого справки**.

   ![Пункт меню "Добавление и удаление содержимого справки" в окне справки](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   В окне справки откроется вкладка "Управление содержимым".

4. Чтобы установить новейшее содержимое справки, выберите **Диск** в разделе источника установки, а затем — многоточие (...).

   ![Выбор диска в качестве источника на вкладке "Управление содержимым" в окне справки](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > На вкладке "Управление содержимым" в поле Local store path (Путь к локальному хранилищу) отображается расположение содержимого на локальном компьютере. Чтобы изменить расположение, щелкните **Переместить**, в поле **Куда** введите путь к другой папке, а затем нажмите кнопку **OK**.
   Если не удается установить справку после изменения пути к локальному хранилищу, закройте и снова откройте средство просмотра справки. Убедитесь, что новое расположение есть в пути к локальному хранилищу, а затем повторите попытку установки.

5. Откройте папку, в которую вы распаковали содержимое. Выберите в папке файл **HelpContentSetup.msha**, а затем щелкните **Открыть**.

   ![Открытие файла HelpContentSetup.msha SQL Server 2014](../sql-server/media/sql-server-offline-documentation/sql-2014-open-msha.png)

6. В строке поиска введите *sql server 2014*. Получив доступ к содержимому 2014, выберите **Добавить** рядом с каждым пакетом содержимого (документацией), который необходимо установить в окне справки, а затем выберите **Обновить**.

   ![Поиск документации SQL Server 2014 в окне справки](../sql-server/media/sql-server-offline-documentation/sql-2014-search.png)

   ![Добавление и обновление документации SQL Server 2014 в окне справки](../sql-server/media/sql-server-offline-documentation/sql-2014-add-update.png)

    > [!NOTE]
    > Если окно справки перестает отвечать на запросы во время добавления содержимого, измените значение в строке Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" в файле %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings или HlpViewer_VisualStudiox_en-US.settings на дату в будущем. Сведения об этой проблеме см. в разделе [Окно справки Visual Studio зависает](/visualstudio/welcome-to-visual-studio).

7. Проверить загрузку содержимого SQL Server 2014 можно, выполнив поиск в области содержимого слева, введя *sql server 2014*.

   ![Автоматическое обновление документации SQL Server 2014](../sql-server/media/sql-server-offline-documentation/sql-2014-content.png)

## <a name="sql-server-2012-offline-content"></a>Автономное содержимое для SQL Server 2012

Ниже описаны действия по загрузке автономного содержимого для SQL Server 2012.

1. Скачайте содержимое [документации по продукту Microsoft SQL Server 2012 для сред с брандмауэром и прокси-сервером](https://www.microsoft.com/download/details.aspx?id=35750) из центра загрузки и сохраните его в папке.

2. Распакуйте файл, чтобы просмотреть файл *MSHA*.

   ![Файл установки содержимого справки SQL Server 2012](../sql-server/media/sql-server-offline-documentation/sql-2012-help-content-setup-msha.png)

3. В SSMS в меню "Справка" выберите пункт **Добавление и удаление содержимого справки**.

   ![Пункт меню "Добавление и удаление содержимого справки" в окне справки](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   В окне справки откроется вкладка "Управление содержимым".

4. Чтобы установить новейшее содержимое справки, выберите **Диск** в разделе источника установки, а затем — многоточие (...).

   ![Выбор диска в качестве источника на вкладке "Управление содержимым" в окне справки](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > На вкладке "Управление содержимым" в поле Local store path (Путь к локальному хранилищу) отображается расположение содержимого на локальном компьютере. Чтобы изменить расположение, щелкните **Переместить**, в поле **Куда** введите путь к другой папке, а затем нажмите кнопку **OK**.
   Если не удается установить справку после изменения пути к локальному хранилищу, закройте и снова откройте средство просмотра справки. Убедитесь, что новое расположение есть в пути к локальному хранилищу, а затем повторите попытку установки.

5. Откройте папку, в которую вы распаковали содержимое. Выберите в папке файл **HelpContentSetup.msha**, а затем щелкните **Открыть**.

   ![Открытие файла HelpContentSetup.msha SQL Server 2012](../sql-server/media/sql-server-offline-documentation/sql-2012-open-msha.png)

6. В строке поиска введите *sql server 2012*. Получив доступ к содержимому 2012, выберите **Добавить** рядом с каждым пакетом содержимого (документацией), который необходимо установить в окне справки, а затем выберите **Обновить**.

   ![Поиск документации SQL Server 2012 в окне справки](../sql-server/media/sql-server-offline-documentation/sql-2012-search.png)

   ![Добавление и обновление документации SQL Server 2012 в окне справки](../sql-server/media/sql-server-offline-documentation/sql-2012-add-update.png)

    > [!NOTE]
    > Если окно справки перестает отвечать на запросы во время добавления содержимого, измените значение в строке Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" в файле %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings или HlpViewer_VisualStudiox_en-US.settings на дату в будущем. Сведения об этой проблеме см. в разделе [Окно справки Visual Studio зависает](/visualstudio/welcome-to-visual-studio).

7. Проверить загрузку содержимого SQL Server 2012 можно, выполнив поиск в области содержимого слева, введя *sql server 2012*.

   ![Автоматическое обновление документации SQL Server 2012](../sql-server/media/sql-server-offline-documentation/sql-2012-content.png)

## <a name="view-offline-documentation"></a>Просмотр автономной документации

Содержимое справки SQL Server можно просмотреть с помощью меню **СПРАВКА** в последней версии [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

### <a name="view-offline-help-content-in-ssms"></a>Просмотр автономного содержимого справки в SSMS

Чтобы просмотреть установленную справку в SSMS, выберите в меню справки пункт **Запустить в средстве просмотра справки**, чтобы открыть окно справки.

   ![Пункт меню "Запустить в средстве просмотра справки"](../sql-server/media/sql-server-offline-documentation/helpviewer-view-offline.png)  

В окне справки откроется вкладка "Управление содержимым" с таблицей содержимого установленной справки в левой области. Выберите статьи в таблице содержимого, чтобы просмотреть их в правой области.

> [!Important]
> Если область содержимого не отображается, щелкните "Содержимое" в левом поле. Чтобы область содержимого не закрывалась, щелкните значок канцелярской кнопки.  

   ![Окно справки с содержимым](../sql-server/media/sql-server-offline-documentation/view-offline-all.png)

## <a name="life-cycle-policy"></a>Политика жизненного цикла

Сведения о поддержке определенных продуктов, служб и технологий см. на следующей странице:

- [Политика жизненного цикла поддержки Майкрософт](https://support.microsoft.com/lifecycle/selectindex)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об архивированном содержимом и окне справки см. на приведенных ниже страницах.

- [Электронная документация SQL Server](../sql-server/index.yml?view=sql-server-2016&preserve-view=true)
- [Электронная документация SQL Server 2014](/previous-versions/sql/2014)
- [Предыдущие версии электронной документации по SQL Server](previous-versions-sql-server.md)
- [Система управления версиями в документации по SQL](../sql-server/versioning-system-monikers-ui-sql-server.md?view=sql-server-2016&preserve-view=true)