---
title: Serializzazione in base a File, TextWriter e XmlWriter1
ms.date: 07/20/2015
ms.assetid: bd3ea6f7-895b-4ff4-a625-fe2bb55b1886
ms.openlocfilehash: c74dc7f429e4ae27f08f7acb6b3a6c39161aac71
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66483542"
---
# <a name="serializing-to-files-textwriters-and-xmlwriters"></a>Serializzazione in base a File, TextWriter e XmlWriter

È possibile serializzare gli alberi XML in un oggetto <xref:System.IO.File>, <xref:System.IO.TextWriter> o <xref:System.Xml.XmlWriter>.

È possibile serializzare qualsiasi componente XML, incluso <xref:System.Xml.Linq.XDocument> e <xref:System.Xml.Linq.XElement>, in una stringa usando il metodo `ToString`.

Per eliminare la formattazione quando si esegue la serializzazione in una stringa, usare il metodo <xref:System.Xml.Linq.XNode.ToString%2A?displayProperty=nameWithType>.

Il comportamento predefinito quando si esegue la serializzazione in un file implica la formattazione, ovvero l'impostazione di rientri, nel documento XML risultante. Quando si impostano i rientri, lo spazio vuoto non significativo nell'albero XML non viene conservato. Per serializzare e formattare al contempo il documento, usare uno degli overload dei metodi seguenti che non accettano <xref:System.Xml.Linq.SaveOptions> come argomento:

- <xref:System.Xml.Linq.XDocument.Save%2A?displayProperty=nameWithType>

- <xref:System.Xml.Linq.XElement.Save%2A?displayProperty=nameWithType>

Se si desidera applicare l'opzione per non impostare i rientri e conservare lo spazio vuoto non significativo nell'albero XML, usare uno degli overload dei metodi seguenti che accettano <xref:System.Xml.Linq.SaveOptions> come argomento:

- <xref:System.Xml.Linq.XDocument.Save%2A?displayProperty=nameWithType>

- <xref:System.Xml.Linq.XElement.Save%2A?displayProperty=nameWithType>

Per gli esempi, vedere l'argomento di riferimento appropriato.

## <a name="see-also"></a>Vedere anche

- [Serializzazione di alberi XML (C#)](serializing-to-files-textwriters-and-xmlwriters.md)
