---
title: Метод ISOSDacInterface::GetModuleData
ms.date: 02/01/2019
api.name:
- ISOSDacInterface::GetModuleData Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- ISOSDacInterface::GetModuleData Method
helpviewer.keywords:
- ISOSDacInterface::GetModuleData Method [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: 80b15f076dfe7a7bbbe7e28d9d68f01255e47202
ms.sourcegitcommit: 3500c4845f96a91a438a02ef2c6b4eef45a5e2af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2019
ms.locfileid: "55828643"
---
# <a name="isosdacinterfacegetmoduledata-method"></a><span data-ttu-id="9ac1f-102">Метод ISOSDacInterface::GetModuleData</span><span class="sxs-lookup"><span data-stu-id="9ac1f-102">ISOSDacInterface::GetModuleData Method</span></span>

<span data-ttu-id="9ac1f-103">Извлекает данные, соответствующие модуль загружен по указанному адресу.</span><span class="sxs-lookup"><span data-stu-id="9ac1f-103">Fetches the data corresponding to the module loaded at a given address.</span></span>

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a><span data-ttu-id="9ac1f-104">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="9ac1f-104">Syntax</span></span>

```
HRESULT GetModuleData(
    CLRDATA_ADDRESS moduleAddr,
    DacpModuleData *data
);
```

### <a name="parameters"></a><span data-ttu-id="9ac1f-105">Параметры</span><span class="sxs-lookup"><span data-stu-id="9ac1f-105">Parameters</span></span>

<span data-ttu-id="9ac1f-106">`moduleAddr` [in] Адрес модуля для получения сведений для.</span><span class="sxs-lookup"><span data-stu-id="9ac1f-106">`moduleAddr` [in] The address of the module to retrieve information for.</span></span>

<span data-ttu-id="9ac1f-107">`data` [out] [DacpModuleData структуры](dacpmoduledata-structure.md) для хранения информации о загруженного модуля.</span><span class="sxs-lookup"><span data-stu-id="9ac1f-107">`data` [out] The [DacpModuleData structure](dacpmoduledata-structure.md) to hold the information of the loaded module.</span></span>


## <a name="remarks"></a><span data-ttu-id="9ac1f-108">Примечания</span><span class="sxs-lookup"><span data-stu-id="9ac1f-108">Remarks</span></span>

<span data-ttu-id="9ac1f-109">Указанный метод является частью `ISOSDacInterface` интерфейса и соответствует 13-й слот в таблице виртуального метода.</span><span class="sxs-lookup"><span data-stu-id="9ac1f-109">The provided method is part of the `ISOSDacInterface` interface and corresponds to the 13th slot of the virtual method table.</span></span>

## <a name="requirements"></a><span data-ttu-id="9ac1f-110">Требования</span><span class="sxs-lookup"><span data-stu-id="9ac1f-110">Requirements</span></span>

<span data-ttu-id="9ac1f-111">**Платформы:** См. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="9ac1f-111">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
<span data-ttu-id="9ac1f-112">**Заголовок.** Нет</span><span class="sxs-lookup"><span data-stu-id="9ac1f-112">**Header:** None</span></span>  
<span data-ttu-id="9ac1f-113">**Библиотека:** Нет</span><span class="sxs-lookup"><span data-stu-id="9ac1f-113">**Library:** None</span></span>  
<span data-ttu-id="9ac1f-114">**Версии платформы .NET Framework:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span><span class="sxs-lookup"><span data-stu-id="9ac1f-114">**.NET Framework Versions:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="9ac1f-115">См. также</span><span class="sxs-lookup"><span data-stu-id="9ac1f-115">See also</span></span>

- [<span data-ttu-id="9ac1f-116">Отладка</span><span class="sxs-lookup"><span data-stu-id="9ac1f-116">Debugging</span></span>](../../../../docs/framework/unmanaged-api/debugging/index.md)
- [<span data-ttu-id="9ac1f-117">Интерфейс ISOSDacInterface</span><span class="sxs-lookup"><span data-stu-id="9ac1f-117">ISOSDacInterface Interface</span></span>](../../../../docs/framework/unmanaged-api/debugging/isosdacinterface-interface.md)