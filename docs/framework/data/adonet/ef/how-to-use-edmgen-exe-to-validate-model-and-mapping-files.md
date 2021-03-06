---
title: 'Vorgehensweise: Überprüfen von Modell- und Zuordnungsdateien mit „EdmGen.exe“'
ms.date: 03/30/2017
ms.assetid: 2641906a-971a-4d0b-8aee-13fabc02a1cc
ms.openlocfilehash: a5e3124eb907b8077df7db4d71240f6e6b7bae63
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90544504"
---
# <a name="how-to-use-edmgenexe-to-validate-model-and-mapping-files"></a>Vorgehensweise: Überprüfen von Modell- und Zuordnungsdateien mit „EdmGen.exe“
In diesem Thema wird gezeigt, wie Sie das Tool [EDM Generator (EdmGen.exe)](edm-generator-edmgen-exe.md) zum Überprüfen der Modell-und Zuordnungsdateien verwenden. Weitere Informationen finden Sie unter [Entity Data Model](../entity-data-model.md).  
  
### <a name="to-validate-the-school-model-using-edmgenexe"></a>So validieren Sie das Modell 'School' mit 'EdmGen.exe'  
  
1. Erstellen der Datenbank "School". Weitere Informationen finden Sie unter [Erstellen der Beispieldatenbank "School](/previous-versions/dotnet/netframework-4.0/bb399731(v=vs.100))".  
  
2. Erstellen des Modells "School" Weitere Informationen finden Sie unter Gewusst [wie: Verwenden von EdmGen.exe zum Generieren von Modell-und Mapping-Dateien](how-to-use-edmgen-exe-to-generate-the-model-and-mapping-files.md).  
  
3. Führen Sie an der Eingabeaufforderung den folgenden Befehl ohne Zeilenumbrüche aus:  
  
    ```console
    "%windir%\Microsoft.NET\Framework\v4.0.30319\edmgen.exe" /mode:ValidateArtifacts /inssdl:.\School.ssdl /inmsl:.\School.msl /incsdl:.\School.csdl  
    ```  
  
## <a name="see-also"></a>Siehe auch

- [Gewusst wie: Manuelles Konfigurieren eines Entity Framework-Projekts](/previous-versions/dotnet/netframework-4.0/bb738546(v=vs.100))
- [ADO.NET-Entity Data Model Tools](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
- [Gewusst wie: Vorgenerieren von Ansichten zur Verbesserung der Abfrageleistung](/previous-versions/dotnet/netframework-4.0/bb896240(v=vs.100))
- [Vorgehensweise: Generieren von Objektebenencode mit „EdmGen.exe“](how-to-use-edmgen-exe-to-generate-object-layer-code.md)
