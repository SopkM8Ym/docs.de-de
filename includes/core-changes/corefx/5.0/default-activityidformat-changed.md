---
ms.openlocfilehash: d75dc2caa3a002b9d34ac74f2c5c5e192281c0df
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281299"
---
### <a name="default-activityidformat-is-w3c"></a><span data-ttu-id="477cb-101">Das ActivityIdFormat-Standardformat ist W3C</span><span class="sxs-lookup"><span data-stu-id="477cb-101">Default ActivityIdFormat is W3C</span></span>

<span data-ttu-id="477cb-102">Das Standardformat für Activity-IDs (<xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=nameWithType>) ist ab sofort <xref:System.Diagnostics.ActivityIdFormat.W3C?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="477cb-102">The default identifier format for activity (<xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=nameWithType>) is now <xref:System.Diagnostics.ActivityIdFormat.W3C?displayProperty=nameWithType>.</span></span>

#### <a name="change-description"></a><span data-ttu-id="477cb-103">Änderungsbeschreibung</span><span class="sxs-lookup"><span data-stu-id="477cb-103">Change description</span></span>

<span data-ttu-id="477cb-104">Das W3C-Activity-ID-Format wurde in .NET Core 3.0 als Alternative zum hierarchischen ID-Format eingeführt.</span><span class="sxs-lookup"><span data-stu-id="477cb-104">The W3C activity ID format was introduced in .NET Core 3.0 as an alternative to the hierarchical ID format.</span></span> <span data-ttu-id="477cb-105">Aus Kompatibilitätsgründen wurde das W3C-Format jedoch erst in .NET 5.0 zum Standardformat gemacht.</span><span class="sxs-lookup"><span data-stu-id="477cb-105">However, to preserve compatibility, the W3C format wasn't made the default until .NET 5.0.</span></span> <span data-ttu-id="477cb-106">Das Standardformat wurde in .NET 5.0 geändert, da das [W3C-Format ratifiziert](https://www.w3.org/TR/trace-context/) und in immer mehr Sprachimplementierungen verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="477cb-106">The default was changed in .NET 5.0 because the [W3C format has been ratified](https://www.w3.org/TR/trace-context/) and gained traction across multiple language implementations.</span></span>

<span data-ttu-id="477cb-107">Wenn Ihre App als Ziel eine andere Plattform als .NET 5.0 oder höhere Versionen verwendet, gilt das alte Verhalten. Das Standardformat wäre dann also <xref:System.Diagnostics.ActivityIdFormat.Hierarchical>.</span><span class="sxs-lookup"><span data-stu-id="477cb-107">If your app targets a platform other than .NET 5.0 or later, it will experience the old behavior, where <xref:System.Diagnostics.ActivityIdFormat.Hierarchical> is the default format.</span></span> <span data-ttu-id="477cb-108">Dieser Standard gilt für die Plattformen net45 und höher, netstandard1.1 und höher und netcoreapp (1.x, 2.x und 3.x).</span><span class="sxs-lookup"><span data-stu-id="477cb-108">This default applies to platforms net45+, netstandard1.1+, and netcoreapp (1.x, 2.x, and 3.x).</span></span> <span data-ttu-id="477cb-109">Ab .NET 5.0 wird <xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=nameWithType> auf <xref:System.Diagnostics.ActivityIdFormat.W3C?displayProperty=nameWithType> festgelegt.</span><span class="sxs-lookup"><span data-stu-id="477cb-109">In .NET 5.0 and later, <xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=nameWithType> is set to <xref:System.Diagnostics.ActivityIdFormat.W3C?displayProperty=nameWithType>.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="477cb-110">Eingeführt in Version</span><span class="sxs-lookup"><span data-stu-id="477cb-110">Version introduced</span></span>

<span data-ttu-id="477cb-111">5.0 Preview 7</span><span class="sxs-lookup"><span data-stu-id="477cb-111">5.0 Preview 7</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="477cb-112">Empfohlene Aktion</span><span class="sxs-lookup"><span data-stu-id="477cb-112">Recommended action</span></span>

<span data-ttu-id="477cb-113">Wenn die Anwendung nicht vom Bezeichner abhängig ist, der für die verteilte Ablaufverfolgung verwendet wird, ist keine Aktion erforderlich.</span><span class="sxs-lookup"><span data-stu-id="477cb-113">If your application is agnostic to the identifier that's used for distributed tracing, no action is needed.</span></span> <span data-ttu-id="477cb-114">Bibliotheken wie ASP.NET Core und <xref:System.Net.Http.HttpClient> können beide Versionen von <xref:System.Diagnostics.ActivityIdFormat> verwenden oder verteilen.</span><span class="sxs-lookup"><span data-stu-id="477cb-114">Libraries such as ASP.NET Core and <xref:System.Net.Http.HttpClient> can consume or propagate both versions of the <xref:System.Diagnostics.ActivityIdFormat>.</span></span>

<span data-ttu-id="477cb-115">Wenn eine Interoperabilität mit vorhandenen Systemen erforderlich ist oder aktuelle Systeme auf dem Format des Bezeichners basieren, können Sie das alte Verhalten beibehalten, indem Sie <xref:System.Diagnostics.Activity.DefaultIdFormat> auf <xref:System.Diagnostics.ActivityIdFormat.Hierarchical?displayProperty=nameWithType> festlegen.</span><span class="sxs-lookup"><span data-stu-id="477cb-115">If you require interoperability with existing systems, or current systems rely on the format of the identifier, you can preserve the old behavior by setting <xref:System.Diagnostics.Activity.DefaultIdFormat> to <xref:System.Diagnostics.ActivityIdFormat.Hierarchical?displayProperty=nameWithType>.</span></span> <span data-ttu-id="477cb-116">Alternativ können Sie einen Wechsel von AppContext auf drei Weisen umsetzen:</span><span class="sxs-lookup"><span data-stu-id="477cb-116">Alternatively, you can set an AppContext switch in one of three ways:</span></span>

- <span data-ttu-id="477cb-117">In der Projektdatei.</span><span class="sxs-lookup"><span data-stu-id="477cb-117">In the project file.</span></span>

  ```xml
  <ItemGroup>
    <RuntimeHostConfigurationOption Include="System.Diagnostics.DefaultActivityIdFormatIsHierarchial" Value="true" />
  </ItemGroup>
  ```

- <span data-ttu-id="477cb-118">In der *runtimeconfig.json*-Datei.</span><span class="sxs-lookup"><span data-stu-id="477cb-118">In the *runtimeconfig.json* file.</span></span>

  ```json
  {
      "runtimeOptions": {
          "configProperties": {
              "System.Diagnostics.DefaultActivityIdFormatIsHierarchial": true
          }
      }
  }
  ```

- <span data-ttu-id="477cb-119">Über eine Umgebungsvariable.</span><span class="sxs-lookup"><span data-stu-id="477cb-119">Through an environment variable.</span></span>

  <span data-ttu-id="477cb-120">`DOTNET_SYSTEM_DIAGNOSTICS_DEFAULTACTIVITYIDFORMATISHIERARCHIAL` wird auf `true` oder 1 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="477cb-120">Set `DOTNET_SYSTEM_DIAGNOSTICS_DEFAULTACTIVITYIDFORMATISHIERARCHIAL` to `true` or 1.</span></span>

#### <a name="category"></a><span data-ttu-id="477cb-121">Kategorie</span><span class="sxs-lookup"><span data-stu-id="477cb-121">Category</span></span>

<span data-ttu-id="477cb-122">Core .NET-Bibliotheken</span><span class="sxs-lookup"><span data-stu-id="477cb-122">Core .NET libraries</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="477cb-123">Betroffene APIs</span><span class="sxs-lookup"><span data-stu-id="477cb-123">Affected APIs</span></span>

- <xref:System.Diagnostics.Activity.DefaultIdFormat?displayProperty=fullName>

<!--

#### Affected APIs

- `P:System.Diagnostics.Activity.DefaultIdFormat`

-->