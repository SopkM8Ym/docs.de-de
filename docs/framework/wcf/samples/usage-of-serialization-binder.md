---
title: Verwendung des Serialisierungsbinders
ms.date: 03/30/2017
ms.assetid: ab46c087-200c-45bf-9c95-5a6cda6e8b98
ms.openlocfilehash: bfce2a14c8757250c520919c8ff2a4d7048a9d5c
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74138650"
---
# <a name="usage-of-serialization-binder"></a>Verwendung des Serialisierungsbinders
In diesem Beispiel wird gezeigt, wie der <xref:System.Runtime.Serialization.SerializationBinder> verwendet wird, um die Version eines generischen Typs zu ändern, während diese serialisiert wird.  
  
## <a name="demonstrates"></a>Veranschaulicht  
 <xref:System.Runtime.Serialization.SerializationBinder>, <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>  
  
## <a name="discussion"></a>Diskussion  
 Dieses Beispiel zeigt, wie zwei Entitäten, die auf unterschiedliche Versionen des .NET Framework abzielen, mit dem binären Formatierer und dem Serialisierungsbinder kommunizieren können.  
  
Dieses Beispiel wurde mithilfe von .NET-Remoting entwickelt. Sie besteht aus einem Server, der auf .NET Framework 4 abzielt, der einen Vertrag mit generischen Typen implementiert, und zwei verschiedenen Clients, von denen eine als Ziel .NET Framework 2,0 und eine andere als Ziel .NET Framework 4 ist.  
  
 Vom Server wird ein <xref:System.Runtime.Serialization.SerializationBinder> an den binären Formatierungsprogramm angehängt, damit die Versionen der Typen entsprechend bei der Serialisierung geändert werden können. Deshalb können beide Clients diese Typen ordnungsgemäß deserialisieren.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>So richten Sie das Beispiel ein, erstellen es und führen es aus  
  
1. Um den Client auszuführen, klicken Sie mit der rechten Maustaste auf die Projekt Mappe sbgenericsvts (6 Projekte), und wählen Sie dann **Eigenschaften**aus.  
  
2. Wählen Sie unter **Allgemeine Eigenschaften**die Option **Startprojekt**aus, und wählen Sie dann **mehrere Start Projekte**aus.  
  
3. Wählen Sie zuerst **Server** , dann **Client20** und dann **Client40**aus. Wählen Sie die Aktion **Start** für diese drei Projekte aus, und lassen Sie den Rest auf **keine**festgelegt.  
  
4. Klicken Sie auf **OK** , und drücken Sie dann F5, um das Beispiel auszuführen.
