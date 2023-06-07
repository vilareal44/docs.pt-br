---
title: Alterando propriedades de prefixo de namespace
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: d5c87cbe-4d69-429f-aad5-3103c2ca2770
ms.openlocfilehash: b817a68ff9789be414118ff4c1a3d88ca3ea9f01
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290909"
---
# <a name="changing-namespace-prefix-properties"></a>Alterando propriedades de prefixo de namespace
A classe **XmlNode** permite que você modifique o prefixo de namespace associado a um nó específico. Por exemplo, o código a seguir mostra o prefixo de um elemento que está sendo alterado.  
  
```vb  
Dim doc as XmlDocument = new XmlDocument()  
doc.LoadXml("<a:test xmlns:a='123' xmlns:b='456'/>")  
Dim e as XmlElement = doc.DocumentElement  
e.Prefix = "b"  
Console.WriteLine(doc.InnerXml)  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
doc.LoadXml("<a:test xmlns:a='123' xmlns:b='456'/>");  
XmlElement e = doc.DocumentElement;
e.Prefix = "b";  
Console.WriteLine(doc.InnerXml);  
```  
  
 **Saída**  
  
```xml  
<b:test xmlns:a="123" xmlns:b="456" />  
```  
  
 Altere o prefixo de um nó não altera seu namespace. O namespace somente pode ser definida quando o nó é criado. Quando você mantém a árvore, novos atributos de namespace podem ser persistentes para fora para satisfazer o prefixo você definir. Se o novo namespace não pode ser criada, então o prefixo é alterado para que as conservas do nó seu nome e local namespace. O exemplo a seguir mostra um atributo do namespace sendo adicionado.  
  
```vb  
Dim doc as XmlDocument = new XmlDocument()  
doc.LoadXml("<test xmlns='123'/>")  
Dim e as XmlElement = doc.DocumentElement  
e.Prefix = "a"  
Console.WriteLine(doc.InnerXml)  
```  
  
```csharp  
XmlDocument doc = new XmlDocument();  
doc.LoadXml("<test xmlns='123'/>");  
XmlElement e = doc.DocumentElement;
e.Prefix = "a";  
Console.WriteLine(doc.InnerXml);  
```  
  
 **Saída**  
  
```xml  
<a:test xmlns="123" xmlns:a="123" />  
```  
  
 Quando a árvore foi mantida em uma cadeia de caracteres como resultado da chamada a **doc.InnerXml**, o atributo `xmlns:a='123'` foi adicionado para preservar o namespace do elemento `test`. Foi `'123'`, e permanece `'123'`.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
