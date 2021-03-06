---
title: Suchen des minimalen Werts in einer numerischen Sequenz
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 78203093-f242-4572-9b31-9495b10926aa
ms.openlocfilehash: d8aee43f13ec92f649b4df20505ac56c336fe07a
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2019
ms.locfileid: "70793833"
---
# <a name="find-the-minimum-value-in-a-numeric-sequence"></a>Suchen des minimalen Werts in einer numerischen Sequenz
Verwenden Sie den <xref:System.Linq.Enumerable.Min%2A>-Operator, um den minimalen Wert einer Sequenz numerischer Werte zurückzugeben.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird der niedrigste Einzelpreis aller Produkte gesucht.  
  
 Wenn Sie diese Abfrage mit der Beispieldatenbank Northwind ausführen, lautet die Ausgabe `2.5000`.  
  
 [!code-csharp[DLinqQueryExamples#9](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#9)]
 [!code-vb[DLinqQueryExamples#9](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#9)]  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird die niedrigste Frachtmenge aller Bestellungen gesucht.  
  
 Wenn Sie diese Abfrage mit der Beispieldatenbank Northwind ausführen, lautet die Ausgabe `0.0200`.  
  
 [!code-csharp[DLinqQueryExamples#10](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#10)]
 [!code-vb[DLinqQueryExamples#10](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#10)]  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel werden mit Min die `Products` (Produkte) ermittelt, die in jeder Kategorie den geringsten Einzelpreis aufweisen. Die Ausgabe wird nach Kategorie sortiert.  
  
 [!code-csharp[DLinqQueryExamples#11](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#11)]
 [!code-vb[DLinqQueryExamples#11](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#11)]  
  
 Wenn Sie die vorherige Abfrage mit der Beispieldatenbank Northwind ausführen, sehen die Ergebnisse wie folgt aus:  
  
 `1`  
  
 `Guaraná Fantástica`  
  
 `2`  
  
 `Aniseed Syrup`  
  
 `3`  
  
 `Teatime Chocolate Biscuits`  
  
 `4`  
  
 `Geitost`  
  
 `5`  
  
 `Filo Mix`  
  
 `6`  
  
 `Tourtière`  
  
 `7`  
  
 `Longlife Tofu`  
  
 `8`  
  
 `Konbu`  
  
## <a name="see-also"></a>Siehe auch

- [Aggregatabfragen](aggregate-queries.md)
- [Downloading Sample Databases (Herunterladen von Beispieldatenbanken)](downloading-sample-databases.md)
