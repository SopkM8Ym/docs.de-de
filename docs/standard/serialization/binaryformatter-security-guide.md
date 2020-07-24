---
title: BinaryFormatter-Sicherheitsleitfaden
description: In diesem Artikel werden die Sicherheitsrisiken beschrieben, die der Typ BinaryFormatter mit sich bringt. Darüber hinaus werden verschiedene Serialisierungsmodule empfohlen, die verwendet werden können.
ms.date: 07/11/2020
ms.author: levib
author: GrabYourPitchforks
ms.openlocfilehash: f6a54b34bbf1e19212fe37aadb448a1722fe9ff0
ms.sourcegitcommit: 2543a78be6e246aa010a01decf58889de53d1636
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2020
ms.locfileid: "86444749"
---
# <a name="binaryformatter-security-guide"></a><span data-ttu-id="8ad01-103">BinaryFormatter-Sicherheitsleitfaden</span><span class="sxs-lookup"><span data-stu-id="8ad01-103">BinaryFormatter security guide</span></span>

<span data-ttu-id="8ad01-104">Dieser Artikel bezieht sich auf die folgenden .NET-Implementierungen:</span><span class="sxs-lookup"><span data-stu-id="8ad01-104">This article applies to the following .NET implementations:</span></span>

* <span data-ttu-id="8ad01-105">.NET Framework (alle Versionen)</span><span class="sxs-lookup"><span data-stu-id="8ad01-105">.NET Framework all versions</span></span>
* <span data-ttu-id="8ad01-106">.NET Core 2.1–3.1</span><span class="sxs-lookup"><span data-stu-id="8ad01-106">.NET Core 2.1 - 3.1</span></span>
* <span data-ttu-id="8ad01-107">.NET 5.0 und höher</span><span class="sxs-lookup"><span data-stu-id="8ad01-107">.NET 5.0 and later</span></span>

## <a name="background"></a><span data-ttu-id="8ad01-108">Hintergrund</span><span class="sxs-lookup"><span data-stu-id="8ad01-108">Background</span></span>

> [!WARNING]
> <span data-ttu-id="8ad01-109">Der Typ <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> ist gefährlich und wird für die Datenverarbeitung ***nicht*** empfohlen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-109">The <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> type is dangerous and is ***not*** recommended for data processing.</span></span> <span data-ttu-id="8ad01-110">Anwendungen sollten so bald wie möglich aufhören, `BinaryFormatter` zu verwenden, auch wenn Sie der Auffassung sind, dass die verarbeiteten Daten vertrauenswürdig sind.</span><span class="sxs-lookup"><span data-stu-id="8ad01-110">Applications should stop using `BinaryFormatter` as soon as possible, even if they believe the data they're processing to be trustworthy.</span></span> <span data-ttu-id="8ad01-111">`BinaryFormatter` ist unsicher und kann nicht sicher gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-111">`BinaryFormatter` is insecure can't be made secure.</span></span>

<span data-ttu-id="8ad01-112">Dieser Artikel bezieht sich auch auf die folgenden Typen:</span><span class="sxs-lookup"><span data-stu-id="8ad01-112">This article also applies to the following types:</span></span>

* <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
* <xref:System.Runtime.Serialization.NetDataContractSerializer>
* <xref:System.Web.UI.LosFormatter>
* <xref:System.Web.UI.ObjectStateFormatter>

<span data-ttu-id="8ad01-113">Sicherheitsrisiken im Zusammenhang mit der Deserialisierung sind eine Bedrohungskategorie, bei der Anforderungsnutzlasten unsicher verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-113">Deserialization vulnerabilities are a threat category where request payloads are processed insecurely.</span></span> <span data-ttu-id="8ad01-114">Ein Angreifer, der diese Sicherheitsrisiken bei einer App erfolgreich nutzt, kann einen Denial of Service (DoS), eine Veröffentlichung von Informationen oder eine Remoteausführung von Code in der Ziel-App bewirken.</span><span class="sxs-lookup"><span data-stu-id="8ad01-114">An attacker who successfully leverages these vulnerabilities against an app can cause denial of service (DoS), information disclosure, or remote code execution inside the target app.</span></span> <span data-ttu-id="8ad01-115">Diese Risikokategorie ist stets Teil der [OWASP Top 10](https://owasp.org/www-project-top-ten/).</span><span class="sxs-lookup"><span data-stu-id="8ad01-115">This risk category consistently makes the [OWASP Top 10](https://owasp.org/www-project-top-ten/).</span></span> <span data-ttu-id="8ad01-116">Ziele sind Apps in [vielen verschiedenen Sprachen](https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data), z. B C/C++, Java und C#.</span><span class="sxs-lookup"><span data-stu-id="8ad01-116">Targets include apps written in [a variety of languages](https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data), including C/C++, Java, and C#.</span></span>

<span data-ttu-id="8ad01-117">In .NET sind die Ziele mit dem höchsten Risiko Anwendungen, die <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> zum Deserialisieren von Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-117">In .NET, the biggest risk target is apps that use the <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> type to deserialize data.</span></span> <span data-ttu-id="8ad01-118">Aufgrund seiner Leistungsfähigkeit und Benutzerfreundlichkeit wird der Typ `BinaryFormatter` im gesamten .NET-Ökosystem häufig verwendet.</span><span class="sxs-lookup"><span data-stu-id="8ad01-118">`BinaryFormatter` is widely used throughout the .NET ecosystem because of its power and its ease of use.</span></span> <span data-ttu-id="8ad01-119">Genau diese Leistungsfähigkeit ermöglicht es Angreifern jedoch, die Ablaufsteuerung in der Ziel-App zu beeinflussen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-119">However, this same power gives attackers the ability to influence control flow within the target app.</span></span> <span data-ttu-id="8ad01-120">Erfolgreiche Angriffe können dazu führen, dass der Angreifer Code im Kontext des Zielprozesses ausführen kann.</span><span class="sxs-lookup"><span data-stu-id="8ad01-120">Successful attacks can result in the attacker being able to run code within the context of the target process.</span></span>

<span data-ttu-id="8ad01-121">Stellen Sie sich als eine einfachere Analogie vor, dass das Aufrufen von `BinaryFormatter.Deserialize` für eine Nutzlast der Interpretation dieser Nutzlast als eigenständige ausführbare Datei und deren Starten entspricht.</span><span class="sxs-lookup"><span data-stu-id="8ad01-121">As a simpler analogy, assume that calling `BinaryFormatter.Deserialize` over a payload is the equivalent of interpreting that payload as a standalone executable and launching it.</span></span>

## <a name="binaryformatter-security-vulnerabilities"></a><span data-ttu-id="8ad01-122">Sicherheitsrisiken bei der Verwendung von BinaryFormatter</span><span class="sxs-lookup"><span data-stu-id="8ad01-122">BinaryFormatter security vulnerabilities</span></span>

> [!WARNING]
> <span data-ttu-id="8ad01-123">Die Methode `BinaryFormatter.Deserialize` ist __nie__ sicher, wenn sie mit nicht vertrauenswürdigen Eingaben verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8ad01-123">The `BinaryFormatter.Deserialize` method is __never__ safe when used with untrusted input.</span></span> <span data-ttu-id="8ad01-124">Es wird dringend empfohlen, dass Benutzer stattdessen die Verwendung einer der weiter unten in diesem Artikel beschriebenen Alternativen in Betracht ziehen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-124">We strongly recommend that consumers instead consider using one of the alternatives outlined later in this article.</span></span>

<span data-ttu-id="8ad01-125">`BinaryFormatter` wurde implementiert, als Sicherheitsrisiken im Zusammenhang mit der Deserialisierung noch keine so gut erforschte Bedrohungskategorie waren wie heutzutage.</span><span class="sxs-lookup"><span data-stu-id="8ad01-125">`BinaryFormatter` was implemented before deserialization vulnerabilities were a well-understood threat category.</span></span> <span data-ttu-id="8ad01-126">Folglich entspricht der Code nicht modernen Best Practices.</span><span class="sxs-lookup"><span data-stu-id="8ad01-126">As a result, the code does not follow modern best practices.</span></span> <span data-ttu-id="8ad01-127">Die Methode `Deserialize` kann von Angreifern für DoS-Angriffe auf Apps, die diese Methode nutzen, als Vektor verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-127">The `Deserialize` method can be used as a vector for attackers to perform DoS attacks against consuming apps.</span></span> <span data-ttu-id="8ad01-128">Diese Angriffe können dafür sorgen, dass die App nicht mehr reagiert, oder zu einer unerwarteten Prozessbeendigung führen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-128">These attacks might render the app unresponsive or result in unexpected process termination.</span></span> <span data-ttu-id="8ad01-129">Bei dieser Art von Angriff kann das Risiko nicht über `SerializationBinder` oder einen anderen `BinaryFormatter`-Konfigurationsparameter gemindert werden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-129">This category of attack cannot be mitigated with a `SerializationBinder` or any other `BinaryFormatter` configuration switch.</span></span> <span data-ttu-id="8ad01-130">Dieses Verhalten wird von .NET als ***strukturbedingt*** angesehen, und es wird keine Aktualisierung für den Code zur Änderung dieses Verhaltens veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="8ad01-130">.NET considers this behavior to be ***by design*** and won't issue a code update to modify the behavior.</span></span>

<span data-ttu-id="8ad01-131">`BinaryFormatter.Deserialize` ist möglicherweise auch für andere Arten von Angriffen anfällig, z. B. die Veröffentlichung von Informationen oder die Remoteausführung von Code.</span><span class="sxs-lookup"><span data-stu-id="8ad01-131">`BinaryFormatter.Deserialize` may be vulnerable to other attack categories, such as information disclosure or remote code execution.</span></span> <span data-ttu-id="8ad01-132">Die Verwendung von Features wie einer benutzerdefinierten <xref:System.Runtime.Serialization.SerializationBinder>-Klasse ist möglicherweise unzureichend, um diese Risiken ordnungsgemäß zu mindern.</span><span class="sxs-lookup"><span data-stu-id="8ad01-132">Utilizing features such as a custom <xref:System.Runtime.Serialization.SerializationBinder> may be insufficient to properly mitigate these risks.</span></span> <span data-ttu-id="8ad01-133">Es besteht die Möglichkeit, dass ein neues Sicherheitsrisiko entdeckt wird, für das .NET praktisch kein Sicherheitsupdate veröffentlichen kann.</span><span class="sxs-lookup"><span data-stu-id="8ad01-133">The possibility exists that a novel vulnerability will be discovered for which .NET cannot practically publish a security update.</span></span> <span data-ttu-id="8ad01-134">Benutzer sollten ihre individuellen Szenarios bewerten und die potenzielle Gefahr berücksichtigen, die für sie von diesen Risiken ausgeht.</span><span class="sxs-lookup"><span data-stu-id="8ad01-134">Consumers should assess their individual scenarios and consider their potential exposure to these risks.</span></span>

<span data-ttu-id="8ad01-135">Es wird empfohlen, dass Benutzer von `BinaryFormatter` individuelle Risikobewertungen für ihre Apps vornehmen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-135">We recommend that `BinaryFormatter` consumers perform individual risk assessments on their apps.</span></span> <span data-ttu-id="8ad01-136">Es liegt in der alleinigen Verantwortung des Benutzers, zu entscheiden, ob er `BinaryFormatter`verwendet.</span><span class="sxs-lookup"><span data-stu-id="8ad01-136">It is the consumer's sole responsibility to determine whether to utilize `BinaryFormatter`.</span></span> <span data-ttu-id="8ad01-137">Benutzer sollten Risiken hinsichtlich der Sicherheitsanforderungen sowie technischen, reputationsbezogenen, rechtlichen und aufsichtsrechtlichen Anforderungen für die Verwendung von `BinaryFormatter` bewerten.</span><span class="sxs-lookup"><span data-stu-id="8ad01-137">Consumers should risk assess the security, technical, reputation, legal, and regulatory requirements of using `BinaryFormatter`.</span></span>

## <a name="preferred-alternatives"></a><span data-ttu-id="8ad01-138">Bevorzugte Alternativen</span><span class="sxs-lookup"><span data-stu-id="8ad01-138">Preferred alternatives</span></span>

<span data-ttu-id="8ad01-139">.NET bietet mehrere integrierte Serialisierungsmodule, die nicht vertrauenswürdige Daten sicher verarbeiten können:</span><span class="sxs-lookup"><span data-stu-id="8ad01-139">.NET offers several in-box serializers that can handle untrusted data safely:</span></span>

* <span data-ttu-id="8ad01-140">Mit <xref:System.Xml.Serialization.XmlSerializer> und <xref:System.Runtime.Serialization.DataContractSerializer> können Objektgraphen in und aus XML serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-140"><xref:System.Xml.Serialization.XmlSerializer> and <xref:System.Runtime.Serialization.DataContractSerializer> to serialize object graphs into and from XML.</span></span> <span data-ttu-id="8ad01-141">Verwechseln Sie `DataContractSerializer` allerdings nicht mit <xref:System.Runtime.Serialization.NetDataContractSerializer>.</span><span class="sxs-lookup"><span data-stu-id="8ad01-141">Do not confuse `DataContractSerializer` with  <xref:System.Runtime.Serialization.NetDataContractSerializer>.</span></span>
* <span data-ttu-id="8ad01-142"><xref:System.IO.BinaryReader> und <xref:System.IO.BinaryWriter> für XML und JSON</span><span class="sxs-lookup"><span data-stu-id="8ad01-142"><xref:System.IO.BinaryReader> and <xref:System.IO.BinaryWriter> for XML and JSON.</span></span>
* <span data-ttu-id="8ad01-143">Die <xref:System.Text.Json>-APIs zum Serialisieren von Objektgraphen in JSON</span><span class="sxs-lookup"><span data-stu-id="8ad01-143">The <xref:System.Text.Json> APIs to serialize object graphs into JSON.</span></span>

## <a name="dangerous-alternatives"></a><span data-ttu-id="8ad01-144">Gefährliche Alternativen</span><span class="sxs-lookup"><span data-stu-id="8ad01-144">Dangerous alternatives</span></span>

<span data-ttu-id="8ad01-145">Verwenden Sie nicht die folgenden Serialisierungsmodule:</span><span class="sxs-lookup"><span data-stu-id="8ad01-145">Avoid the following serializers:</span></span>

* <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
* <xref:System.Web.UI.LosFormatter>
* <xref:System.Runtime.Serialization.NetDataContractSerializer>
* <xref:System.Web.UI.ObjectStateFormatter>

<span data-ttu-id="8ad01-146">Die obigen Serialisierungsmodule führen alle eine uneingeschränkte polymorphe Deserialisierung durch und sind gefährlich, genau wie `BinaryFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8ad01-146">The preceding serializers all perform unrestricted polymorphic deserialization and are dangerous, just like `BinaryFormatter`.</span></span>

## <a name="the-risks-of-assuming-data-to-be-trustworthy"></a><span data-ttu-id="8ad01-147">Risiken aufgrund der Annahme der Vertrauenswürdigkeit von Daten</span><span class="sxs-lookup"><span data-stu-id="8ad01-147">The risks of assuming data to be trustworthy</span></span>

<span data-ttu-id="8ad01-148">Häufig sind App-Entwickler möglicherweise der Meinung, dass sie nur vertrauenswürdige Eingaben verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="8ad01-148">Frequently, an app developer might believe that they are processing only trusted input.</span></span> <span data-ttu-id="8ad01-149">Eine sichere Eingabe ist nur in ein paar wenigen Fällen gegeben.</span><span class="sxs-lookup"><span data-stu-id="8ad01-149">The safe input case is true in some rare circumstances.</span></span> <span data-ttu-id="8ad01-150">Viel häufiger ist es so, dass eine Nutzlast die Vertrauenswürdigkeitsgrenze überschreitet, ohne dass dies dem Entwickler bewusst ist.</span><span class="sxs-lookup"><span data-stu-id="8ad01-150">But it's much more common that a payload crosses a trust boundary without the developer realizing it.</span></span>

<span data-ttu-id="8ad01-151">__Stellen Sie sich einen lokalen Server vor__, bei dem Mitarbeiter einen Desktopclient auf ihren Arbeitsstationen verwenden, um mit dem Dienst zu interagieren.</span><span class="sxs-lookup"><span data-stu-id="8ad01-151">__Consider an on-prem server__ where employees use a desktop client from their workstations to interact with the service.</span></span> <span data-ttu-id="8ad01-152">Dieses Szenario kann naiv als „sicheres“ Setup angesehen werden, bei dem die Verwendung von `BinaryFormatter` akzeptabel ist.</span><span class="sxs-lookup"><span data-stu-id="8ad01-152">This scenario might be seen naïvely as a "safe" setup where utilizing `BinaryFormatter` is acceptable.</span></span> <span data-ttu-id="8ad01-153">Es stellt jedoch einen Vektor für Schadsoftware dar, die sich Zugriff auf den Computer eines einzelnen Mitarbeiters verschafft, um sich dann im gesamten Unternehmen verteilen zu können.</span><span class="sxs-lookup"><span data-stu-id="8ad01-153">However, this scenario presents a vector for malware that gains access to a single employee's machine to be able to spread throughout the enterprise.</span></span> <span data-ttu-id="8ad01-154">Diese Schadsoftware kann die Tatsache, dass das Unternehmen `BinaryFormatter` verwendet, nutzen, um lateral von der Arbeitsstation des Mitarbeiters zum Back-End-Server zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="8ad01-154">That malware can leverage the enterprise's use of `BinaryFormatter` to move laterally from the employee's workstation to the backend server.</span></span> <span data-ttu-id="8ad01-155">Sie kann vertrauliche Daten des Unternehmens herausschleusen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-155">It can then exfiltrate the company's sensitive data.</span></span> <span data-ttu-id="8ad01-156">Diese Daten könnten Geschäftsgeheimnisse oder Kundendaten enthalten.</span><span class="sxs-lookup"><span data-stu-id="8ad01-156">Such data could include trade secrets or customer data.</span></span>

<span data-ttu-id="8ad01-157">__Stellen Sie sich außerdem eine App vor, die `BinaryFormatter` zum Beibehalten des Speicherstatus verwendet.__</span><span class="sxs-lookup"><span data-stu-id="8ad01-157">__Consider also an app that uses `BinaryFormatter` to persist save state.__</span></span> <span data-ttu-id="8ad01-158">Auf den ersten Blick wirkt dies möglicherweise wie ein sicheres Szenario, da das Lesen und Schreiben von Daten auf Ihrer eigenen Festplatte eine nur geringfügige Bedrohung darstellt.</span><span class="sxs-lookup"><span data-stu-id="8ad01-158">This might at first seem to be a safe scenario, as reading and writing data on your own hard drive represents a minor threat.</span></span> <span data-ttu-id="8ad01-159">Das Teilen von Dokumenten per E-Mail oder über das Internet ist jedoch üblich, und die meisten Endbenutzer würden das Öffnen dieser heruntergeladenen Dateien nicht als riskantes Verhalten ansehen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-159">However, sharing documents across email or the internet is common, and most end users wouldn't perceive opening these downloaded files as risky behavior.</span></span>

<span data-ttu-id="8ad01-160">Dieses Szenario kann für bösartige Zwecke genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-160">This scenario can be leveraged to nefarious effect.</span></span> <span data-ttu-id="8ad01-161">Wenn es sich bei der App um ein Spiel handelt, gefährden sich Benutzer, die sichere Dateien teilen, unwissentlich selbst.</span><span class="sxs-lookup"><span data-stu-id="8ad01-161">If the app is a game, users who share save files unknowingly place themselves at risk.</span></span> <span data-ttu-id="8ad01-162">Die Entwickler selbst können ebenfalls Ziel eines Angriffs werden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-162">The developers themselves can also be targeted.</span></span> <span data-ttu-id="8ad01-163">Der Angreifer könnte eine E-Mail an den technischen Support des Entwicklers senden, eine schädliche Datendatei anfügen und das Supportpersonal bitten, diese zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-163">The attacker might email the developers' tech support, attaching a malicious data file and asking the support staff to open it.</span></span> <span data-ttu-id="8ad01-164">Diese Art von Angriff könnte dem Angreifer Zugriff auf Unternehmensressourcen verschaffen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-164">This kind of attack could give the attacker a foothold in the enterprise.</span></span>

<span data-ttu-id="8ad01-165">In einem anderen Szenario wird die Datendatei im Cloudspeicher gespeichert und automatisch zwischen den Computern des Benutzers synchronisiert.</span><span class="sxs-lookup"><span data-stu-id="8ad01-165">Another scenario is where the data file is stored in cloud storage and automatically synced between the user's machines.</span></span> <span data-ttu-id="8ad01-166">Ein Angreifer, der Zugriff auf das Cloudspeicherkonto erlangen kann, kann aus der Datendatei eine schädliche Datei machen.</span><span class="sxs-lookup"><span data-stu-id="8ad01-166">An attacker who is able to gain access to the cloud storage account can poison the data file.</span></span> <span data-ttu-id="8ad01-167">Diese Datendatei wird dann automatisch mit den Computern des Benutzers synchronisiert.</span><span class="sxs-lookup"><span data-stu-id="8ad01-167">This data file will be automatically synced to the user's machines.</span></span> <span data-ttu-id="8ad01-168">Wenn der Benutzer die Datendatei das nächste Mal öffnet, wird die Nutzlast des Angreifers ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8ad01-168">The next time the user opens the data file, the attacker's payload runs.</span></span> <span data-ttu-id="8ad01-169">Daher kann der Angreifer eine Kompromittierung des Cloudspeicherkontos nutzen, um vollständige Codeausführungsberechtigungen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="8ad01-169">Thus the attacker can leverage a cloud storage account compromise to gain full code execution permissions.</span></span>

<span data-ttu-id="8ad01-170">__Stellen Sie sich eine App vor, die von einem Desktopinstallationsmodell zu einem Cloud-First-Modell wechselt.__</span><span class="sxs-lookup"><span data-stu-id="8ad01-170">__Consider an app that moves from a desktop-install model to a cloud-first model.__</span></span> <span data-ttu-id="8ad01-171">Dieses Szenario umfasst Apps, die von einer Desktop-App oder einem Rich-Client-Modell zu einem webbasierten Modell wechseln.</span><span class="sxs-lookup"><span data-stu-id="8ad01-171">This scenario includes apps that move from a desktop app or rich client model into a web-based model.</span></span> <span data-ttu-id="8ad01-172">Bedrohungsmodelle für die Desktop-App sind nicht notwendigerweise auf den cloudbasierten Dienst anwendbar.</span><span class="sxs-lookup"><span data-stu-id="8ad01-172">Any threat models drawn for the desktop app aren't necessarily applicable to the cloud-based service.</span></span> <span data-ttu-id="8ad01-173">Möglicherweise stuft das Bedrohungsmodell für die Desktop-App eine bestimmte Bedrohung als „für den Client, der angegriffen werden soll, nicht relevant“ ein.</span><span class="sxs-lookup"><span data-stu-id="8ad01-173">The threat model for the desktop app might dismiss a given threat as "not interesting for the client to attack itself."</span></span> <span data-ttu-id="8ad01-174">Dieselbe Bedrohung kann jedoch sehr wohl relevant werden, wenn angenommen wird, dass ein Remotebenutzer (der Client) den Clouddienst selbst angreift.</span><span class="sxs-lookup"><span data-stu-id="8ad01-174">But that same threat might become interesting when it considers a remote user (the client) attacking the cloud service itself.</span></span>

> [!NOTE]
> <span data-ttu-id="8ad01-175">Allgemein ist der Zweck der Serialisierung das Übertragen eines Objekts in eine oder aus einer App.</span><span class="sxs-lookup"><span data-stu-id="8ad01-175">In general terms, the intent of serialization is to transmit an object into or out of an app.</span></span> <span data-ttu-id="8ad01-176">Bei Übungen zur Bedrohungsmodellierung wird diese Art von Datenübertragung fast immer als Überschreiten der Vertrauenswürdigkeitsgrenze eingestuft.</span><span class="sxs-lookup"><span data-stu-id="8ad01-176">A threat modeling exercise almost always marks this kind of data transfer as crossing a trust boundary.</span></span>

## <a name="further-resources"></a><span data-ttu-id="8ad01-177">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="8ad01-177">Further resources</span></span>

* <span data-ttu-id="8ad01-178">[YSoSerial.Net:](https://github.com/pwntester/ysoserial.net) Erfahren Sie mehr darüber, wie Angreifer Apps angreifen, die `BinaryFormatter` verwenden.</span><span class="sxs-lookup"><span data-stu-id="8ad01-178">[YSoSerial.Net](https://github.com/pwntester/ysoserial.net) for research into how adversaries attack apps that utilize `BinaryFormatter`.</span></span>
* <span data-ttu-id="8ad01-179">[Bedrohungsmodellierung:](/securityengineering/sdl/threatmodeling) Informationen zu Apps und Diensten für die Bedrohungsmodellierung</span><span class="sxs-lookup"><span data-stu-id="8ad01-179">[Threat Modeling](/securityengineering/sdl/threatmodeling) for information on threat modeling apps and services.</span></span>
* <span data-ttu-id="8ad01-180">Allgemeine Hintergrundinformationen zu Sicherheitsrisiken im Zusammenhang mit der Deserialisierung:</span><span class="sxs-lookup"><span data-stu-id="8ad01-180">General background on deserialization vulnerabilities:</span></span>
  * [<span data-ttu-id="8ad01-181">OWASP Top 10: A8:2017 – Unsichere Deserialisierung</span><span class="sxs-lookup"><span data-stu-id="8ad01-181">OWASP Top 10 - A8:2017-Insecure Deserialization</span></span>](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A8-Insecure_Deserialization)
  * [<span data-ttu-id="8ad01-182">CWE-502: Deserialisierung von nicht vertrauenswürdigen Daten</span><span class="sxs-lookup"><span data-stu-id="8ad01-182">CWE-502: Deserialization of Untrusted Data</span></span>](https://cwe.mitre.org/data/definitions/502.html)