---
title: Assemblyspeicherort
description: Der Speicherort einer .NET-Assembly bestimmt, wie die CLR sie findet, und ob sie für andere Assemblys freigegeben werden kann.
ms.date: 08/20/2019
helpviewer_keywords:
- locating assemblies
- assemblies [.NET Framework], location
ms.assetid: 9f1f41a7-2954-49d3-a2c0-62b6ef4d40ab
ms.openlocfilehash: 7ab3804b14b586e1430d654f4da32a310bcb6cc9
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2020
ms.locfileid: "83379897"
---
# <a name="assembly-location"></a>Assemblyspeicherort
Der Speicherort einer Assembly bestimmt, ob die Common Language Runtime diese finden kann, wenn auf sie verwiesen wird. Ebenso bestimmt er, ob die Assembly für andere Assemblys freigegeben werden kann. Sie können eine Assembly in den folgenden Speicherorten bereitstellen:

- Die Verzeichnisse und Unterverzeichnisse der Anwendung

     Dies ist der häufigste Speicherort für das Bereitstellen einer Assembly. Das Unterverzeichnis des Stammverzeichnisses einer Anwendung kann auf der Sprache oder der Kultur basieren. Wenn eine Assembly über Informationen im Kulturattribut verfügt, muss sie sich in einem Unterverzeichnis im Anwendungsverzeichnis mit dem Kulturnamen befinden.

- Im globalen Assemblycache.

     Dabei handelt es sich um einen computerweiten Codecache, der überall dort installiert wird, wo auch die Common Language Runtime installiert ist. Wenn Sie eine Assembly für mehrere Anwendungen freigeben möchten, sollten Sie die Assembly in den meisten Fällen im globalen Assemblycache bereitstellen.

- Auf einem HTTP-Server

     Eine auf einem HTTP-Server bereitgestellte Assembly muss einen starken Namen haben. Sie zeigen im Codebasisabschnitt Ihrer Anwendungskonfigurationsdatei auf die Assembly.

## <a name="see-also"></a>Siehe auch

- [Erstellen von Assemblys](create.md)
- [Globaler Assemblycache](../../framework/app-domains/gac.md)
- [So sucht Common Language Runtime nach Assemblys](../../framework/deployment/how-the-runtime-locates-assemblies.md)
