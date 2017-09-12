---
title: "Шифрование ключа при хранении"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f2bbbf4e-0945-43ce-be59-8bf19e448798
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 16a9385630d88c4c9f33954f83fce2bbce5be719
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="key-encryption-at-rest"></a><span data-ttu-id="cfe53-103">Шифрование ключа при хранении</span><span class="sxs-lookup"><span data-stu-id="cfe53-103">Key Encryption At Rest</span></span>

<a name=data-protection-implementation-key-encryption-at-rest></a>

<span data-ttu-id="cfe53-104">По умолчанию система защиты данных [использует эвристику](../configuration/default-settings.md#data-protection-default-settings) для определения способа шифрования материал ключа должен быть в зашифрованном виде.</span><span class="sxs-lookup"><span data-stu-id="cfe53-104">By default the data protection system [employs a heuristic](../configuration/default-settings.md#data-protection-default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="cfe53-105">Разработчик можно переопределить эвристика и вручную задать как ключи должны быть в зашифрованном виде.</span><span class="sxs-lookup"><span data-stu-id="cfe53-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="cfe53-106">При указании явную ключа шифрования в механизм rest, система защиты данных будет отменить регистрацию механизм хранилища ключей по умолчанию, предоставленный эвристики.</span><span class="sxs-lookup"><span data-stu-id="cfe53-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="cfe53-107">Вы должны [укажите механизм явного хранилища ключей](key-storage-providers.md#data-protection-implementation-key-storage-providers), в противном случае система защиты данных не смогут запуститься.</span><span class="sxs-lookup"><span data-stu-id="cfe53-107">You must [specify an explicit key storage mechanism](key-storage-providers.md#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name=data-protection-implementation-key-encryption-at-rest-providers></a>

<span data-ttu-id="cfe53-108">Система защиты данных поставляется с тремя механизмы шифрования ключа в поле.</span><span class="sxs-lookup"><span data-stu-id="cfe53-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="cfe53-109">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="cfe53-109">Windows DPAPI</span></span>

<span data-ttu-id="cfe53-110">*Этот механизм доступен только в Windows.*</span><span class="sxs-lookup"><span data-stu-id="cfe53-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="cfe53-111">Если используется Windows DPAPI, материал ключа, будут зашифрованы через [функция CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) перед сохранением в хранилище.</span><span class="sxs-lookup"><span data-stu-id="cfe53-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="cfe53-112">DPAPI — это механизм нужные данные, которые никогда не будет считываться из-за пределами текущего компьютера (хотя можно выполнить эти ключи до Active Directory; см. раздел [перемещаемых профилей и DPAPI](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="cfe53-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it is possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="cfe53-113">Например настроить шифрование ключа статических DPAPI.</span><span class="sxs-lookup"><span data-stu-id="cfe53-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
       // only the local user account can decrypt the keys
       .ProtectKeysWithDpapi();
   ```

<span data-ttu-id="cfe53-114">Если ProtectKeysWithDpapi вызывается без параметров, только текущей учетной записи Windows, может расшифровать материализованный материал ключа.</span><span class="sxs-lookup"><span data-stu-id="cfe53-114">If ProtectKeysWithDpapi is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="cfe53-115">При необходимости можно указать, что любой учетной записи пользователя на компьютере (не только текущей учетной записи пользователя), требуется возможность расшифровать материал ключа, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="cfe53-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
       // all user accounts on the machine can decrypt the keys
       .ProtectKeysWithDpapi(protectToLocalMachine: true);
   ```

## <a name="x509-certificate"></a><span data-ttu-id="cfe53-116">Сертификат X.509</span><span class="sxs-lookup"><span data-stu-id="cfe53-116">X.509 certificate</span></span>

<span data-ttu-id="cfe53-117">*Этот механизм не доступен на `.NET Core 1.0` или `1.1`.*</span><span class="sxs-lookup"><span data-stu-id="cfe53-117">*This mechanism is not available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="cfe53-118">Если приложение распределены между несколькими компьютерами, может быть удобным для распределения общего сертификата X.509 на компьютерах и настройка приложений для использования этого сертификата для шифрования ключей при хранении.</span><span class="sxs-lookup"><span data-stu-id="cfe53-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="cfe53-119">Пример см. ниже.</span><span class="sxs-lookup"><span data-stu-id="cfe53-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
       // searches the cert store for the cert with this thumbprint
       .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
   ```

<span data-ttu-id="cfe53-120">Из-за ограничений платформы .NET Framework, поддерживаются только сертификаты с закрытыми ключами CAPI.</span><span class="sxs-lookup"><span data-stu-id="cfe53-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="cfe53-121">В разделе [шифрования на основе сертификата с Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) ниже возможных способах обхода этих ограничений.</span><span class="sxs-lookup"><span data-stu-id="cfe53-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name=data-protection-implementation-key-encryption-at-rest-dpapi-ng></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="cfe53-122">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="cfe53-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="cfe53-123">*Этот механизм доступен только в Windows 8 и Windows Server 2012 и более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="cfe53-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="cfe53-124">Начиная с Windows 8, операционная система поддерживает DPAPI-NG (также называемые CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="cfe53-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="cfe53-125">Корпорация Майкрософт располагает его сценария использования следующим образом.</span><span class="sxs-lookup"><span data-stu-id="cfe53-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="cfe53-126">Облако, тем не менее, часто требуется содержимого зашифрованные на одном компьютере быть расшифрованы на другом.</span><span class="sxs-lookup"><span data-stu-id="cfe53-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="cfe53-127">Таким образом начиная с Windows 8, Microsoft расширенные идея использования относительно простой интерфейс API для охвата облачных сценариев.</span><span class="sxs-lookup"><span data-stu-id="cfe53-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="cfe53-128">Это новый API с названием DPAPI-NG, позволяет безопасно совместно использовать секретные данные (ключи, пароли, материал ключа) и сообщения, защищая набор субъектов, которые можно использовать для отключения их защиты на разных компьютерах после надлежащую проверку подлинности и авторизации.</span><span class="sxs-lookup"><span data-stu-id="cfe53-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="cfe53-129">Из [о CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="cfe53-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="cfe53-130">Участник кодируются как дескриптор правила защиты.</span><span class="sxs-lookup"><span data-stu-id="cfe53-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="cfe53-131">Рассмотрим ниже примере, который шифрует материал ключа таким образом, что только присоединенных к домену пользователя с указанным SID может расшифровать материал ключа.</span><span class="sxs-lookup"><span data-stu-id="cfe53-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
     // uses the descriptor rule "SID=S-1-5-21-..."
     .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
       flags: DpapiNGProtectionDescriptorFlags.None);
   ```

<span data-ttu-id="cfe53-132">Имеется также перегрузка ProtectKeysWithDpapiNG без параметров.</span><span class="sxs-lookup"><span data-stu-id="cfe53-132">There is also a parameterless overload of ProtectKeysWithDpapiNG.</span></span> <span data-ttu-id="cfe53-133">Это удобный метод для указания правило «SID = Мое», где мои — идентификатор безопасности текущего пользователя Windows.</span><span class="sxs-lookup"><span data-stu-id="cfe53-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
     // uses the descriptor rule "SID={current account SID}"
     .ProtectKeysWithDpapiNG();
   ```

<span data-ttu-id="cfe53-134">В этом сценарии контроллер домена AD отвечает за распределение ключи шифрования, используемые в операциях DPAPI NG.</span><span class="sxs-lookup"><span data-stu-id="cfe53-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="cfe53-135">Целевого пользователя будет иметь возможность расшифровки зашифрованных полезных данных с любого компьютера, присоединенных к домену (при условии, что процесс выполняется их удостоверением).</span><span class="sxs-lookup"><span data-stu-id="cfe53-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="cfe53-136">Шифрование на основе сертификата с Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="cfe53-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="cfe53-137">Если вы работаете на Windows 8.1 или Windows Server 2012 R2 или более поздней версии, Windows DPAPI-NG можно использовать для выполнения шифрования на основе сертификатов, даже если приложение выполняется [.NET Core](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="cfe53-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on [.NET Core](https://www.microsoft.com/net/core).</span></span> <span data-ttu-id="cfe53-138">Для использования этой возможности, используйте строки дескриптора правило «сертификата = HashId:thumbprint», где отпечаток является шестнадцатеричной кодировке отпечаток SHA1 сертификата для использования.</span><span class="sxs-lookup"><span data-stu-id="cfe53-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="cfe53-139">Пример см. ниже.</span><span class="sxs-lookup"><span data-stu-id="cfe53-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
       // searches the cert store for the cert with this thumbprint
       .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
           flags: DpapiNGProtectionDescriptorFlags.None);
   ```

<span data-ttu-id="cfe53-140">Приложение, которое указывает на этот репозиторий должны быть запущены на Windows 8.1 или Windows Server 2012 R2 или более поздней версии, чтобы иметь возможность расшифровать этот ключ.</span><span class="sxs-lookup"><span data-stu-id="cfe53-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="cfe53-141">Пользовательские ключа шифрования</span><span class="sxs-lookup"><span data-stu-id="cfe53-141">Custom key encryption</span></span>

<span data-ttu-id="cfe53-142">Если в поле механизмы не подходят, разработчик может указать собственный механизм шифрования ключа, предоставляя пользовательские IXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="cfe53-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom IXmlEncryptor.</span></span>