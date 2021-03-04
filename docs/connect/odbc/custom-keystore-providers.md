---
title: Пользовательские поставщики хранилища ключей
description: Сведения о реализации пользовательского поставщика хранилища ключей для использования с драйвером ODBC для SQL Server и функцией Always Encrypted.
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a6166d7d-ef34-4f87-bd1b-838d3ca59ae7
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: f1f075c7ea2e80ec1760012c33ed9f719837c036
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837435"
---
# <a name="custom-keystore-providers"></a>Пользовательские поставщики хранилища ключей
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Обзор

Функция шифрования столбцов SQL Server 2016 требует, чтобы ключи шифрования зашифрованных столбцов (ECEK), хранящиеся на сервере, были получены клиентом и затем расшифрованы в ключи шифрования столбцов (CEK) для доступа к данным, хранящимся в зашифрованных столбцах. Ключи шифрования зашифрованных столбцов шифруются главными ключами столбцов, а безопасность главного ключа важна для обеспечения безопасности шифрования столбцов. Таким образом, главный ключ шифрования должен храниться в безопасном месте; целью поставщика ключей шифрования столбца является предоставление интерфейса, позволяющего драйверу ODBC получать доступ к этим главным ключам шифрования, которые хранятся в безопасном месте. Для пользователей с собственным безопасным хранилищем пользовательский интерфейс поставщика хранилища ключей предоставляет платформу для доступа к защищенному хранилищу главного ключа шифрования для драйвера ODBC, который затем можно использовать для шифрования и расшифровки ключа шифрования столбцов.

Каждый поставщик хранилища ключей содержит один или несколько главных ключей шифрования и управляет ими. Они идентифицируются по пути к ключам — строкам формата, определенного поставщиком. Этот главный ключ шифрования, а также алгоритм шифрования и строку, определенную поставщиком, можно использовать для выполнения шифрования CEK и расшифровки ECEK. Алгоритм, а также ECEK и имя поставщика хранятся в метаданных шифрования базы данных. Дополнительные сведения см. в статьях [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) и [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md). Таким образом, двумя фундаментальными операциями управления ключами являются:

```
CEK = DecryptViaCEKeystoreProvider(CEKeystoreProvider_name, Key_path, Key_algorithm, ECEK)

-and-

ECEK = EncryptViaCEKeystoreProvider(CEKeyStoreProvider_name, Key_path, Key_algorithm, CEK)
```

где `CEKeystoreProvider_name` используется для поиска конкретного поставщика ключей шифрования столбцов (CEKeystoreProvider), а другие аргументы используются CEKeystoreProvider для шифрования и расшифровки (E) CEK. Имя и путь ключа предоставляются метаданными главного ключа шифрования, в то время как алгоритм и значение ключа шифрования зашифрованных столбцов предоставляются метаданными ключа шифрования столбцов. Рядом со встроенными поставщиками по умолчанию могут присутствовать несколько поставщиков хранилища ключей. При выполнении операции, требующей ключ шифрования столбцов, драйвер использует метаданные главного ключа шифрования для поиска соответствующего поставщика хранилища ключей по имени и выполняет операцию расшифровки, которая может быть выражена следующим образом:

```
CEK = CEKeyStoreProvider_specific_decrypt(Key_path, Key_algorithm, ECEK)
```

Хотя для реализации таких операций, как создание и обращение главного ключа шифрования, драйверу не требуется шифровать ключ шифрования столбцов, может потребоваться средство управления ключами. Для выполнения этих действий требуется выполнить обратную операцию:

```
ECEK = CEKeyStoreProvider_specific_encrypt(Key_path, Key_algorithm, CEK)
```

### <a name="cekeystoreprovider-interface"></a>Интерфейс CEKeyStoreProvider

В этом документе подробно описывается интерфейс CEKeyStoreProvider. Поставщик хранилища ключей, реализующий этот интерфейс, может использоваться драйвером Microsoft ODBC Driver for SQL Server. Разработчики CEKeyStoreProvider могут воспользоваться этим руководством для разработки пользовательских поставщиков хранилища ключей, пригодных для использования драйвером.

Библиотека поставщика хранилища ключей ("библиотека поставщика") — это библиотека динамической компоновки, которая может быть загружена драйвером ODBC и содержит один или несколько поставщиков хранилища ключей. Символ `CEKeystoreProvider` должен быть экспортирован библиотекой поставщика и являться адресом массива указателей (который не оканчивается символом NULL) на `CEKeystoreProvider` структуры, по одному для каждого поставщика хранилища ключей внутри библиотеки.

Структура `CEKeystoreProvider` определяет точки входа отдельного провайдера хранилища ключей:

```
typedef struct CEKeystoreProvider {
    wchar_t *Name;
    int (*Init)(CEKEYSTORECONTEXT *ctx, errFunc *onError);
    int (*Read)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int *len);
    int (*Write)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len);
    int (*DecryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *ecek,
                        unsigned short ecekLen,
                        unsigned char **cekOut,
                        unsigned short *cekLen);
    int (*EncryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *cek,
                        unsigned short cekLen,
                        unsigned char **ecekOut,
                        unsigned short *ecekLen);
    void (*Free)();
} CEKEYSTOREPROVIDER;
```

|Имя поля|Описание|
|:--|:--|
|`Name`|Имя поставщика хранилища ключей. Он не должен совпадать с любым другим поставщиком хранилища, ранее загруженным драйвером или представленным в этой библиотеке. Строка расширенных символов*, заканчивающаяся нулем.|
|`Init`|Функция инициализации. Если функция инициализации не является обязательной, это поле может иметь значение null.|
|`Read`|Функция read поставщика. Может иметь значение null, если это не требуется.|
|`Write`|Функция write поставщика. Требуется, если Read имеет значение, отличное от null. Может иметь значение null, если это не требуется.|
|`DecryptCEK`|Функция расшифровки ключа шифрования зашифрованных столбцов. Эта функция является причиной существования поставщика хранилища ключей и не должна иметь значение null.|
|`EncryptCEK`|Функция шифрования ключа шифрования столбца. Драйвер не вызывает эту функцию, но она предоставляется для обеспечения программного доступа к созданию ключа шифрования зашифрованных столбцов с помощью средств управления ключами. Может иметь значение null, если это не требуется.|
|`Free`|Функция завершения. Может иметь значение null, если это не требуется.|

За исключением бесплатной версии, функции в этом интерфейсе имеют пару параметров, **ctx** и **onError**. Первый определяет контекст, в котором вызывается функция, а второй используется для сообщения об ошибках. Дополнительные сведения см. в разделах [Контексты](#context-association) и [Обработка ошибок](#error-handling) ниже.

```
int Init(CEKEYSTORECONTEXT *ctx, errFunc onError);
```
Имя заполнителя для определяемой поставщиком функции инициализации. Драйвер вызывает эту функцию один раз после загрузки поставщика, но до того момента, когда он впервые потребует выполнения расшифровки ключа шифрования столбцов или запросов Read()/Write(). Эта функция используется для выполнения любых задач инициализации.

|Аргумент|Описание|
|:--|:--|
|`ctx`|[Input] Контекст операции.|
|`onError`|[Input] Функция отчетов об ошибках.|
|`Return Value`|Возвращает ненулевое значение для обозначения успеха или нуль для обозначения сбоя.|

```
int Read(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int *len);
```

Имя заполнителя для определяемой поставщиком функции связи. Драйвер вызывает эту функцию, когда приложение запрашивает чтение данных от поставщика (ранее написанного) с помощью атрибута подключения SQL_COPT_SS_CEKEYSTOREDATA, позволяя приложению считывать произвольные данные от поставщика. Дополнительные сведения см. в разделе [Взаимодействие с поставщиками хранилища ключей](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Аргумент|Описание|
|:--|:--|
|`ctx`|[Input] Контекст операции.|
|`onError`|[Input] Функция отчетов об ошибках.|
|`data`|[Output] Указатель на буфер, в котором поставщик записывает данные для чтения приложением. Этот буфер соответствует полю данных структуры CEKEYSTOREDATA.|
|`len`|[InOut] Указатель на значение длины; при вводе данных это максимальная длина буфера данных, и поставщик не должен записывать в него больше длины * байт. После возврата поставщик должен обновить значение *len с учетом количества записанных байтов.|
|`Return Value`|Возвращает ненулевое значение для обозначения успеха или нуль для обозначения сбоя.|

```
int Write(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int len);
```
Имя заполнителя для определяемой поставщиком функции связи. Драйвер вызывает эту функцию, когда приложение запрашивает запись данных в поставщик с помощью атрибута подключения SQL_COPT_SS_CEKEYSTOREDATA, позволяя приложению записать произвольные данные в поставщик. Дополнительные сведения см. в разделе [Взаимодействие с поставщиками хранилища ключей](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Аргумент|Описание|
|:--|:--|
|`ctx`|[Input] Контекст операции.|
|`onError`|[Input] Функция отчетов об ошибках.|
|`data`|[Input] Указатель на буфер, содержащий данные для чтения поставщиком. Этот буфер соответствует полю данных структуры CEKEYSTOREDATA. Поставщик не должен считывать больше, чем длина байт из этого буфера.|
|`len`|[Input] Число байтов, доступных в данных. Это соответствует полю размера данных CEKEYSTOREDATA.|
|`Return Value`|Возвращает ненулевое значение для обозначения успеха или нуль для обозначения сбоя.|

```
int (*DecryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen);
```
Имя заполнителя для определяемой поставщиком функции расшифровки ключа шифрования зашифрованных столбцов. Драйвер вызывает эту функцию для расшифровки ECEK, зашифрованного CMK, связанного с этим поставщиком, в ключ шифрования столбцов.

|Аргумент|Описание|
|:--|:--|
|`ctx`|[Input] Контекст операции.|
|`onError`|[Input] Функция отчетов об ошибках.|
|`keyPath`|[Input] Значение [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) атрибута метаданных для главного ключа шифрования, на который ссылается заданный ключ шифрования зашифрованных столбцов. Строка расширенных символов*, заканчивающаяся нулем. Это значение предназначено для обнаружения главного ключа шифрования, обрабатываемого этим поставщиком.|
|`alg`|[Input] Значение [АЛГОРИТМА](../../t-sql/statements/create-column-encryption-key-transact-sql.md) атрибута метаданных для заданного ключа шифрования зашифрованных столбцов. Строка расширенных символов*, заканчивающаяся нулем. Это значение предназначено для указания алгоритма шифрования, используемого для шифрования заданного ECEK.|
|`ecek`|[Input] Указатель на ECEK, который необходимо расшифровать.|
|`ecekLen`|[Input] Длина ключа шифрования зашифрованных столбцов.|
|`cekOut`|[Output] Поставщик должен выделить память для расшифрованного ECEK и записать его адрес в указатель, на который указывает cekOut. Этот блок памяти должен быть освобожден с помощью функции [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) или бесплатной функции для (Linux/macOS). Если память не была выделена из-за ошибки или по другой причине, поставщик должен установить *cekOut в нулевой указатель.|
|`cekLen`|[Output] Поставщик должен записать длину расшифрованного ключа шифрования зашифрованных столбцов, который был записан в **cekOut на адрес, на который указывает cekLen.|
|`Return Value`|Возвращает ненулевое значение для обозначения успеха или нуль для обозначения сбоя.|

```
int (*EncryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *cek,unsigned short cekLen, unsigned char **ecekOut, unsigned short *ecekLen);
```
Имя заполнителя для определяемой поставщиком функции шифрования ключа шифрования столбцов. Драйвер не вызывает эту функцию и не предоставляет ее функциональность через интерфейс ODBC, но она предоставляется для обеспечения программного доступа к созданию ECEK с помощью средств управления ключами.

|Аргумент|Описание|
|:--|:--|
|`ctx`|[Input] Контекст операции.|
|`onError`|[Input] Функция отчетов об ошибках.|
|`keyPath`|[Input] Значение [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) атрибута метаданных для главного ключа шифрования, на который ссылается заданный ключ шифрования зашифрованных столбцов. Строка расширенных символов*, заканчивающаяся нулем. Предназначено для обнаружения главного ключа шифрования, обрабатываемого этим поставщиком.|
|`alg`|[Input] Значение [АЛГОРИТМА](../../t-sql/statements/create-column-encryption-key-transact-sql.md) атрибута метаданных для заданного ключа шифрования зашифрованных столбцов. Строка расширенных символов*, заканчивающаяся нулем. Это значение предназначено для указания алгоритма шифрования, используемого для шифрования заданного ECEK.|
|`cek`|[Input] Указатель на ключ шифрования столбцов, который необходимо расшифровать.|
|`cekLen`|[Input] Длина ключа шифрования столбцов.|
|`ecekOut`|[Output] Поставщик должен выделить память для зашифрованного ключа шифрования столбцов и записать его адрес в указатель, на который указывает ecekOut. Этот блок памяти должен быть освобожден с помощью функции [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) или бесплатной функции для (Linux/macOS). Если память не была выделена из-за ошибки или по другой причине, поставщик должен установить *ecekOut в нулевой указатель.|
|`ecekLen`|[Output] Поставщик должен записать длину зашифрованного ключа шифрования столбцов, который был записан в **ecekOut на адрес, на который указывает ecekLen.|
|`Return Value`|Возвращает ненулевое значение для обозначения успеха или нуль для обозначения сбоя.|

```
void (*Free)();
```
Имя заполнителя для определяемой поставщиком функции завершения. Драйвер может вызвать эту функцию при нормальном завершении процесса.

> [!NOTE]
> *Строки расширенных символов — это 2-байтовые символы (UTF-16) из-за того, как SQL Server сохраняет их.*


### <a name="error-handling"></a>Обработка ошибок

Так как ошибки могут возникать во время обработки поставщика, предоставляется механизм, позволяющий передавать сообщения об ошибках в драйвер более подробно, чем при логическом выполнении или сбое. Многие функции имеют пару параметров, **ctx** и **onError**, которые используются совместно для этой цели в дополнение к возвращаемому значению логического выполнения или сбоя.

Параметр **ctx** определяет контекст, в котором выполняется операция поставщика.

Параметр **onError** указывает на функцию отчетов об ошибках со следующим прототипом:

`typedef void errFunc(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...);`

|Аргумент|Описание|
|:--|:--|
|`ctx`|[Input] Контекст, в котором сообщается об ошибке.|
|`msg`|[Input] Сообщение об ошибке для отчета. Строка расширенных символов, заканчивающаяся нулем. Чтобы разрешить присутствие параметризованной информации, эта строка может содержать последовательности вставки форматирования формы, принимаемой функцией [FormatMessage](/windows/desktop/api/winbase/nf-winbase-formatmessage). С помощью этого параметра можно указать расширенные функциональные возможности, как описано ниже.|
|...|[Input] Дополнительные измененные параметры для соответствия описателям формата в msg соответствующим образом.|

Чтобы сообщить о том, что произошла ошибка, поставщик вызывает onError, предоставляя параметр контекста, передаваемый в функцию поставщика драйвером, и сообщение об ошибке с дополнительными параметрами, которые необходимо отформатировать. Поставщик может вызывать эту функцию несколько раз для отправки нескольких сообщений об ошибках последовательно в один вызов функции поставщика. Пример:

```
    if (!doSomething(...))
    {
        onError(ctx, L"An error occurred in doSomething.");
        onError(ctx, L"Additional error message with more details.");
        return 0;
    }
```


Параметр `msg` обычно является строкой расширенных символов, но доступны дополнительные расширения:

Используя одно из специальных стандартных значений с макросом IDS_MSG, можно использовать общие сообщения об ошибках и локальную форму в драйвере. Например, если поставщику не удается выделить память, можно использовать сообщение `IDS_S1_001` "Ошибка выделения памяти":

`onError(ctx, IDS_MSG(IDS_S1_001));`

Чтобы ошибка была распознана драйвером, функция поставщика должна возвратить ошибку. Если в контексте операции ODBC происходит сбой, опубликованные ошибки будут доступны для соединения или обработчика инструкций через стандартный механизм диагностики ODBC (`SQLError`, `SQLGetDiagRec` и `SQLGetDiagField`).


### <a name="context-association"></a>Взаимосвязь контекста

Структура `CEKEYSTORECONTEXT`, помимо предоставления контекста для обратного вызова ошибки, может также использоваться для определения контекста ODBC, в котором выполняется операция поставщика. Этот контекст позволяет поставщику связывать данные с каждым из этих контекстов, например для реализации конфигурации для каждого подключения. Для этой цели структура содержит три непрозрачных указателя, соответствующих контексту среды, подключения и инструкции.

```
typedef struct CEKeystoreContext
{
void *envCtx;
void *dbcCtx;
void *stmtCtx;
} CEKEYSTORECONTEXT;
```

|Поле|Описание|
|:--|:--|
|`envCtx`|Контекст среды.|
|`dbcCtx`|Контекст подключения.|
|`stmtCtx`|Контекст инструкции.|

Каждый из этих контекстов является непрозрачным значением, которое, в отличие от соответствующего обработчика ODBC, может использоваться в качестве уникального идентификатора для дескриптора: если дескриптор *X* связан со значением контекста *Y*, то никакие другие дескрипторы среды, соединения или оператора, которые существуют одновременно с *X*, не будут иметь значение контекста *Y* и никакие другие значения контекста не будут связаны с дескриптором *X*. Если выполняемая операция поставщика не имеет определенного контекста обработчика (например, вызовы SQLSetConnectAttr для загрузки и настройки поставщиков, в которых отсутствует обработчик инструкции), соответствующее значение контекста в структуре равно значению null.


## <a name="example"></a>Пример

### <a name="keystore-provider"></a>Поставщик хранилища ключей

Следующий код является примером минимальной реализации поставщика хранилища ключей.

```
/* Custom Keystore Provider Example

Windows:   compile with cl MyKSP.c /LD /MD /link /out:MyKSP.dll
Linux/macOS: compile with gcc -fshort-wchar -fPIC -o MyKSP.so -shared MyKSP.c

 */

#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#endif

#include <stdlib.h>
#include <sqltypes.h>
#include "msodbcsql.h"
#include <sql.h>
#include <sqlext.h>

int __stdcall KeystoreInit(CEKEYSTORECONTEXT *ctx, errFunc *onError) {
    printf("KSP Init() function called\n");
    return 1;
}

static unsigned char *g_encryptKey;
static unsigned int g_encryptKeyLen;

int __stdcall KeystoreWrite(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len) {
    printf("KSP Write() function called (%d bytes)\n", len);
    if (len) {
        if (g_encryptKey)
            free(g_encryptKey);
        g_encryptKey = malloc(len);
        if (!g_encryptKey) {
            onError(ctx, L"Memory Allocation Error");
            return 0;
        }
        memcpy(g_encryptKey, data, len);
        g_encryptKeyLen = len;
    }
    return 1;
}

// Very simple "encryption" scheme - rotating XOR with the key
int __stdcall KeystoreDecrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg,
    unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen) {
    unsigned int i;
    printf("KSP Decrypt() function called (keypath=%S alg=%S ecekLen=%u)\n", keyPath, alg, ecekLen);
    if (wcscmp(keyPath, L"TheOneAndOnlyKey")) {
        onError(ctx, L"Invalid key path");
        return 0;
    }
    if (wcscmp(alg, L"none")) {
        onError(ctx, L"Invalid algorithm");
        return 0;
    }
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
#ifndef _WIN32
    *cekOut = malloc(ecekLen);
#else
    *cekOut = LocalAlloc(LMEM_FIXED, ecekLen);
#endif
    if (!*cekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *cekLen = ecekLen;
    for (i = 0; i < ecekLen; i++)
        (*cekOut)[i] = ecek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

// Note that in the provider interface, this function would be referenced via the CEKEYSTOREPROVIDER
// structure. However, that does not preclude keystore providers from exporting their own functions,
// as illustrated by this example where the encryption is performed via a separate function (with a
// different prototype than the one in the KSP interface.)
#ifdef _WIN32
__declspec(dllexport)
#endif
int KeystoreEncrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError,
    unsigned char *cek, unsigned short cekLen,
    unsigned char **ecekOut, unsigned short *ecekLen) {
    unsigned int i;
    printf("KSP Encrypt() function called (cekLen=%u)\n", cekLen);
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
    *ecekOut = malloc(cekLen);
    if (!*ecekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *ecekLen = cekLen;
    for (i = 0; i < cekLen; i++)
        (*ecekOut)[i] = cek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

CEKEYSTOREPROVIDER MyCustomKSPName_desc = {
    L"MyCustomKSPName",
    KeystoreInit,
    0,
    KeystoreWrite,
    KeystoreDecrypt,
    0
};

#ifdef _WIN32
__declspec(dllexport)
#endif
CEKEYSTOREPROVIDER *CEKeystoreProvider[] = {
    &MyCustomKSPName_desc,
    0
};
```

### <a name="odbc-application"></a>Приложение ODBC

Следующий код является демонстрационным приложением, которое использует указанный выше поставщик хранилища. При запуске убедитесь, что библиотека поставщика находится в том же каталоге, что и двоичный файл приложения, а строка подключения указывает параметр `ColumnEncryption=Enabled` (или указывает имя DSN, содержащее этот параметр).

```
/*
 Example application for demonstration of custom keystore provider usage

Windows:   compile with cl /MD kspapp.c /link odbc32.lib
Linux/macOS: compile with gcc -o kspapp -fshort-wchar kspapp.c -lodbc -ldl
 
 usage: kspapp connstr

 */

#define KSPNAME L"MyCustomKSPName"
#define PROV_ENCRYPT_KEY "JHKCWYT06N3RG98J0MBLG4E3"

#include <stdio.h>
#include <stdlib.h>
#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#include <dlfcn.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include "msodbcsql.h"

/* Convenience functions */

int checkRC(SQLRETURN rc, char *msg, int ret, SQLHANDLE h, SQLSMALLINT ht) {
    if (rc == SQL_ERROR) {
        fprintf(stderr, "Error occurred upon %s\n", msg);
        if (h) {
            SQLSMALLINT i = 0;
            SQLSMALLINT outlen = 0;
            char errmsg[1024];
            while ((rc = SQLGetDiagField(
                ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
                || rc == SQL_SUCCESS_WITH_INFO) {
                fprintf(stderr, "Err#%d: %s\n", i, errmsg);
            }
        }
        if (ret)
            exit(ret);
        return 0;
    }
    else if (rc == SQL_SUCCESS_WITH_INFO && h) {
        SQLSMALLINT i = 0;
        SQLSMALLINT outlen = 0;
        char errmsg[1024];
        printf("Success with info for %s:\n", msg);
        while ((rc = SQLGetDiagField(
            ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
            || rc == SQL_SUCCESS_WITH_INFO) {
            fprintf(stderr, "Msg#%d: %s\n", i, errmsg);
        }
    }
    return 1;
}

void postKspError(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...) {
    if (msg > (wchar_t*)65535)
        wprintf(L"Provider emitted message: %s\n", msg);
    else
        wprintf(L"Provider emitted message ID %d\n", msg);
}

int main(int argc, char **argv) {
    char sqlbuf[1024];
    SQLHENV env;
    SQLHDBC dbc;
    SQLHSTMT stmt;
    SQLRETURN rc;
    unsigned char CEK[32];
    unsigned char *ECEK;
    unsigned short ECEKlen;
#ifdef _WIN32
    HMODULE hProvLib;
#else
    void *hProvLib;
#endif
    CEKEYSTORECONTEXT ctx = {0};
    CEKEYSTOREPROVIDER **ppKsp, *pKsp;
    int(__stdcall *pEncryptCEK)(CEKEYSTORECONTEXT *, errFunc *, unsigned char *, unsigned short, unsigned char **, unsigned short *);
    int i;
    if (argc < 2) {
        fprintf(stderr, "usage: kspapp connstr\n");
        return 1;
    }

    /* Load the provider library */
#ifdef _WIN32
    if (!(hProvLib = LoadLibrary("MyKSP.dll"))) {
#else
    if (!(hProvLib = dlopen("./MyKSP.so", RTLD_NOW))) {
#endif
        fprintf(stderr, "Error loading KSP library\n");
        return 2;
    }
#ifdef _WIN32
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)GetProcAddress(hProvLib, "CEKeystoreProvider"))) {
#else
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)dlsym(hProvLib, "CEKeystoreProvider"))) {
#endif
        fprintf(stderr, "The export CEKeystoreProvider was not found in the KSP library\n");
        return 3;
    }
    while (pKsp = *ppKsp++) {
        if (!memcmp(KSPNAME, pKsp->Name, sizeof(KSPNAME)))
            goto FoundProv;
    }
    fprintf(stderr, "Could not find provider in the library\n");
    return 4;
FoundProv:
    if (pKsp->Init && !pKsp->Init(&ctx, postKspError)) {
        fprintf(stderr, "Could not initialize provider\n");
        return 5;
    }
#ifdef _WIN32
    if (!(pEncryptCEK = (LPVOID)GetProcAddress(hProvLib, "KeystoreEncrypt"))) {
#else
    if (!(pEncryptCEK = dlsym(hProvLib, "KeystoreEncrypt"))) {
#endif
        fprintf(stderr, "The export KeystoreEncrypt was not found in the KSP library\n");
        return 6;
    }
    if (!pKsp->Write) {
        fprintf(stderr, "Provider does not support configuration\n");
        return 7;
    }

    /* Configure the provider with the key */
    if (!pKsp->Write(&ctx, postKspError, PROV_ENCRYPT_KEY, strlen(PROV_ENCRYPT_KEY))) {
        fprintf(stderr, "Error writing to KSP\n");
        return 8;
    }

    /* Generate a CEK and encrypt it with the provider */
    srand(time(0) ^ getpid());
    for (i = 0; i < sizeof(CEK); i++)
        CEK[i] = rand();

    if (!pEncryptCEK(&ctx, postKspError, CEK, sizeof(CEK), &ECEK, &ECEKlen)) {
        fprintf(stderr, "Error encrypting CEK\n");
        return 9;
    }

    /* Connect to Server */
    rc = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &env);
    checkRC(rc, "allocating environment handle", 2, 0, 0);
    rc = SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0);
    checkRC(rc, "setting ODBC version to 3.0", 3, env, SQL_HANDLE_ENV);
    rc = SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc);
    checkRC(rc, "allocating connection handle", 4, env, SQL_HANDLE_ENV);
    rc = SQLDriverConnect(dbc, 0, argv[1], strlen(argv[1]), NULL, 0, NULL, SQL_DRIVER_NOPROMPT);
    checkRC(rc, "connecting to data source", 5, dbc, SQL_HANDLE_DBC);
    rc = SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt);
    checkRC(rc, "allocating statement handle", 6, dbc, SQL_HANDLE_DBC);

    /* Create a CMK definition on the server */
    {
        static char cmkSql[] = "CREATE COLUMN MASTER KEY CustomCMK WITH ("
            "KEY_STORE_PROVIDER_NAME = 'MyCustomKSPName',"
            "KEY_PATH = 'TheOneAndOnlyKey')";
        printf("Create CMK: %s\n", cmkSql);
        SQLExecDirect(stmt, cmkSql, SQL_NTS);
    }

    /* Create a CEK definition on the server */
    {
        const char cekSqlBefore[] = "CREATE COLUMN ENCRYPTION KEY CustomCEK WITH VALUES ("
            "COLUMN_MASTER_KEY = CustomCMK,"
            "ALGORITHM = 'none',"
            "ENCRYPTED_VALUE = 0x";
        char *cekSql = malloc(sizeof(cekSqlBefore) + 2 * ECEKlen + 2); /* 1 for ')', 1 for null terminator */
        strcpy(cekSql, cekSqlBefore);
        for (i = 0; i < ECEKlen; i++)
            sprintf(cekSql + sizeof(cekSqlBefore) - 1 + 2 * i, "%02x", ECEK[i]);
        strcat(cekSql, ")");
        printf("Create CEK: %s\n", cekSql);
        SQLExecDirect(stmt, cekSql, SQL_NTS);
        free(cekSql);
#ifdef _WIN32
        LocalFree(ECEK);
#else
        free(ECEK);
#endif
    }

#ifdef _WIN32
    FreeLibrary(hProvLib);
#else
    dlclose(hProvLib);
#endif

    /* Create a table with encrypted columns */
    {
        static char *tableSql = "CREATE TABLE CustomKSPTestTable ("
            "c1 int,"
            "c2 varchar(255) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = CustomCEK, ENCRYPTION_TYPE = DETERMINISTIC, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'))";
        printf("Create table: %s\n", tableSql);
        SQLExecDirect(stmt, tableSql, SQL_NTS);
    }

    /* Load provider into the ODBC Driver and configure it */
    {
        unsigned char ksd[sizeof(CEKEYSTOREDATA) + sizeof(PROV_ENCRYPT_KEY) - 1];
        CEKEYSTOREDATA *pKsd = (CEKEYSTOREDATA*)ksd;
        pKsd->name = L"MyCustomKSPName";
        pKsd->dataSize = sizeof(PROV_ENCRYPT_KEY) - 1;
        memcpy(pKsd->data, PROV_ENCRYPT_KEY, sizeof(PROV_ENCRYPT_KEY) - 1);
#ifdef _WIN32
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "MyKSP.dll", SQL_NTS);
#else
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "./MyKSP.so", SQL_NTS);
#endif
        checkRC(rc, "Loading KSP into ODBC Driver", 7, dbc, SQL_HANDLE_DBC);
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREDATA, (SQLPOINTER)pKsd, SQL_IS_POINTER);
        checkRC(rc, "Configuring the KSP", 7, dbc, SQL_HANDLE_DBC);
    }

    /* Insert some data */
    {
        int c1;
        char c2[256];
        rc = SQLBindParameter(stmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &c1, 0, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        rc = SQLBindParameter(stmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_VARCHAR, 255, 0, c2, 255, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        for (i = 0; i < 10; i++) {
            c1 = i * 10 + i + 1;
            sprintf(c2, "Sample data %d for column 2", i);
            rc = SQLExecDirect(stmt, "INSERT INTO CustomKSPTestTable (c1, c2) values (?, ?)", SQL_NTS);
            checkRC(rc, "Inserting rows query", 10, stmt, SQL_HANDLE_STMT);
        }
        printf("(Encrypted) data has been inserted into the [CustomKSPTestTable]. You may inspect the data now.\n"
            "Press Enter to continue...\n");
        getchar();
    }

    /* Retrieve the data */
    {
        int c1;
        char c2[256];
        rc = SQLBindCol(stmt, 1, SQL_C_LONG, &c1, 0, 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLBindCol(stmt, 2, SQL_C_CHAR, c2, sizeof(c2), 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLExecDirect(stmt, "SELECT c1, c2 FROM CustomKSPTestTable", SQL_NTS);
        checkRC(rc, "Retrieving rows query", 12, stmt, SQL_HANDLE_STMT);
        while (SQL_SUCCESS == (rc = SQLFetch(stmt)))
            printf("Retrieved data: c1=%d c2=%s\n", c1, c2);
        SQLFreeStmt(stmt, SQL_CLOSE);
        printf("Press Enter to clean up and exit...\n");
        getchar();
    }

    /* Clean up */
    {
        SQLExecDirect(stmt, "DROP TABLE CustomKSPTestTable", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN ENCRYPTION KEY CustomCEK", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN MASTER KEY CustomCMK", SQL_NTS);
        printf("Removed table, CEK, and CMK\n");
    }
    SQLDisconnect(dbc);
    SQLFreeHandle(SQL_HANDLE_DBC, dbc);
    SQLFreeHandle(SQL_HANDLE_ENV, env);
    return 0;
}

```

## <a name="see-also"></a>См. также:

[Использование функции Always Encrypted с драйвером ODBC](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
