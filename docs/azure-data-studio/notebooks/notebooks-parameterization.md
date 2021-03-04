---
title: Параметризация записных книжек в Azure Data Studio
description: В этом учебнике показано, как создать параметризованную записную книжку в Azure Data Studio.
ms.topic: how-to
ms.prod: azure-data-studio
ms.technology: azure-data-studio
author: vasubhog
ms.author: vabhog
ms.reviewer: mikeray, alayu, maghan
ms.custom: ''
ms.date: 01/25/2021
ms.openlocfilehash: 25e8ea8c4f10ccdb7ee2901dced68f4a66d57000
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836081"
---
# <a name="create-a-parameterized-notebook"></a>Создание параметризованной записной книжки

**Параметризация** — это возможность выполнения одной и той же записной книжки с разными параметрами.

В этой статье показано, как создать и запустить параметризованную записную книжку в Azure Data Studio с помощью ядра Python.

## <a name="prerequisites"></a>Предварительные требования

- [Azure Data Studio](../download-azure-data-studio.md)
- [Python](https://www.python.org/downloads/)

## <a name="install-and-set-up-papermill-in-azure-data-studio"></a>Установка и настройка Papermill в Azure Data Studio

Действия, описанные в этом разделе, выполняются в записной книжке Azure Data Studio.

1. Создайте новую записную книжку и измените **ядро** на *Python 3*.

   ![Новая записная книжка](media/notebooks-kqlmagic/install-new-notebook.png)

2. Вам может быть предложено обновить пакеты Python, когда пакеты нуждаются в обновлении.

   ![Да](media/notebooks-kqlmagic/install-python-yes.png)

3. Установите Papermill:

   ```python
   import sys
   !{sys.executable} -m pip install papermill --no-cache-dir --upgrade
   ```

   Убедитесь, что она установлена:

   ```python
   import sys
   !{sys.executable} -m pip list
   ```

   :::image type="content" source="media/notebooks-parameterization/install-list-papermill.png" alt-text="Список":::

5. Чтобы проверить, правильно ли загружено средство Papermill, можно проверить его версию.

   ```python
   import papermill
   papermill
   ```

   :::image type="content" source="media/notebooks-parameterization/install-validation-papermill.png" alt-text="Проверка":::

## <a name="set-up-a-parameterized-notebook"></a>Настройка параметризованной записной книжки

1. Убедитесь, что для параметра **Ядро** установлено *Python3*.

   ![Изменение ядра](media/notebooks-kqlmagic/change-kernel.png)

2. Создайте ячейку кода и пометьте ее как **ячейку параметров**.

   ```python
   x = 2.0
   y = 5.0
   ```

   :::image type="content" source="media/notebooks-parameterization/make-parameter-cell.png" alt-text="Записная книжка с ячейкой параметров":::

3. Добавьте другие ячейки для тестирования различных параметров.

   ```python
   addition = x + y
   multiply = x * y
   ```

   ```python
   print("Addition: " + str(addition))
   print("Multiplication: " + str(multiply))
   ```

   Ячейки в примере входной записной книжки: :::image type="content" source="media/notebooks-parameterization/test-cells.png" alt-text="Дополнительные ячейки входной записной книжки":::

4. Сохраните записную книжку как **input.ipynb**.
   :::image type="content" source="media/notebooks-parameterization/save-notebook.png" alt-text="Сохранение записной книжки":::

## <a name="how-to-execute-papermill-notebook"></a>Выполнение записной книжки Papermill

Papermill можно выполнять двумя способами:

- Интерфейс командной строки
- API Python

### <a name="parameterized-cli-execution"></a>Параметризованное выполнение через интерфейс командной строки

Чтобы выполнить записную книжку с помощью интерфейса командной строки (CLI), введите в окне терминала команду papermill, указав входную записную книжку, расположение выходной записной книжки и параметры.

> [!Note]
   > Документацию по интерфейсу командной строки Papermill см. [здесь](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-cli).

1. Выполните входную записную книжку с новыми параметрами.

   ```shell
   papermill Input.ipynb Output.ipynb -p x 10 -p y 20
   ```

   В результате входная записная книжка будет выполнена с новыми значениями параметров **x** и **y**.

2. После выполнения просмотрите новую выходную параметризованную записную книжку.
   Обратите внимание на то, что появилась новая ячейка **# Injected-Parameters**, содержащая новые значения параметров, переданные через CLI.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Выходная записная книжка":::

### <a name="parameterized-python-api-execution"></a>Параметризованное выполнение API Python

> [!Note]
   > Документацию по API Python для Papermill см. [здесь](https://papermill.readthedocs.io/en/latest/usage-execute.html#execute-via-the-python-api).

1. Создайте новую записную книжку и измените **ядро** на *Python 3*.
   ![Новая записная книжка](media/notebooks-kqlmagic/install-new-notebook.png)

2. Добавьте новую ячейку кода и выполните метод execute с помощью Papermill.

   ```python
   import papermill as pm

   pm.execute_notebook(
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Input.ipynb',
   '/Users/vasubhog/GitProjects/AzureDataStudio-Notebooks/Demo_Parameterization/Output.ipynb',
   parameters = dict(x = 10, y = 20)
   )
   ```

   ![Выполнение API Python для Papermill](media/notebooks-parameterization/python-api-execute.png)

3. После выполнения просмотрите новую выходную параметризованную записную книжку.

   Обратите внимание на то, что появилась новая ячейка **# Injected-Parameters**, содержащая новые значения параметров, переданные через CLI.

   :::image type="content" source="media/notebooks-parameterization/output-notebook.png" alt-text="Выходная записная книжка":::

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о записных книжках и параметризации:

- [Использование записных книжек в Azure Data Studio](./notebooks-guidance.md)
- [Документация по параметризации в Papermill](https://papermill.readthedocs.io/en/latest/index.html)