---
title: Установка .NET Core на Ubuntu 18.04 — диспетчер пакетов — .NET Core
description: Используйте диспетчер пакетов для установки пакета SDK для .NET Core и среды выполнения на Ubuntu 18.04.
author: thraka
ms.author: adegeo
ms.date: 12/04/2019
ms.openlocfilehash: 38568b31bf9cd02a19a15addfe4926e8857a87b5
ms.sourcegitcommit: 42ed59871db1f29a32b3d8e7abeb20e6eceeda7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2019
ms.locfileid: "74999047"
---
# <a name="ubuntu-1804-package-manager---install-net-core"></a><span data-ttu-id="0e0f9-103">Диспетчер пакетов Ubuntu 18.04 — установка .NET Core</span><span class="sxs-lookup"><span data-stu-id="0e0f9-103">Ubuntu 18.04 Package Manager - Install .NET Core</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

<span data-ttu-id="0e0f9-104">Эта статья описывает, как использовать диспетчер пакетов для установки .NET Core на Ubuntu 18.04.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-104">This article describes how to use a package manager to install .NET Core on Ubuntu 18.04.</span></span> <span data-ttu-id="0e0f9-105">Если вы устанавливаете среду выполнения, мы рекомендуем установить [среду выполнения ASP.NET Core](#install-the-aspnet-core-runtime), так как она включает в себя среды выполнения .NET Core и ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-105">If you're installing the runtime, we suggest you install the [ASP.NET Core runtime](#install-the-aspnet-core-runtime), as it includes both .NET Core and ASP.NET Core runtimes.</span></span>

## <a name="register-microsoft-key-and-feed"></a><span data-ttu-id="0e0f9-106">Регистрация ключа Майкрософт и веб-канала</span><span class="sxs-lookup"><span data-stu-id="0e0f9-106">Register Microsoft key and feed</span></span>

<span data-ttu-id="0e0f9-107">Перед установкой .NET нужно сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="0e0f9-107">Before installing .NET, you'll need to:</span></span>

- <span data-ttu-id="0e0f9-108">зарегистрировать ключ Майкрософт;</span><span class="sxs-lookup"><span data-stu-id="0e0f9-108">Register the Microsoft key</span></span>
- <span data-ttu-id="0e0f9-109">зарегистрировать репозиторий продуктов;</span><span class="sxs-lookup"><span data-stu-id="0e0f9-109">register the product repository</span></span>
- <span data-ttu-id="0e0f9-110">установить необходимые зависимости.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-110">Install required dependencies</span></span>

<span data-ttu-id="0e0f9-111">Данную операцию достаточно выполнить один раз для каждого компьютера.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-111">This only needs to be done once per machine.</span></span>

<span data-ttu-id="0e0f9-112">Откройте терминал и выполните приведенные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-112">Open a terminal and run the following commands.</span></span>

```bash
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-core-sdk"></a><span data-ttu-id="0e0f9-113">Установка пакета SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="0e0f9-113">Install the .NET Core SDK</span></span>

<span data-ttu-id="0e0f9-114">Обновите продукты, доступные для установки, а затем установите пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-114">Update the products available for installation, then install the .NET Core SDK.</span></span> <span data-ttu-id="0e0f9-115">В терминале выполните приведенные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-115">In your terminal, run the following commands.</span></span>

```bash
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-3.1
```

> [!IMPORTANT]
> <span data-ttu-id="0e0f9-116">Если появляется сообщение об ошибке, похожее на **Unable to locate package dotnet-sdk-3.1** (Не удалось найти пакет dotnet-sdk-3.1), см. раздел [Устранение неполадок диспетчера пакетов](#troubleshoot-the-package-manager).</span><span class="sxs-lookup"><span data-stu-id="0e0f9-116">If you receive an error message similar to **Unable to locate package dotnet-sdk-3.1**, see the [Troubleshoot the package manager](#troubleshoot-the-package-manager) section.</span></span>

## <a name="install-the-aspnet-core-runtime"></a><span data-ttu-id="0e0f9-117">Установка среды выполнения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e0f9-117">Install the ASP.NET Core runtime</span></span>

<span data-ttu-id="0e0f9-118">Обновите продукты, доступные для установки, а затем установите среду выполнения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-118">Update the products available for installation, then install the ASP.NET Core runtime.</span></span> <span data-ttu-id="0e0f9-119">В терминале выполните приведенные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-119">In your terminal, run the following commands.</span></span>

```bash
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install aspnetcore-runtime-3.1
```

> [!IMPORTANT]
> <span data-ttu-id="0e0f9-120">Если появляется сообщение об ошибке, похожее на **Unable to locate package aspnetcore-runtime-3.1** (Не удалось найти пакет aspnetcore-runtime-3.1), см. раздел [Устранение неполадок диспетчера пакетов](#troubleshoot-the-package-manager).</span><span class="sxs-lookup"><span data-stu-id="0e0f9-120">If you receive an error message similar to **Unable to locate package aspnetcore-runtime-3.1**, see the [Troubleshoot the package manager](#troubleshoot-the-package-manager) section.</span></span>

## <a name="install-the-net-core-runtime"></a><span data-ttu-id="0e0f9-121">Установка среды выполнения .NET Core</span><span class="sxs-lookup"><span data-stu-id="0e0f9-121">Install the .NET Core runtime</span></span>

<span data-ttu-id="0e0f9-122">Обновите продукты, доступные для установки, а затем установите среду выполнения .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-122">Update the products available for installation, then install the .NET Core runtime.</span></span> <span data-ttu-id="0e0f9-123">В терминале выполните приведенные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-123">In your terminal, run the following commands.</span></span>

```bash
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-runtime-3.1
```

> [!IMPORTANT]
> <span data-ttu-id="0e0f9-124">Если появляется сообщение об ошибке, похожее на **Unable to locate package dotnet-runtime-3.1** (Не удалось найти пакет dotnet-runtime-3.1), см. раздел [Устранение неполадок диспетчера пакетов](#troubleshoot-the-package-manager).</span><span class="sxs-lookup"><span data-stu-id="0e0f9-124">If you receive an error message similar to **Unable to locate package dotnet-runtime-3.1**, see the [Troubleshoot the package manager](#troubleshoot-the-package-manager) section.</span></span>

## <a name="how-to-install-other-versions"></a><span data-ttu-id="0e0f9-125">Установка других версий</span><span class="sxs-lookup"><span data-stu-id="0e0f9-125">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a><span data-ttu-id="0e0f9-126">Устранение неполадок диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="0e0f9-126">Troubleshoot the package manager</span></span>

<span data-ttu-id="0e0f9-127">Если появляется сообщение об ошибке, похожее на **Unable to locate package {the .NET Core package}** (Не удалось найти пакет {пакет .NET Core}), выполните проведенные ниже команды.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-127">If you receive an error message similar to **Unable to locate package {the .NET Core package}**, run the following commands.</span></span>

```bash
sudo dpkg --purge packages-microsoft-prod && sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install {the .NET Core package}
```

<span data-ttu-id="0e0f9-128">Если проблема не решена, можно выполнить установку вручную с помощью приведенных ниже команд.</span><span class="sxs-lookup"><span data-stu-id="0e0f9-128">If that doesn't work, you can run a manual install with the following commands.</span></span>

```bash
sudo apt-get install -y gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget -q https://packages.microsoft.com/config/ubuntu/18.04/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
sudo apt-get install -y apt-transport-https
sudo apt-get update
sudo apt-get install {the .NET Core package}
```