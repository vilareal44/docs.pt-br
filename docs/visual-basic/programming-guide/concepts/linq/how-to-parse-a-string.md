---
title: 'Como: analisar uma cadeia de caracteres'
ms.date: 07/20/2015
ms.assetid: 896e1b4b-f9bd-4975-8bc1-55b6badce1ac
ms.openlocfilehash: 0a9076fc516bb8e6bc74732ca252fabfeda43d53
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84398006"
---
# <a name="how-to-parse-a-string-visual-basic"></a>Como analisar uma cadeia de caracteres (Visual Basic)
Este tópico mostra como criar uma árvore XML em C#.  
  
## <a name="example"></a>Exemplo  
 Você pode analisar uma cadeia de caracteres em Visual Basic usando o `XElement.Parse` método. No entanto, é mais eficiente usar literais XML, como mostrado no código a seguir, pois os literais XML não sofrem das mesmas penalidades de desempenho que a análise XML de uma cadeia de caracteres.  
  
 Usando literais XML, você pode simplesmente copiar e colar o XML em seu programa Visual Basic.  
  
> [!NOTE]
> Analisar texto ou carregar um documento XML de um arquivo de texto é menos eficiente do que a construção funcional. Se você estiver inicializando uma árvore XML de código, o tempo do processador é menor para usar a construção funcional do que para analisar texto.  
  
```vb  
Dim contacts as XElement = _  
    <Contacts>  
        <Contact>  
            <Name>Patrick Hines</Name>  
            <Phone Type="home">206-555-0144</Phone>  
            <Phone Type="work">425-555-0145</Phone>  
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
            <Phone Type="mobile">206-555-0163</Phone>  
            <Address>  
            <Street1>123 Main St</Street1>  
            <City>Mercer Island</City>  
            <State>WA</State>  
            <Postal>68042</Postal>  
            </Address>  
            <NetWorth>11</NetWorth>  
        </Contact>  
    </Contacts>  
```  
  
## <a name="see-also"></a>Confira também

- [Analisando XML (Visual Basic)](parsing-xml.md)
