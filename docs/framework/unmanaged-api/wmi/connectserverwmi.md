---
title: Функция Коннектсервервми (Справочник по неуправляемым API)
description: Функция Коннектсервервми использует DCOM для создания соединения с пространством имен WMI.
ms.date: 11/06/2017
api_name:
- ConnectServerWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- ConnectServerWmi
helpviewer_keywords:
- ConnectServerWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 25a73060ed242fd0e77042cd0ea9618b9b27250f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128694"
---
# <a name="connectserverwmi-function"></a>Функция Коннектсервервми

Создает подключение через DCOM к пространству имен WMI на указанном компьютере.

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>Синтаксис

```cpp
HRESULT ConnectServerWmi (
   [in] BSTR               strNetworkResource,
   [in] BSTR               strUser,
   [in] BSTR               strPassword,
   [in] BSTR               strLocale,
   [in] long               lSecurityFlags,
   [in] BSTR               strAuthority,
   [in] IWbemContext*      pCtx,
   [out] IWbemServices**   ppNamespace,
   [in] DWORD              impLevel,
   [in] DWORD              authLevel
);
```

## <a name="parameters"></a>Параметры

`strNetworkResource`\
окне Указатель на допустимый `BSTR`, содержащий путь к объекту для правильного пространства имен WMI. Дополнительные сведения см. в разделе ["Примечания"](#remarks) .

`strUser`\
окне Указатель на допустимое `BSTR`, содержащее имя пользователя. Значение `null` указывает текущий контекст безопасности. Если пользователь находится в домене, отличном от домена текущего, `strUser` может также содержать домен и имя пользователя, разделенные обратной косой чертой. `strUser` также может быть в формате имени участника-пользователя (UPN), например `userName@domainName`. Дополнительные сведения см. в разделе ["Примечания"](#remarks) .

`strPassword`\
окне Указатель на допустимое `BSTR`, содержащее пароль. `null` указывает текущий контекст безопасности. Пустая строка ("") указывает действительный пароль нулевой длины.

`strLocale`\
окне Указатель на допустимое `BSTR`, указывающий правильный языковой стандарт для извлечения сведений. Для идентификаторов языкового стандарта Майкрософт формат строки — "MS\_*xxx*", где *xxx* — строка в шестнадцатеричном формате, которая указывает код локали (LCID). Если указан недопустимый языковой стандарт, метод возвращает `WBEM_E_INVALID_PARAMETER`, за исключением Windows 7, где используется языковой стандарт по умолчанию сервера. Если значение равно "NULL1", используется текущий языковой стандарт.

`lSecurityFlags`\
окне Флаги, передаваемые в метод `ConnectServerWmi`. Значение, равное нулю (0) для этого параметра, приводит к вызову `ConnectServerWmi` возврате только после установления соединения с сервером. Это может привести к тому, что приложение не будет реагировать на неопределенное время, если сервер не работает. Другие допустимые значения:

| Константа  | значения  | Описание  |
|---------|---------|---------|
| `CONNECT_REPOSITORY_ONLY` | 0x40 | Зарезервировано для внутреннего использования. Не используется. |
| `WBEM_FLAG_CONNECT_USE_MAX_WAIT` | 0x80 | `ConnectServerWmi` возвращают не более двух минут. |

`strAuthority`\
окне Доменное имя пользователя. Возможны следующие значения:

| значения | Описание |
|---------|---------|
| пустая | Используется проверка подлинности NTLM, и используется домен NTLM текущего пользователя. Если `strUser` указывает домен (рекомендуемое расположение), его не следует указывать здесь. Функция возвращает `WBEM_E_INVALID_PARAMETER`, если домен указан в обоих параметрах. |
| Kerberos:*имя участника* | Используется проверка подлинности Kerberos, и этот параметр содержит имя участника Kerberos. |
| НТЛМДОМАИН:*доменное имя* | Используется проверка подлинности NT LAN Manager, и этот параметр содержит доменное имя NTLM. |

`pCtx`\
окне Как правило, этот параметр имеет `null`. В противном случае он является указателем на объект [ивбемконтекст](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext) , необходимый одному или нескольким динамическим поставщикам классов.

`ppNamespace`\
заполняет При возврате функции получает указатель на объект [IWbemServices](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemservices) , привязанный к указанному пространству имен. Он указывает на `null` при возникновении ошибки.

`impLevel`\
окне Уровень олицетворения.

`authLevel`\
окне Уровень авторизации.

## <a name="return-value"></a>Возвращаемое значение

Следующие значения, возвращаемые этой функцией, определены в файле заголовка *вбемкли. h* , или их можно определить как константы в коде:

|Константа  |значения  |Описание  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | Общий сбой. |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | Недопустимый параметр. |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | Недостаточно памяти для завершения операции. |
| `WBEM_S_NO_ERROR` | 0 | Вызов функции выполнен успешно.  |

## <a name="remarks"></a>Заметки

Эта функция заключает в оболочку вызов метода [ивбемлокатор:: коннектсервер](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemlocator-connectserver) .

Для локального доступа к пространству имен по умолчанию `strNetworkResource` может быть простым путем к объекту: "root\default" или "\\.\рут\дефаулт". Для доступа к пространству имен по умолчанию на удаленном компьютере, использующем COM или совместимую с корпорацией Майкрософт сеть, укажите имя компьютера: "\\мисервер\рут\дефаулт". Имя компьютера также может быть DNS-именем или IP-адресом. Функция `ConnectServerWmi` также может подключаться к компьютерам с IPv6 с помощью IPv6-адреса.

`strUser` не может быть пустой строкой. Если домен указан в `strAuthority`, он также не должен включаться в `strUser`или функция возвращает `WBEM_E_INVALID_PARAMETER`.

## <a name="requirements"></a>Требования

 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).

 **Заголовок:** WMINet_Utils. idl

 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>См. также

- [WMI и счетчики производительности (Справочник по неуправляемым интерфейсам API)](index.md)
