---
title: Operador GetXmlNamespace
ms.date: 07/20/2015
f1_keywords:
- vb.GetXmlNamespace
- GetXmlNamespace
helpviewer_keywords:
- GetXmlNamespace operator [Visual Basic]
- GetXmlNamespace keyword [Visual Basic]
ms.assetid: d0d28cfd-0755-4896-ae0b-4981aa35517c
ms.openlocfilehash: 85422fb9e11d78e228577adc25cba746149c645a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371110"
---
# <a name="getxmlnamespace-operator-visual-basic"></a>Operador GetXmlNamespace (Visual Basic)
Obtém o <xref:System.Xml.Linq.XNamespace> objeto que corresponde ao prefixo de namespace XML especificado.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
GetXmlNamespace(xmlNamespacePrefix)  
```  
  
## <a name="parts"></a>Partes  
 `xmlNamespacePrefix`  
 Opcional. A cadeia de caracteres que identifica o prefixo do namespace XML. Se fornecido, essa cadeia de caracteres deve ser um identificador XML válido. Para obter mais informações, consulte [nomes de elementos XML declarados e atributos](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md). Se nenhum prefixo for especificado, o namespace padrão será retornado. Se nenhum namespace padrão for especificado, o namespace vazio será retornado.  
  
## <a name="return-value"></a>Valor Retornado  
 O <xref:System.Xml.Linq.XNamespace> objeto que corresponde ao prefixo do namespace XML.  
  
## <a name="remarks"></a>Comentários  
 O `GetXmlNamespace` operador Obtém o <xref:System.Xml.Linq.XNamespace> objeto que corresponde ao prefixo de namespace XML `xmlNamespacePrefix` .  
  
 Você pode usar prefixos de namespace XML diretamente em literais XML e propriedades de eixo XML. No entanto, você deve usar o `GetXmlNamespace` operador para converter um prefixo de namespace em um <xref:System.Xml.Linq.XNamespace> objeto antes de poder usá-lo em seu código. Você pode acrescentar um nome de elemento não qualificado a um <xref:System.Xml.Linq.XNamespace> objeto para obter um objeto totalmente qualificado <xref:System.Xml.Linq.XName> , que muitos [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] métodos exigem.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir importa `ns` como um prefixo de namespace XML. Em seguida, ele usa o prefixo do namespace para criar um literal XML e acessar o primeiro nó filho que tem o nome qualificado `ns:phone` . Em seguida, ele passa esse nó filho para a `ShowName` sub-rotina, que constrói um nome qualificado usando o `GetXmlNamespace` operador. `ShowName`Em seguida, a sub-rotina passa o nome qualificado para o <xref:System.Xml.Linq.XNode.Ancestors%2A> método para obter o `ns:contact` nó pai.  
  
 [!code-vb[VbXMLSamples#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/GetXmlNamespace.vb#38)]  
  
 Quando você chama `TestGetXmlNamespace.RunSample()` , ele exibe uma caixa de mensagem que contém o seguinte texto:  
  
 `Name: Patrick Hines`  
  
## <a name="see-also"></a>Confira também

- [Instrução Imports (namespace XML)](../statements/imports-statement-xml-namespace.md)
- [Acessando XML no Visual Basic](../../programming-guide/language-features/xml/accessing-xml.md)
