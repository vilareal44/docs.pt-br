---
title: Expressões inseridas no XML
ms.date: 07/20/2015
f1_keywords:
- vb.XmlEmbeddedExpression
helpviewer_keywords:
- embedded expressions [Visual Basic]
- LINQ to XML [Visual Basic], embedded expressions
- XML literals [Visual Basic], embedded expressions
ms.assetid: bf2eb779-b751-4b7c-854f-9f2161482352
ms.openlocfilehash: d4ff9442aa82a3eb46d56500159562174646ea58
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410251"
---
# <a name="embedded-expressions-in-xml-visual-basic"></a>Expressões inseridas no XML (Visual Basic)
As expressões inseridas permitem que você crie literais XML que contêm expressões que são avaliadas em tempo de execução. A sintaxe para uma expressão inserida é `<%=` `expression` `%>` , que é igual à sintaxe usada em ASP.net.  
  
 Por exemplo, você pode criar um literal de elemento XML, combinando expressões inseridas com conteúdo de texto literal.  
  
 [!code-vb[VbXMLSamples#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#27)]  
  
 Se `isbnNumber` contiver o inteiro 12345 e `modifiedDate` contiver a data 3/5/2006, quando esse código for executado, o valor de `book` será:  
  
```xml  
<book category="fiction" isbn="12345">  
  <modifiedDate>3/5/2006</modifiedDate>  
</book>  
```  
  
## <a name="embedded-expression-location-and-validation"></a>Local e validação da expressão inserida  
 As expressões inseridas podem aparecer apenas em determinados locais dentro de expressões literais XML. O local da expressão controla quais tipos a expressão pode retornar e como `Nothing` é manipulada. A tabela a seguir descreve os locais permitidos e os tipos de expressões inseridas.  
  
|Local em literal|Tipo de expressão|Manipulação de`Nothing`|  
|---|---|---|  
|Nome do elemento XML|<xref:System.Xml.Linq.XName>|Erro|  
|Conteúdo do elemento XML|`Object`ou matriz de`Object`|Ignored|  
|Nome do atributo do elemento XML|<xref:System.Xml.Linq.XName>|Erro, a menos que o valor do atributo também seja`Nothing`|  
|Valor de atributo de elemento XML|`Object`|Declaração de atributo ignorada|  
|Atributo de elemento XML|<xref:System.Xml.Linq.XAttribute>ou uma coleção de<xref:System.Xml.Linq.XAttribute>|Ignored|  
|Elemento raiz do documento XML|<xref:System.Xml.Linq.XElement>ou uma coleção de um <xref:System.Xml.Linq.XElement> objeto e um número arbitrário de <xref:System.Xml.Linq.XProcessingInstruction> <xref:System.Xml.Linq.XComment> objetos e|Ignored|  
  
- Exemplo de uma expressão incorporada em um nome de elemento XML:  
  
     [!code-vb[VbXMLSamples#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#32)]  
  
- Exemplo de uma expressão incorporada no conteúdo de um elemento XML:  
  
     [!code-vb[VbXMLSamples#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#33)]  
  
- Exemplo de uma expressão incorporada em um nome de atributo de elemento XML:  
  
     [!code-vb[VbXMLSamples#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#34)]  
  
- Exemplo de uma expressão incorporada em um valor de atributo de elemento XML:  
  
     [!code-vb[VbXMLSamples#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#35)]  
  
- Exemplo de uma expressão incorporada em um atributo de elemento XML:  
  
     [!code-vb[VbXMLSamples#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#36)]  
  
- Exemplo de uma expressão incorporada em um elemento raiz do documento XML:  
  
     [!code-vb[VbXMLSamples#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#37)]  
  
 Se você habilitar `Option Strict` , o compilador verificará se o tipo de cada expressão inserida se amplia ao tipo necessário. A única exceção é para o elemento raiz de um documento XML, que é verificado quando o código é executado. Se você compilar sem `Option Strict` , poderá inserir expressões do tipo `Object` e seu tipo será verificado em tempo de execução.  
  
 Em locais onde o conteúdo é opcional, as expressões inseridas que contêm `Nothing` são ignoradas. Isso significa que você não precisa verificar se o conteúdo do elemento, os valores de atributo e os elementos da matriz não estão `Nothing` antes de usar um literal XML. Os valores necessários, como nomes de elementos e atributos, não podem ser `Nothing` .  
  
 Para obter mais informações sobre como usar uma expressão inserida em um tipo específico de literal, consulte [literal de documento XML](../../../language-reference/xml-literals/xml-document-literal.md), [literal de elemento XML](../../../language-reference/xml-literals/xml-element-literal.md).  
  
## <a name="scoping-rules"></a>Regras de escopo  
 O compilador converte cada literal XML em uma chamada de construtor para o tipo literal apropriado. O conteúdo literal e as expressões inseridas em um literal XML são passados como argumentos para o construtor. Isso significa que todos os elementos de programação de Visual Basic disponíveis para um literal XML também estão disponíveis para suas expressões inseridas.  
  
 Em um literal XML, você pode acessar os prefixos de namespace XML declarados com a `Imports` instrução. Você pode declarar um novo prefixo de namespace XML ou sombrear um prefixo de namespace XML existente, em um elemento usando o `xmlns` atributo. O novo namespace está disponível para os nós filho desse elemento, mas não para literais XML em expressões inseridas.  
  
> [!NOTE]
> Quando você declara um prefixo de namespace XML usando o `xmlns` atributo namespace, o valor do atributo deve ser uma cadeia de caracteres constante. Nesse sentido, usar o `xmlns` atributo é como usar a `Imports` instrução para declarar um namespace XML. Você não pode usar uma expressão inserida para especificar o valor do namespace XML.  
  
## <a name="see-also"></a>Confira também

- [Criando XML no Visual Basic](creating-xml.md)
- [Literal de Documento XML](../../../language-reference/xml-literals/xml-document-literal.md)
- [Literal do Elemento XML](../../../language-reference/xml-literals/xml-element-literal.md)
- [Instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)
- [Instrução Imports (tipo e namespace .NET)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [Visão geral dos literais XML](xml-literals-overview.md)
