---
title: Propriedade de Eixo Descendente XML
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyDescendantsAxis
helpviewer_keywords:
- Visual Basic code, accessing XML
- XML descendant axis property [Visual Basic]
- descendant axis property [Visual Basic]
- XML axis [Visual Basic], descendant
- XML [Visual Basic], accessing
ms.assetid: a178f85b-5d54-438f-8479-40b62af6fe76
ms.openlocfilehash: 52544619171dbc7034baeb5feb61395d81096387
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400247"
---
# <a name="xml-descendant-axis-property-visual-basic"></a>Propriedade de eixo descendente XML (Visual Basic)

Fornece acesso aos descendentes dos seguintes: um <xref:System.Xml.Linq.XElement> objeto, um <xref:System.Xml.Linq.XDocument> objeto, uma coleção de <xref:System.Xml.Linq.XElement> objetos ou uma coleção de <xref:System.Xml.Linq.XDocument> objetos.

## <a name="syntax"></a>Sintaxe

```vb
object...<descendant>
```

## <a name="parts"></a>Partes

`object` Necessário. Um <xref:System.Xml.Linq.XElement> objeto, um <xref:System.Xml.Linq.XDocument> objeto, uma coleção de <xref:System.Xml.Linq.XElement> objetos ou uma coleção de <xref:System.Xml.Linq.XDocument> objetos.

`...<` Necessário. Denota o início de uma propriedade de eixo descendente.

`descendant` Necessário. Nome dos nós descendentes a serem acessados, no formato [ `prefix:]name` .

|Parte|Descrição|
|----------|-----------------|
|`prefix`|Opcional. Prefixo do namespace XML para o nó descendente. Deve ser um namespace XML global que é definido usando uma `Imports` instrução.|
|`name`|Obrigatórios. Nome local do nó descendente. Consulte [nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md).|

`>` Necessário. Denota o final de uma propriedade de eixo descendente.

## <a name="return-value"></a>Valor Retornado

Uma coleção de objetos <xref:System.Xml.Linq.XElement>.

## <a name="remarks"></a>Comentários

Você pode usar uma propriedade de eixo descendente XML para acessar nós descendentes por nome de um <xref:System.Xml.Linq.XElement> objeto ou ou <xref:System.Xml.Linq.XDocument> de uma coleção <xref:System.Xml.Linq.XElement> de <xref:System.Xml.Linq.XDocument> objetos ou. Use a `Value` propriedade XML para acessar o valor do primeiro nó descendente na coleção retornada. Para obter mais informações, consulte [propriedade de valor XML](xml-value-property.md).

O compilador Visual Basic converte Propriedades do eixo descendente em chamadas para o <xref:System.Xml.Linq.XContainer.Descendants%2A> método.

## <a name="xml-namespaces"></a>Namespaces XML

O nome em uma propriedade de eixo descendente pode usar somente namespaces XML declarados globalmente com a `Imports` instrução. Ele não pode usar namespaces XML declarados localmente em literais de elemento XML. Para obter mais informações, consulte [instrução Imports (namespace XML)](../statements/imports-statement-xml-namespace.md).

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como acessar o valor do primeiro nó descendente chamado `name` e os valores de todos os nós descendentes nomeados `phone` a partir do `contacts` objeto.

[!code-vb[VbXMLSamples#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples11.vb#25)]

Esse código exibe o seguinte texto:

`Name: Patrick Hines`

`Home Phone = 206-555-0144`

## <a name="example"></a>Exemplo

O exemplo a seguir declara `ns` como um prefixo de namespace XML. Em seguida, ele usa o prefixo do namespace para criar um literal XML e acessar o valor do primeiro nó filho com o nome qualificado `ns:name` .

[!code-vb[VbXMLSamples#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples12.vb#26)]

Esse código exibe o seguinte texto:

`Name: Patrick Hines`

## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement>
- [Propriedades do eixo XML](index.md)
- [Literais XML](../xml-literals/index.md)
- [Criando XML no Visual Basic](../../programming-guide/language-features/xml/creating-xml.md)
- [Nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
