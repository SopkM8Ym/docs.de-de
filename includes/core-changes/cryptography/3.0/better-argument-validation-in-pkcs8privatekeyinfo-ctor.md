---
ms.openlocfilehash: a9b6af31b68c25ab58c52757f48ed23cca3f5a35
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2019
ms.locfileid: "74567997"
---
### <a name="better-argument-validation-in-the-pkcs8privatekeyinfo-constructor"></a><span data-ttu-id="60c55-101">Bessere Argumentvalidierung im Pkcs8PrivateKeyInfo-Konstruktor</span><span class="sxs-lookup"><span data-stu-id="60c55-101">Better argument validation in the Pkcs8PrivateKeyInfo constructor</span></span>

<span data-ttu-id="60c55-102">Ab .NET Core 3.0 Vorschau 9 validiert der `Pkcs8PrivateKeyInfo`-Konstruktor den Parameter `algorithmParameters` als einzelnen BER-codierten Wert.</span><span class="sxs-lookup"><span data-stu-id="60c55-102">Starting with .NET Core 3.0 Preview 9, the `Pkcs8PrivateKeyInfo` constructor validates the `algorithmParameters` parameter as a single BER-encoded value.</span></span>

#### <a name="change-description"></a><span data-ttu-id="60c55-103">Änderungsbeschreibung</span><span class="sxs-lookup"><span data-stu-id="60c55-103">Change description</span></span>

<span data-ttu-id="60c55-104">Vor .NET Core 3.0 Vorschau 9 hat der [`Pkcs8PrivateKeyInfo`-Konstruktor](xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.%23ctor(System.Security.Cryptography.Oid,System.Nullable%7BSystem.ReadOnlyMemory%7BSystem.Byte%7D%7D,System.ReadOnlyMemory%7BSystem.Byte%7D,System.Boolean)) das Argument `algorithmParameters` nicht validiert.</span><span class="sxs-lookup"><span data-stu-id="60c55-104">Before .NET Core 3.0 Preview 9, the [`Pkcs8PrivateKeyInfo` constructor](xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.%23ctor(System.Security.Cryptography.Oid,System.Nullable%7BSystem.ReadOnlyMemory%7BSystem.Byte%7D%7D,System.ReadOnlyMemory%7BSystem.Byte%7D,System.Boolean)) did not validate the `algorithmParameters` argument.</span></span>  <span data-ttu-id="60c55-105">Wenn dieses Argument ein ungültiger Wert war, war der Konstruktor zwar erfolgreich, aber ein Aufruf einer der Methoden <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encode>, <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.TryEncode%2A>, <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encrypt%2A> oder <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.TryEncrypt%2A> löste entweder eine <xref:System.ArgumentException> für ein nicht akzeptiertes Argument (`preEncodedValue`) oder eine <xref:System.Security.Cryptography.CryptographicException> aus.</span><span class="sxs-lookup"><span data-stu-id="60c55-105">When this argument represented an invalid value, the constructor would succeed, but a call to any of the <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encode>, <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.TryEncode%2A>, <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encrypt%2A>, or <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.TryEncrypt%2A> methods would throw either an <xref:System.ArgumentException> for an argument they did not accept (`preEncodedValue`) or a <xref:System.Security.Cryptography.CryptographicException>.</span></span>

<span data-ttu-id="60c55-106">Wenn der folgende Code mit .NET Core 3.0 vor Vorschau 9 ausgeführt wird, löst er nur dann eine Ausnahme aus, wenn die <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encode>-Methode aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="60c55-106">If run with .NET Core 3.0 before Preview 9, the following code throws an exception only when the <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.Encode> method is called:</span></span>

```csharp
byte[] algorithmParameters = { 0x05, 0x00, 0x05, 0x00 };

var info = new Pkcs8PrivateKeyInfo(algorithmId, algorithmParameters, privateKey);
// This line throws an ArgumentException for `preEncodedValue`:
byte[] encoded = info.Encode();
```

<span data-ttu-id="60c55-107">Ab .NET Core 3.0 Vorschau 9 wird das Argument im Konstruktor validiert, und ein ungültiger Wert führt dazu, dass die Methode eine <xref:System.Security.Cryptography.CryptographicException> auslöst.</span><span class="sxs-lookup"><span data-stu-id="60c55-107">Starting with .NET Core 3.0 Preview 9, the argument is validated in the constructor, and an invalid value results in the method throwing a <xref:System.Security.Cryptography.CryptographicException>.</span></span> <span data-ttu-id="60c55-108">Diese Änderung verschiebt die Ausnahme näher an die Quelle des Datenfehlers.</span><span class="sxs-lookup"><span data-stu-id="60c55-108">This change moves the exception closer to the source of the data error.</span></span> <span data-ttu-id="60c55-109">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="60c55-109">For example:</span></span>

```csharp
byte[] algorithmParameters = { 0x05, 0x00, 0x05, 0x00 };

// This line throws a CryptographicException
var info = new Pkcs8PrivateKeyInfo(algorithmId, algorithmParameters, privateKey);
```

#### <a name="version-introduced"></a><span data-ttu-id="60c55-110">Eingeführt in Version</span><span class="sxs-lookup"><span data-stu-id="60c55-110">Version introduced</span></span>

<span data-ttu-id="60c55-111">3.0 Vorschau 9</span><span class="sxs-lookup"><span data-stu-id="60c55-111">3.0 Preview 9</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="60c55-112">Empfohlene Maßnahme</span><span class="sxs-lookup"><span data-stu-id="60c55-112">Recommended action</span></span>

<span data-ttu-id="60c55-113">Stellen Sie sicher, dass nur gültige `algorithmParameters`-Werte angegeben werden, oder dass Aufrufe des `Pkcs8PrivateKeyInfo`-Konstruktortests für <xref:System.ArgumentException> und <xref:System.Security.Cryptography.CryptographicException> erfolgen, wenn Ausnahmebehandlung gewünscht wird.</span><span class="sxs-lookup"><span data-stu-id="60c55-113">Ensure that only valid `algorithmParameters` values are provided, or that calls to the `Pkcs8PrivateKeyInfo` constructor test for both <xref:System.ArgumentException> and <xref:System.Security.Cryptography.CryptographicException> if exception handling is desired.</span></span>

### <a name="category"></a><span data-ttu-id="60c55-114">Kategorie</span><span class="sxs-lookup"><span data-stu-id="60c55-114">Category</span></span>

<span data-ttu-id="60c55-115">Kryptografie</span><span class="sxs-lookup"><span data-stu-id="60c55-115">Cryptography</span></span>

### <a name="affected-apis"></a><span data-ttu-id="60c55-116">Betroffene APIs</span><span class="sxs-lookup"><span data-stu-id="60c55-116">Affected APIs</span></span>

- <span data-ttu-id="60c55-117">[Pkcs8PrivateKeyInfo-Konstruktor](xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.%23ctor(System.Security.Cryptography.Oid,System.Nullable%7BSystem.ReadOnlyMemory%7BSystem.Byte%7D%7D,System.ReadOnlyMemory%7BSystem.Byte%7D,System.Boolean))</span><span class="sxs-lookup"><span data-stu-id="60c55-117">[Pkcs8PrivateKeyInfo constructor](xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.%23ctor(System.Security.Cryptography.Oid,System.Nullable%7BSystem.ReadOnlyMemory%7BSystem.Byte%7D%7D,System.ReadOnlyMemory%7BSystem.Byte%7D,System.Boolean))</span></span>

<!--

### Affected APIs

- `M:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo.#ctor(System.Security.Cryptography.Oid,System.Nullable{System.ReadOnlyMemory{System.Byte}},System.ReadOnlyMemory{System.Byte},System.Boolean))

-->