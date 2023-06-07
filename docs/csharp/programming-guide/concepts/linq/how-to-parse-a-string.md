---
title: Como analisar uma cadeia de caracteres (C#)
description: Saiba como analisar uma cadeia de caracteres para criar uma árvore XML em C#. Saiba como acessar dados específicos em seu XML analisado.
ms.date: 07/20/2015
ms.assetid: 81e5686c-9658-42d8-a7e3-b11be0a2c98b
ms.openlocfilehash: a4664e090b6a44c52c519e61b66ccdc5d59a71f1
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104817"
---
# <a name="how-to-parse-a-string-c"></a>Como analisar uma cadeia de caracteres (C#)

Este tópico mostra como analisar uma cadeia de caracteres para criar uma árvore XML no C#.

## <a name="example"></a>Exemplo

O código C# a seguir mostra como analisar uma cadeia de caracteres XML:

```csharp
XElement contacts = XElement.Parse(
    @"<Contacts>
        <Contact>
            <Name>Patrick Hines</Name>
            <Phone Type=""home"">206-555-0144</Phone>
            <Phone Type=""work"">425-555-0145</Phone>
            <Address>
            <Street1>123 Main St</Street1>
            <City>Mercer Island</City>
            <State>WA</State>
            <Postal>68042</Postal>
            </Address>
            <NetWorth>10</NetWorth>
        </Contact>
        <Contact>
            <Name>Gretchen Rivas</Name>
            <Phone Type=""mobile"">206-555-0163</Phone>
            <Address>
            <Street1>123 Main St</Street1>
            <City>Mercer Island</City>
            <State>WA</State>
            <Postal>68042</Postal>
            </Address>
            <NetWorth>11</NetWorth>
        </Contact>
    </Contacts>");
Console.WriteLine(contacts);
```

O `Contacts` nó raiz tem dois `Contact` nós. Para acessar alguns dados específicos em seu XML analisado, use o método [XElement. Elements ()](xref:System.Xml.Linq.XContainer.Elements) , que, nesse caso, retorna os elementos filho do `Contacts` nó raiz. O exemplo a seguir imprime o primeiro `Contact` nó no console:

```csharp
List<XElement> contactNodes = contacts.Elements("Contact").ToList();
Console.WriteLine(contactNodes[0]);
```

## <a name="see-also"></a>Confira também

- [Como localizar um elemento com um atributo específico (C#)](how-to-find-an-element-with-a-specific-attribute.md)
