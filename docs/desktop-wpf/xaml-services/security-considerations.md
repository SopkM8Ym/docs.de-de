---
title: XAML-Sicherheitsüberlegungen
ms.date: 03/30/2017
helpviewer_keywords:
- security [XAML Services], .NET XAML services
- XAML security [XAML Services]
ms.assetid: 544296d4-f38e-4498-af49-c9f4dad28964
ms.openlocfilehash: 1864910b339c74e3033fb4d6d8baebffada1a4f8
ms.sourcegitcommit: c2d9718996402993cf31541f11e95531bc68bad0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "81432917"
---
# <a name="xaml-security-considerations"></a><span data-ttu-id="9228b-102">XAML-Sicherheitsüberlegungen</span><span class="sxs-lookup"><span data-stu-id="9228b-102">XAML security considerations</span></span>

<span data-ttu-id="9228b-103">In diesem Artikel werden bewährte Methoden für die Sicherheit in Anwendungen beschrieben, wenn Sie XAML und .NET XAML Services API verwenden.</span><span class="sxs-lookup"><span data-stu-id="9228b-103">This article describes best practices for security in applications when you use XAML and .NET XAML Services API.</span></span>

## <a name="untrusted-xaml-in-applications"></a><span data-ttu-id="9228b-104">Nicht vertrauenswürdiges XAML in Anwendungen</span><span class="sxs-lookup"><span data-stu-id="9228b-104">Untrusted XAML in Applications</span></span>

<span data-ttu-id="9228b-105">Im weitesten Sinne ist nicht vertrauenswürdiges XAML eine XAML-Quelle, die Ihre Anwendung nicht ausdrücklich eingeschlossen oder ausgestoßen hat.</span><span class="sxs-lookup"><span data-stu-id="9228b-105">In the most general sense, untrusted XAML is any XAML source that your application did not specifically include or emit.</span></span>

<span data-ttu-id="9228b-106">XAML, das in einer `resx`vertrauenswürdigen und signierten Assembly kompiliert oder als -type-Ressource gespeichert wird, ist nicht von Natur aus nicht vertrauenswürdig.</span><span class="sxs-lookup"><span data-stu-id="9228b-106">XAML that is compiled into or stored as a `resx`-type resource within a trusted and signed assembly is not inherently untrusted.</span></span> <span data-ttu-id="9228b-107">Sie können dem XAML genauso vertrauen wie der Assembly als Ganzes.</span><span class="sxs-lookup"><span data-stu-id="9228b-107">You can trust the XAML as much as you trust the assembly as a whole.</span></span> <span data-ttu-id="9228b-108">In den meisten Fällen befassen Sie sich nur mit den Vertrauensaspekten von losem XAML, einer XAML-Quelle, die Sie aus einem Stream oder anderen E/A-Nachrichten laden.</span><span class="sxs-lookup"><span data-stu-id="9228b-108">In most cases, you are only concerned with the trust aspects of loose XAML, which is a XAML source that you load from a stream or other I/O.</span></span> <span data-ttu-id="9228b-109">Lose XAML ist keine bestimmte Komponente oder Funktion eines Anwendungsmodells mit einer Bereitstellungs- und Verpackungsinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9228b-109">Loose XAML is not a specific component or feature of an application model with a deployment and packaging infrastructure.</span></span> <span data-ttu-id="9228b-110">Eine Assembly kann jedoch ein Verhalten implementieren, das das Laden von losem XAML umfasst.</span><span class="sxs-lookup"><span data-stu-id="9228b-110">However, an assembly might implement a behavior that involves loading loose XAML.</span></span>

<span data-ttu-id="9228b-111">Bei nicht vertrauenswürdigem XAML sollten Sie es im Allgemeinen so behandeln, als wäre es nicht vertrauenswürdiger Code.</span><span class="sxs-lookup"><span data-stu-id="9228b-111">For untrusted XAML, you should treat it generally the same as if it were untrusted code.</span></span> <span data-ttu-id="9228b-112">Verwenden Sie Sandboxing oder andere Metaphern, um zu verhindern, dass möglicherweise nicht vertrauenswürdige XAML auf Ihren vertrauenswürdigen Code zugreifen.</span><span class="sxs-lookup"><span data-stu-id="9228b-112">Use sandboxing or other metaphors to prevent possibly untrusted XAML from accessing your trusted code.</span></span>

<span data-ttu-id="9228b-113">Die Art der XAML-Funktionen gibt XAML das Recht, Objekte zu erstellen und ihre Eigenschaften festzulegen.</span><span class="sxs-lookup"><span data-stu-id="9228b-113">The nature of XAML capabilities gives the XAML the right to construct objects and set their properties.</span></span> <span data-ttu-id="9228b-114">Zu diesen Funktionen gehören auch der Zugriff auf Typkonverter, das Zuordnen und `x:Code` Zugreifen auf Assemblys in der Anwendungsdomäne, die Verwendung von Markuperweiterungen, Blöcken usw.</span><span class="sxs-lookup"><span data-stu-id="9228b-114">These capabilities also include accessing type converters, mapping and accessing assemblies in the application domain, using markup extensions, `x:Code` blocks, and so on.</span></span>

<span data-ttu-id="9228b-115">Zusätzlich zu den Funktionen auf Sprachebene wird XAML für die UI-Definition in vielen Technologien verwendet.</span><span class="sxs-lookup"><span data-stu-id="9228b-115">In addition to its language-level capabilities, XAML is used for UI definition in many technologies.</span></span> <span data-ttu-id="9228b-116">Das Laden von nicht vertrauenswürdigem XAML kann das Laden einer bösartigen Spoofing-Benutzeroberfläche bedeuten.</span><span class="sxs-lookup"><span data-stu-id="9228b-116">Loading untrusted XAML might mean loading a malicious spoofing UI.</span></span>

## <a name="sharing-context-between-readers-and-writers"></a><span data-ttu-id="9228b-117">Freigeben von Kontext zwischen Lesern und Autoren</span><span class="sxs-lookup"><span data-stu-id="9228b-117">Sharing Context Between Readers and Writers</span></span>

<span data-ttu-id="9228b-118">.NET XAML Services-Architektur für XAML-Reader und XAML-Writer erfordert häufig die Freigabe eines XAML-Readers für einen XAML-Writer oder einen freigegebenen XAML-Schemakontext.</span><span class="sxs-lookup"><span data-stu-id="9228b-118">.NET XAML Services architecture for XAML readers and XAML writers often requires sharing a XAML reader to a XAML writer, or a shared XAML schema context.</span></span> <span data-ttu-id="9228b-119">Wenn Sie XAML-Knotenschleifenlogik schreiben oder einen benutzerdefinierten Speicherpfad bereitstellen, ist möglicherweise die Freigabe von Objekten oder Kontexten erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9228b-119">Sharing objects or contexts might be required if you are writing XAML node loop logic, or providing a custom save path.</span></span> <span data-ttu-id="9228b-120">Geben Sie XAML-Readerinstanzen, nicht standardmäßigen XAML-Schemakontext oder Einstellungen für XAML-Reader-/Writer-Klassen zwischen vertrauenswürdigem und nicht vertrauenswürdigem Code nicht gemeinsam.</span><span class="sxs-lookup"><span data-stu-id="9228b-120">Don't share XAML reader instances, nondefault XAML schema context, or settings for XAML reader/writer classes between trusted and untrusted code.</span></span>

<span data-ttu-id="9228b-121">Die meisten Szenarien und Vorgänge, die das Schreiben von XAML-Objekten für eine CLR-basierte Typsicherung beinhalten, können nur den standardmäßigen XAML-Schemakontext verwenden.</span><span class="sxs-lookup"><span data-stu-id="9228b-121">Most scenarios and operations involving XAML object writing for a CLR-based type backing can just use default XAML schema context.</span></span> <span data-ttu-id="9228b-122">Der standardmäßige XAML-Schemakontext enthält keine expliziten Einstellungen, die die volle Vertrauenswürdigkeit gefährden könnten.</span><span class="sxs-lookup"><span data-stu-id="9228b-122">The default XAML schema context does not explicitly include settings that could compromise full trust.</span></span> <span data-ttu-id="9228b-123">Es ist daher sicher, den Kontext zwischen vertrauenswürdigen und nicht vertrauenswürdigen XAML-Reader-/Writer-Komponenten gemeinsam zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="9228b-123">It is thus safe to share context between trusted and untrusted XAML reader/writer components.</span></span> <span data-ttu-id="9228b-124">Wenn Sie dies jedoch tun, ist es immer noch eine <xref:System.AppDomain> bewährte Methode, solche Leser und Autoren in getrennten Bereichen zu halten, wobei einer von ihnen speziell für teilweise vertrauenswürdig gedacht/sandboxed ist.</span><span class="sxs-lookup"><span data-stu-id="9228b-124">However, if you do this, it is still a best practice to keep such readers and writers in separate <xref:System.AppDomain> scopes, with one of them specifically intended/sandboxed for partial trust.</span></span>

## <a name="xaml-namespaces-and-assembly-trust"></a><span data-ttu-id="9228b-125">XAML-Namespaces und Assembly-Vertrauensstellung</span><span class="sxs-lookup"><span data-stu-id="9228b-125">XAML Namespaces and Assembly Trust</span></span>

<span data-ttu-id="9228b-126">Die grundlegende nicht qualifizierte Syntax und Definition für die Interpretation einer benutzerdefinierten XAML-Namespacezuordnung zu einer Assembly unterscheidet nicht zwischen einer vertrauenswürdigen und einer nicht vertrauenswürdigen Assembly, die in die Anwendungsdomäne geladen wird.</span><span class="sxs-lookup"><span data-stu-id="9228b-126">The basic unqualified syntax and definition for how XAML interprets a custom XAML namespace mapping to an assembly does not distinguish between a trusted and untrusted assembly as loaded into the application domain.</span></span> <span data-ttu-id="9228b-127">Daher ist es technisch möglich, dass eine nicht vertrauenswürdige Assembly die beabsichtigte XAML-Namespacezuordnung einer vertrauenswürdigen Assembly vortäuscht und die deklarierten Objekt- und Eigenschaftsinformationen einer XAML-Quelle erfasst.</span><span class="sxs-lookup"><span data-stu-id="9228b-127">Thus, it is technically possible for an untrusted assembly to spoof a trusted assembly's intended XAML namespace mapping and capture a XAML source's declared object and property information.</span></span> <span data-ttu-id="9228b-128">Wenn Sie Sicherheitsanforderungen haben, um diese Situation zu vermeiden, sollte die beabsichtigte XAML-Namespacezuordnung mit einer der folgenden Techniken erfolgen:</span><span class="sxs-lookup"><span data-stu-id="9228b-128">If you have security requirements to avoid this situation, your intended XAML namespace mapping should be made using one of the following techniques:</span></span>

- <span data-ttu-id="9228b-129">Verwenden Sie einen vollqualifizierten Assemblynamen mit starkem Namen in jeder XAML-Namespacezuordnung, die vom XAML Ihrer Anwendung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="9228b-129">Use a fully qualified assembly name with strong name in any XAML namespace mapping made by your application's XAML.</span></span>

- <span data-ttu-id="9228b-130">Beschränken Sie die Assemblyzuordnung auf einen festen <xref:System.Xaml.XamlSchemaContext> Satz von Verweisassemblys, indem Sie eine bestimmte für Ihre XAML-Lesegeräte und XAML-Objektschreiber erstellen.</span><span class="sxs-lookup"><span data-stu-id="9228b-130">Restrict assembly mapping to a fixed set of reference assemblies, by constructing a specific <xref:System.Xaml.XamlSchemaContext> for your XAML readers and XAML object writers.</span></span> <span data-ttu-id="9228b-131">Siehe <xref:System.Xaml.XamlSchemaContext.%23ctor%28System.Collections.Generic.IEnumerable%7BSystem.Reflection.Assembly%7D%29>.</span><span class="sxs-lookup"><span data-stu-id="9228b-131">See <xref:System.Xaml.XamlSchemaContext.%23ctor%28System.Collections.Generic.IEnumerable%7BSystem.Reflection.Assembly%7D%29>.</span></span>

## <a name="xaml-type-mapping-and-type-system-access"></a><span data-ttu-id="9228b-132">XAML-Typzuordnung und Systemzugriff</span><span class="sxs-lookup"><span data-stu-id="9228b-132">XAML Type Mapping and Type System Access</span></span>

<span data-ttu-id="9228b-133">XAML unterstützt ein eigenes Typsystem, das in vielerlei Hinsicht ein Peer ist, wie CLR das grundlegende CLR-Typsystem implementiert.</span><span class="sxs-lookup"><span data-stu-id="9228b-133">XAML supports its own type system, which in many ways is a peer to how CLR implements the basic CLR type system.</span></span> <span data-ttu-id="9228b-134">Bei bestimmten Aspekten der Typbewissung, bei denen Sie Vertrauensentscheidungen zu einem Typ basierend auf seinen Typinformationen treffen, sollten Sie die Typinformationen in den CLR-Sicherungstypen aufschieben.</span><span class="sxs-lookup"><span data-stu-id="9228b-134">However, for certain aspects of type awareness where you are making trust decisions about a type based on its type information, you should defer to the type information in the CLR backing types.</span></span> <span data-ttu-id="9228b-135">Dies liegt daran, dass einige der spezifischen Berichtsfunktionen des XAML-Typsystems als virtuelle Methoden offen gelassen werden und daher nicht vollständig unter der Kontrolle der ursprünglichen .NET XAML Services-Implementierungen stehen.</span><span class="sxs-lookup"><span data-stu-id="9228b-135">This is because some of the specific reporting capabilities of the XAML type system are left open as virtual methods and are therefore, not fully under the control of the original .NET XAML Services implementations.</span></span> <span data-ttu-id="9228b-136">Diese Erweiterbarkeitspunkte sind vorhanden, da das XAML-Typsystem erweiterbar ist, um der Erweiterbarkeit von XAML selbst und seinen möglichen alternativen Typzuordnungsstrategien im Vergleich zur standardmäßigen CLR-gestützten Implementierung und dem standardmäßigen XAML-Schemakontext zu entsprechen.</span><span class="sxs-lookup"><span data-stu-id="9228b-136">These extensibility points exist because the XAML type system is extensible, to match the extensibility of XAML itself and its possible alternative type-mapping strategies versus the default CLR-backed implementation and default XAML schema context.</span></span> <span data-ttu-id="9228b-137">Weitere Informationen finden Sie in den spezifischen <xref:System.Xaml.XamlType> Hinweisen <xref:System.Xaml.XamlMember>zu mehreren Eigenschaften von und .</span><span class="sxs-lookup"><span data-stu-id="9228b-137">For more information, see the specific notes on several of the properties of <xref:System.Xaml.XamlType> and <xref:System.Xaml.XamlMember>.</span></span>

## <a name="see-also"></a><span data-ttu-id="9228b-138">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="9228b-138">See also</span></span>

- <xref:System.Xaml.Permissions.XamlAccessLevel>