---
title: Скачивание и установка Azure Data Studio
description: Скачайте и установите Azure Data Studio для Windows, macOS или Linux. В этой статье приведены даты выпуска, номера версий, требования к системе и ссылки для загрузки.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 10/20/2020
ms.openlocfilehash: b3363e9b5b8872d3a78d7c5c0fa7f70a80c8d6f9
ms.sourcegitcommit: ae474d21db4f724523e419622ce79f611e956a22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2020
ms.locfileid: "92257373"
---
# <a name="download-and-install-azure-data-studio"></a>Скачивание и установка Azure Data Studio

Azure Data Studio выполняется в Windows, macOS и Linux.

Скачайте и установите последний выпуск:

> [!NOTE]
> При обновлении с SQL Operations Studio и необходимости сохранения параметров, сочетаний клавиш или фрагментов кода см. раздел [Перемещение параметров пользователя](#move-user-settings).

|Платформа|Скачивание|Дата выпуска| Версия |
|--------|--------|------------|---------|
| Windows | [Пользовательский установщик (рекомендуется)](https://go.microsoft.com/fwlink/?linkid=2145989)<br>[Системный установщик](https://go.microsoft.com/fwlink/?linkid=2145990)<br>[ZIP](https://go.microsoft.com/fwlink/?linkid=2145864) | 14 октября 2020 г. | 1.23.0 |
| macOS | [ZIP](https://go.microsoft.com/fwlink/?linkid=2145865) | 14 октября 2020 г. | 1.23.0 |
| Linux | [.deb](https://go.microsoft.com/fwlink/?linkid=2146016)<br>[.rpm](https://go.microsoft.com/fwlink/?linkid=2146015)<br>[.tar.gz](https://go.microsoft.com/fwlink/?linkid=2145866) | 14 октября 2020 г. | 1.23.0 |

Подробнее см. в [заметках о выпуске](./release-notes-azure-data-studio.md).

## <a name="get-azure-data-studio-for-windows"></a>Получение Azure Data Studio для Windows

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

Этот выпуск Azure Data Studio включает стандартные средства установщика Windows и ZIP-файл.

Рекомендуем *пользовательский установщик*, так как он не требует прав администратора, что упрощает установку и обновление. Пользовательскому установщику не нужны права администратора, так как он размещается в пользовательской локальной папке AppData (LOCALAPPDATA). Пользовательский установщик также обеспечивает более гладкую фоновую работу по обновлению. Дополнительные сведения см. в статье [Пользовательская настройка в Windows](https://code.visualstudio.com/updates/v1_26#_user-setup-for-windows).

**Пользовательский установщик** (рекомендуется)

1. Скачайте и запустите [*пользовательский* установщик Azure Data Studio для Windows](https://go.microsoft.com/fwlink/?linkid=2145989).
2. Запустите приложение Azure Data Studio.

**Системный установщик**

1. Скачайте и запустите [*системный* установщик Azure Data Studio для Windows](https://go.microsoft.com/fwlink/?linkid=2145990).
2. Запустите приложение Azure Data Studio.

**ZIP-файл**

1. Скачайте [ZIP-файл Azure Data Studio для Windows](https://go.microsoft.com/fwlink/?linkid=2145864).
2. Перейдите к скачанному файлу и извлеките его содержимое.
3. Выполнить `\azuredatastudio-windows\azuredatastudio.exe`

## <a name="get-azure-data-studio-for-macos"></a>Получение Azure Data Studio для macOS

1. Скачайте[Azure Data Studio для macOS](https://go.microsoft.com/fwlink/?linkid=2145865).
2. Чтобы извлечь содержимое ZIP-файла, дважды щелкните его.
3. Чтобы сделать Azure Data Studio доступным на *панели запуска*, перетащите *Azure Data Studio.app* в папку *Приложения*.

## <a name="get-azure-data-studio-for-linux"></a>Получение Azure Data Studio для Linux

1. Скачайте Azure Data Studio для Linux с помощью одного из установщиков или архива tar.gz:
    - [.deb](https://go.microsoft.com/fwlink/?linkid=2146016)
    - [.rpm](https://go.microsoft.com/fwlink/?linkid=2146015)
    - [.tar.gz](https://go.microsoft.com/fwlink/?linkid=2145866)
1. Чтобы извлечь файл и запустить Azure Data Studio, откройте новое окно Терминала и введите следующие команды:

   **Установка в Debian:**

   ```bash
   cd ~
   sudo dpkg -i ./Downloads/azuredatastudio-linux-<version string>.deb

   azuredatastudio
   ```

   **Установка из RPM:**

   ```bash
   cd ~
   yum install ./Downloads/azuredatastudio-linux-<version string>.rpm

   azuredatastudio
   ```

   **Установка из tar.gz:**

   ```bash
   cd ~
   cp ~/Downloads/azuredatastudio-linux-<version string>.tar.gz ~ 
   tar -xvf ~/azuredatastudio-linux-<version string>.tar.gz 
   echo 'export PATH="$PATH:~/azuredatastudio-linux-x64"' >> ~/.bashrc
   source ~/.bashrc
   azuredatastudio
   ```

   > [!NOTE]
   > В Debian, Redhat и Ubuntu, возможно, будут отсутствовать некоторые зависимости. Чтобы установить эти зависимости с учетом вашей версии Linux, используйте следующие команды:

   **Debian:**

   ```bash
   sudo apt-get install libunwind8
   ```

   **Redhat:**

   ```bash
   yum install libXScrnSaver
   ```

   **Ubuntu:**

   ```bash
   sudo apt-get install libxss1

   sudo apt-get install libgconf-2-4

   sudo apt-get install libunwind8
   ```

## <a name="download-insiders-build-of-azure-data-studio"></a>Скачивание сборки Azure Data Studio для инсайдеров

Как правило, пользователям следует скачивать стабильный выпуск Azure Data Studio выше. Однако если вы хотите испытать бета-версии функций и отправить свой отзыв, вы можете скачать [сборку Azure Data Studio для предварительной оценки](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-main).

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

Azure Data Studio выполняется в Windows, macOS и Linux, а также поддерживается на следующих платформах:

### <a name="windows"></a>Windows

- Windows 10 (64-разрядная)
- Windows 8.1 (64-разрядная)
- Windows 8 (64-разрядная)
- Windows 7 с пакетом обновления 1 (SP1)
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2 (64-разрядная версия)
- Windows Server 2012 (64-разрядная версия)
- Windows Server 2008 R2 (64-разрядная версия)

### <a name="macos"></a>macOS

- macOS 10.15 Catalina
- macOS 10.14 Mojave
- macOS 10.13 High Sierra
- macOS 10.12 Sierra

### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.3
- SUSE Linux Enterprise Server версии 12 с пакетом обновления 2 (SP2)
- Ubuntu 16.04

## <a name="recommended-system-requirements"></a>Рекомендованные требования к системе

| Рекомендуемые/минимальные | Ядра ЦП | Память (ОЗУ) |
|---------------------|-----------|------------|
| Рекомендуемая         |     4     |   8 ГБ     |
|   Минимальные           |     2     |   4 ГБ     |

## <a name="check-for-updates"></a>Проверка обновлений

Чтобы проверить наличие обновлений, щелкните значок шестеренки в левом нижнем углу окна и выберите **Проверка обновлений**.

Чтобы применить обновления в автономной среде, вы можете [установить последнюю версию](#download-and-install-azure-data-studio) непосредственно поверх установленной ранее. Удалять предыдущие версии Azure Data Studio не требуется. Установщик обновит текущее установленное приложение при его наличии.

## <a name="supported-sql-offerings"></a>Поддерживаемые предложения SQL

- Эта версия Azure Data Studio работает со всеми [поддерживаемыми версиями SQL Server 2014 — [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]](https://support.microsoft.com/lifecycle?C2=1044) и обеспечивает поддержку новейших облачных функций базы данных SQL Azure и Azure Synapse Analytics. Azure Data Studio также предоставляет предварительную поддержку для управляемых экземпляров SQL Azure.

## <a name="move-user-settings"></a>Перемещение параметров пользователя

Если вы хотите переместить пользовательские параметры, сочетания клавиш или фрагменты кода, выполните следующие действия. Это важно при переходе на Azure Data Studio с версии SQL Operations Studio.

*Если у вас уже есть Azure Data Studio или вы никогда не устанавливали или не настраивали SQL Operations Studio, этот раздел можно пропустить*.

1. Откройте параметры, щелкнув значок шестеренки в левом нижнем углу и выбрав **Параметры**.

   ![Измените параметры в Azure Data Studio](./media/download/open-settings.png)

2. Щелкните правой кнопкой мыши вкладку **Параметры пользователя** вверху и выберите пункт **Отобразить в проводнике**.

   ![Запустите обозреватель для перехода к локальной файловой системе](./media/download/reveal-in-explorer.png)

3. Скопируйте все файлы в этой папке и сохраните их в удобном для поиска расположении на локальном диске, например в папке "Документы".

   ![Скопируйте нужные файлы в свое расположение](./media/download/copy-settings.png)

4. В новой версии Azure Data Studio выполните шаги 1–2, а затем на шаге 3 вставьте сохраненное содержимое в папку. Вы также можете вручную скопировать параметры, сочетания клавиш или фрагменты в соответствующие расположения.

5. При замене существующей установки удалите старый каталог установки перед установкой, чтобы избежать ошибок при подключении к учетной записи Azure для обозревателя ресурсов.

## <a name="unattended-install-for-windows"></a>Автоматическая установка для Windows

Вы также можете установить Azure Data Studio с помощью скрипта командной строки.

Чтобы установить Azure Data Studio на платформе Windows в фоновом режиме без каких-либо запросов графического пользовательского интерфейса, выполните действия ниже.

1. Откройте командную строку с повышенными привилегиями.

2. Введите в командной строке следующую команду.

    ```console
    <path where the azuredatastudio-windows-user-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    ```

    Пример

    ```console
    %systemdrive%\azuredatastudio-windows-user-setup-1.23.0.exe /VERYSILENT /MERGETASKS=!runcode
    ```

    > [!Note]
    > Пример также работает с файлом системного установщика.
    > 
    > ```console
    > <path where the azuredatastudio-windows-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    > ```

    Вы также можете передать */SILENT* вместо */VERYSILENT*, чтобы увидеть пользовательский интерфейс установки.

3. Если все сработает правильно, вы увидите установленную среду Azure Data Studio.

## <a name="uninstall-azure-data-studio"></a>Удаление Azure Data Studio

Если вы установили Azure Data Studio с помощью установщика Windows, удаление выполняется так же, как для любого приложения Windows.

Если вы установили Azure Data Studio с помощью ZIP-файла или другого архива, то файлы нужно удалить.

## <a name="next-steps"></a>Next Steps

Чтобы приступить к работе, ознакомьтесь со следующими краткими руководствами.

- [Подключение и отправка запроса к SQL Server](quickstart-sql-server.md)
- [Подключение и отправка запроса к базе данных SQL Azure](quickstart-sql-database.md)
- [Подключение и отправка запроса к хранилищу данных Azure](quickstart-sql-dw.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

[Заявление о конфиденциальности Майкрософт](https://go.microsoft.com/fwlink/?LinkId=521839) и [Сбор данных об использовании](usage-data-collection.md).
