---
title: Auflistungen und Auflistungstypen für XAML
ms.date: 03/30/2017
ms.assetid: 58f8e7c6-9a41-4f25-8551-c042f1315baa
ms.openlocfilehash: 2ec58026c605df31489c8aab4c4cc714dab11474
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "81432581"
---
# <a name="collections-and-collection-types-for-xaml"></a><span data-ttu-id="15b54-102">Auflistungen und Auflistungstypen für XAML</span><span class="sxs-lookup"><span data-stu-id="15b54-102">Collections and collection types for XAML</span></span>

<span data-ttu-id="15b54-103">In diesem Artikel wird beschrieben, wie Eigenschaften von Typen definiert werden, die eine Auflistung unterstützen sollen, und wie die XAML-Syntax zum Instanziieren von Auflistungselementen als elementuntergeordneteElemente eines übergeordneten Objektelements oder Eigenschaftselements unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="15b54-103">This article describes how to define properties of types that are intended to support a collection, and to support the XAML syntax for instantiating collection items as element children of a parent object element or property element.</span></span>

## <a name="xaml-collection-concepts"></a><span data-ttu-id="15b54-104">XAML-Auflistungskonzepte</span><span class="sxs-lookup"><span data-stu-id="15b54-104">XAML Collection Concepts</span></span>

<span data-ttu-id="15b54-105">Im Prinzip muss jede Beziehung in XAML, in der sich mehrere untergeordnete Elemente im Bereich eines XAML-Objektelements oder XAML-Eigenschaftselements befinden, als Auflistung implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="15b54-105">Conceptually, any relationship in XAML where there are multiple child items within the scope of a XAML object element or XAML property element must be implemented as a collection.</span></span> <span data-ttu-id="15b54-106">Diese Auflistung muss einer bestimmten XAML-Eigenschaft des XAML-Typs zugeordnet sein, der das übergeordnete Element in dieser Beziehung ist.</span><span class="sxs-lookup"><span data-stu-id="15b54-106">That collection must be associated with a particular XAML property of the XAML type that is the parent in that relationship.</span></span> <span data-ttu-id="15b54-107">Die Eigenschaft muss eine Auflistung sein, da ein XAML-Prozessor erwartet, dass jedes Element in Markup als neu hinzugefügtes Element der Backing Collection-Eigenschaft zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="15b54-107">The property must be a collection because a XAML processor expects to assign each item in markup to be a newly added item of the backing collection property.</span></span>

<span data-ttu-id="15b54-108">Auf der XAML-Sprachebene sind die genauen Anforderungen an die Sammlungsunterstützung nicht vollständig definiert.</span><span class="sxs-lookup"><span data-stu-id="15b54-108">At the XAML language level, the exact requirements of collection support are not fully defined.</span></span> <span data-ttu-id="15b54-109">Das Konzept, dass eine Auflistung entweder eine Liste oder ein Wörterbuch (aber nicht beides) sein kann, wird auf XAML-Sprachebene definiert, aber welche Sicherungstypen entweder Listen oder Wörterbücher darstellen, wird nicht durch die XAML-Sprache definiert.</span><span class="sxs-lookup"><span data-stu-id="15b54-109">The concept that a collection can be either a list or a dictionary(but not both) is defined at the XAML language level, but which backing types represent either lists or dictionaries is not defined by the XAML language.</span></span>

<span data-ttu-id="15b54-110">In .NET XAML Services ist das Konzept der XAML-Auflistungsunterstützung in Bezug auf .NET-Sicherungstypen klarer definiert.</span><span class="sxs-lookup"><span data-stu-id="15b54-110">In .NET XAML Services, the concept of XAML collection support is more clearly defined in terms of .NET backing types.</span></span> <span data-ttu-id="15b54-111">Insbesondere basiert die XAML-Unterstützung für Sammlungen auf mehreren .NET-Konzepten und APIs, die für Listen und Wörterbücher in der allgemeinen .NET-Programmierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="15b54-111">Specifically, the XAML support for collections is based on several .NET concepts and APIs that are used for lists and dictionaries in general .NET programming.</span></span>

1. <span data-ttu-id="15b54-112">Die <xref:System.Collections.IList> Schnittstelle gibt eine Listenauflistung an.</span><span class="sxs-lookup"><span data-stu-id="15b54-112">The <xref:System.Collections.IList> interface indicates a list collection.</span></span>

2. <span data-ttu-id="15b54-113">Die <xref:System.Collections.IDictionary> Schnittstelle gibt eine Wörterbuchsammlung an.</span><span class="sxs-lookup"><span data-stu-id="15b54-113">The <xref:System.Collections.IDictionary> interface indicates a dictionary collection.</span></span>

3. <span data-ttu-id="15b54-114"><xref:System.Array>stellt ein Array dar, <xref:System.Collections.IList> und ein Array unterstützt Methoden.</span><span class="sxs-lookup"><span data-stu-id="15b54-114"><xref:System.Array> represents an array, and an array supports <xref:System.Collections.IList> methods.</span></span>

<span data-ttu-id="15b54-115">In jedem dieser Auflistungskonzepte erwartet ein .NET XAML Services `Add` XAML-Prozessor, dass die Methode für eine bestimmte Instanz des Auflistungseigenschaftstyps aufruft.</span><span class="sxs-lookup"><span data-stu-id="15b54-115">In each of these collection concepts, a .NET XAML Services XAML processor expects to call the `Add` method on a specific instance of the collection property's type.</span></span> <span data-ttu-id="15b54-116">Oder in einem Serialisierungsszenario erstellt ein XAML-Prozessor diskrete XAML-Typinstanzen für jedes Element, das in der Liste, im Wörterbuch oder im Array gefunden wird, basierend auf dem spezifischen Konzept der einzelnen "Elemente" jeder Auflistung.</span><span class="sxs-lookup"><span data-stu-id="15b54-116">Or, in a serialization scenario, a XAML processor produces discrete XAML-type instances for each item found in the list, dictionary, or array based on each collection's specific concept of "Items".</span></span> <span data-ttu-id="15b54-117">Diese Punkte <xref:System.Collections.IList.Item%2A?displayProperty=nameWithType>sind: ; <xref:System.Collections.IDictionary.Item%2A?displayProperty=nameWithType>; die <xref:System.Array.System%23Collections%23IList%23Item%2A?displayProperty=nameWithType> explizite für <xref:System.Array>.</span><span class="sxs-lookup"><span data-stu-id="15b54-117">These items are: <xref:System.Collections.IList.Item%2A?displayProperty=nameWithType>; <xref:System.Collections.IDictionary.Item%2A?displayProperty=nameWithType>; the explicit <xref:System.Array.System%23Collections%23IList%23Item%2A?displayProperty=nameWithType> for <xref:System.Array>.</span></span>

## <a name="generic-collections"></a><span data-ttu-id="15b54-118">Generische Sammlungen</span><span class="sxs-lookup"><span data-stu-id="15b54-118">Generic Collections</span></span>

<span data-ttu-id="15b54-119">Generische Auflistungen können für die allgemeine .NET-Programmierung nützlich sein und auch für XAML-Auflistungseigenschaften verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="15b54-119">Generic collections can be useful for general .NET programming, and can also be used for XAML collection properties.</span></span> <span data-ttu-id="15b54-120">Die generischen Schnittstellen <xref:System.Collections.Generic.IList%601> <xref:System.Collections.Generic.IDictionary%602> und werden jedoch nicht von .NET XAML Services XAML-Prozessoren als äquivalent zu den nicht generischen <xref:System.Collections.IList> oder <xref:System.Collections.IDictionary>.</span><span class="sxs-lookup"><span data-stu-id="15b54-120">However, the generic interfaces <xref:System.Collections.Generic.IList%601> and <xref:System.Collections.Generic.IDictionary%602> are not identified by .NET XAML Services XAML processors as being equivalent to the non-generic <xref:System.Collections.IList> or <xref:System.Collections.IDictionary>.</span></span> <span data-ttu-id="15b54-121">Anstatt die Schnittstellen zu implementieren, wird ein empfohlener Ansatz für <xref:System.Collections.Generic.List%601> generische <xref:System.Collections.Generic.Dictionary%602>Auflistungseigenschaftstypen von den Klassen oder ableiten.</span><span class="sxs-lookup"><span data-stu-id="15b54-121">Rather than implementing the interfaces, a recommended approach for generic collection property types is to derive from the classes <xref:System.Collections.Generic.List%601> or <xref:System.Collections.Generic.Dictionary%602>.</span></span> <span data-ttu-id="15b54-122">Diese Klassen implementieren die nicht generischen Schnittstellen und enthalten daher die erwartete Unterstützung für XAML-Auflistungen in der Basisimplementierung.</span><span class="sxs-lookup"><span data-stu-id="15b54-122">These classes implement the non-generic interfaces and thus include the expected support for XAML collections in the base implementation.</span></span>

## <a name="read-only-collections-and-initialization-logic"></a><span data-ttu-id="15b54-123">Read-Only Collections und Initialisierungslogik</span><span class="sxs-lookup"><span data-stu-id="15b54-123">Read-Only Collections and Initialization Logic</span></span>

<span data-ttu-id="15b54-124">In der .NET-Programmierung ist es ein allgemeines Entwurfsmuster, jede Eigenschaft, die einen Wert einer Auflistung enthält, als schreibgeschützte Auflistung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="15b54-124">In .NET programming, it is a common design pattern to make any property that holds a value of a collection as a read-only collection.</span></span> <span data-ttu-id="15b54-125">Dieses Muster ermöglicht es der Instanz, die Eigentümerin der Auflistungseigenschaft ist, besser zu steuern, was mit der Auflistung geschieht.</span><span class="sxs-lookup"><span data-stu-id="15b54-125">This pattern permits the instance that owns the collection property to better control what happens to the collection..</span></span> <span data-ttu-id="15b54-126">Insbesondere verhindert das Muster den versehentlichen Austausch der gesamten bereits vorhandenen Auflistung durch Festlegen der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="15b54-126">Specifically, the pattern prevents accidental replacement of the entire pre-existing collection by setting the property.</span></span> <span data-ttu-id="15b54-127">In diesem Muster sollte jeder Zugriff von Aufrufern auf die Auflistung stattdessen durch Aufrufen von Methoden oder Eigenschaften <xref:System.Collections.IList>erfolgen, die vom Auflistungstyp und/oder den entsprechenden Auflistungsschnittstellen wie unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="15b54-127">In this pattern, any access to the collection by callers should instead be made by calling methods or properties as supported by the collection type and/or the relevant collection interfaces such as <xref:System.Collections.IList>.</span></span>

<span data-ttu-id="15b54-128">Die Verwendung dieses Musters impliziert, dass jede Klasse, die eine schreibgeschützte Auflistungseigenschaft verfügbar macht, diese Eigenschaft zuerst initialisieren muss, um eine leere Auflistung zu enthalten.</span><span class="sxs-lookup"><span data-stu-id="15b54-128">Using this pattern implies that any class that exposes a read-only collection property must first initialize that property to hold an empty collection.</span></span> <span data-ttu-id="15b54-129">In der Regel wird die Initialisierung als Teil des Konstruktionsverhaltens für die Klasse durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="15b54-129">Typically the initialization is performed as part of the construction behavior for the class.</span></span> <span data-ttu-id="15b54-130">Um für XAML nützlich zu sein, ist es wichtig, dass auf diese Logik immer vom parameterlosen Konstruktor verwiesen wird, da XAML im Allgemeinen den parameterlosen Konstruktor aufruft, bevor die Eigenschaften verarbeitet werden (Auflistungseigenschaften oder auf andere Weise).</span><span class="sxs-lookup"><span data-stu-id="15b54-130">To be useful for XAML, it is important that such logic is always referenced by the parameterless constructor, because XAML generally calls the parameterless constructor prior to processing the properties (collection properties or otherwise).</span></span>

## <a name="xaml-type-system-support-and-collections"></a><span data-ttu-id="15b54-131">XAML-Typsystemunterstützung und -sammlungen</span><span class="sxs-lookup"><span data-stu-id="15b54-131">XAML Type System Support and Collections</span></span>

<span data-ttu-id="15b54-132">Neben der grundlegenden Mechanik des Analysierens von XAML und des Auffüllens oder Serialisierens von Auflistungseigenschaften enthält das XAML-Typsystem, wie es in .NET XAML Services implementiert ist, mehrere Entwurfsfeatures, die sich auf Sammlungen in XAML beziehen.</span><span class="sxs-lookup"><span data-stu-id="15b54-132">Beyond the basic mechanics of parsing XAML and populating or serializing collection properties, the XAML type system as implemented in .NET XAML Services includes several design features that pertain to collections in XAML.</span></span>

1. <span data-ttu-id="15b54-133"><xref:System.Xaml.XamlType.IsCollection%2A>gibt true zurück, wenn der XAML-Typ von einem Typ unterstützt wird, der XAML-Auflistungsunterstützung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="15b54-133"><xref:System.Xaml.XamlType.IsCollection%2A> returns true if the XAML type is backed by a type that provides XAML collection support.</span></span>

2. <span data-ttu-id="15b54-134"><xref:System.Xaml.XamlType.IsDictionary%2A>und <xref:System.Xaml.XamlType.IsArray%2A> kann weiter identifizieren, welchen Erfassungsmodus der XAML-Typ unterstützt.</span><span class="sxs-lookup"><span data-stu-id="15b54-134"><xref:System.Xaml.XamlType.IsDictionary%2A> and <xref:System.Xaml.XamlType.IsArray%2A> can further identify which collection mode the XAML type supports.</span></span> <span data-ttu-id="15b54-135">Für benutzerdefinierte XAML-Prozessoren, die auf .NET XAML-Diensten und dem <xref:System.Xaml.XamlWriter> XAML-Typsystem basieren, aber nicht auf vorhandenen Implementierungen basieren, ist möglicherweise zu wissen, welcher Erfassungsmodus verwendet wird, um zu wissen, welche Methode für die Sammlungsverarbeitung aufgerufen werden soll.</span><span class="sxs-lookup"><span data-stu-id="15b54-135">For custom XAML processors that are based on .NET XAML Services and the XAML type system but not based on existing <xref:System.Xaml.XamlWriter> implementations, knowing which collection mode is used might be necessary in order to know which method to invoke for collection processing.</span></span>

3. <span data-ttu-id="15b54-136">Jeder der vorherigen Eigenschaftswerte wird möglicherweise <xref:System.Xaml.XamlType.LookupCollectionKind%2A> durch Überschreibungen eines XAML-Typs beeinflusst.</span><span class="sxs-lookup"><span data-stu-id="15b54-136">Each of the previous property values is potentially influenced by overrides of <xref:System.Xaml.XamlType.LookupCollectionKind%2A> on a XAML type.</span></span>