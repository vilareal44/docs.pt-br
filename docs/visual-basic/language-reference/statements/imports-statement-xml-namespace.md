---
title: Instrução Imports-namespace XML
ms.date: 07/20/2015
f1_keywords:
- vb.ImportsXmlns
helpviewer_keywords:
- XML namespace [Visual Basic], importing
- imports [Visual Basic]
- Imports statement [Visual Basic]
- namespaces [Visual Basic], importing
ms.assetid: 1f4d50a6-08c7-4c2e-8206-ccae35fcd1b4
ms.openlocfilehash: a3184d68b0e4cdff5d4296a5a638e22b4e83bcde
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404531"
---
# <a name="imports-statement-xml-namespace"></a>Instrução Imports (namespace XML)

Importa prefixos de namespace XML para uso em literais XML e propriedades de eixo XML.

## <a name="syntax"></a>Sintaxe

```vb
Imports <xmlns:xmlNamespacePrefix = "xmlNamespaceName">
```

## <a name="parts"></a>Partes

`xmlNamespacePrefix`  
Opcional. A cadeia de caracteres pela qual os elementos e atributos XML podem se referir `xmlNamespaceName` . Se não `xmlNamespacePrefix` for fornecido, o namespace XML importado será o namespace XML padrão. Deve ser um identificador XML válido. Para obter mais informações, consulte [nomes de elementos XML declarados e atributos](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md).

`xmlNamespaceName`  
Obrigatórios. A cadeia de caracteres que identifica o namespace XML que está sendo importado.

## <a name="remarks"></a>Comentários

Você pode usar a `Imports` instrução para definir namespaces XML globais que você pode usar com literais XML e propriedades de eixo XML, ou como parâmetros passados para o `GetXmlNamespace` operador. (Para obter informações sobre como usar a `Imports` instrução para importar um alias que pode ser usado onde os nomes de tipos são usados em seu código, consulte a [instrução Imports (namespace e tipo do .net)](imports-statement-net-namespace-and-type.md).) A sintaxe para declarar um namespace XML usando a `Imports` instrução é idêntica à sintaxe usada em XML. Portanto, você pode copiar uma declaração de namespace de um arquivo XML e usá-la em uma `Imports` instrução.

Os prefixos de namespace XML são úteis quando você deseja criar repetidamente elementos XML que são do mesmo namespace. O prefixo de namespace XML declarado com a `Imports` instrução é global no sentido de que está disponível para todo o código no arquivo. Você pode usá-lo ao criar literais de elemento XML e ao acessar as propriedades do eixo XML. Para obter mais informações, consulte [literal do elemento XML](../xml-literals/xml-element-literal.md) e [Propriedades do eixo XML](../xml-axis/index.md).

Se você definir um namespace XML global sem um prefixo de namespace (por exemplo, `Imports <xmlns="http://SomeNameSpace>"` ), esse namespace será considerado o namespace XML padrão. O namespace XML padrão é usado para quaisquer literais de elemento XML ou propriedades de eixo de atributo XML que não especificam explicitamente um namespace. O namespace padrão também será usado se o namespace especificado for o namespace vazio (ou seja, `xmlns=""` ). O namespace XML padrão não se aplica a atributos XML em literais XML ou a propriedades do eixo de atributo XML que não têm um namespace.

Namespaces XML que são definidos em um literal XML, que são chamados de *namespaces XML locais*, têm precedência sobre namespaces XML que são definidos pela `Imports` instrução como global. Namespaces XML que são definidos pela `Imports` instrução têm precedência sobre namespaces XML importados para um projeto Visual Basic. Se um literal XML definir um namespace XML, esse namespace local não se aplicará a expressões inseridas.

Namespaces XML globais seguem as mesmas regras de escopo e definição que .NET Framework namespaces. Como resultado, você pode incluir uma `Imports` instrução para definir um namespace XML global em qualquer lugar em que você possa importar um namespace de .NET Framework. Isso inclui arquivos de código e namespaces importados no nível do projeto. Para obter informações sobre namespaces importados no nível do projeto, consulte [página de referências, designer de projeto (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic).

Cada arquivo de origem pode conter qualquer número de `Imports` instruções. Eles devem seguir as declarações de opção, como a `Option Strict` instrução, e devem preceder as declarações de elemento de programação, como `Module` `Class` instruções or.

## <a name="example"></a>Exemplo

O exemplo a seguir importa um namespace XML padrão e um namespace XML identificado com o prefixo `ns` . Em seguida, ele cria literais XML que usam ambos os namespaces.

[!code-vb[VbXMLSamples#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/Module1.vb#45)]

Esse código exibe o seguinte texto:

```xml
<ns:outer xmlns="http://DefaultNamespace"
          xmlns:ns="http://NewNamespace">
  <ns:innerElement></ns:innerElement>
  <siblingElement></siblingElement>
  <innerElement />
</ns:outer>
```

## <a name="example"></a>Exemplo

O exemplo a seguir importa o prefixo do namespace XML `ns` . Em seguida, ele cria um literal XML que usa o prefixo do namespace e exibe a forma final do elemento.

[!code-vb[VbXMLSamples#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples10.vb#22)]

Esse código exibe o seguinte texto:

```xml
<ns:outer xmlns:ns="http://SomeNamespace">
  <ns:middle xmlns:ns="http://NewNamespace">
    <ns:inner1 />
    <inner2 xmlns="http://SomeNamespace" />
  </ns:middle>
</ns:outer>
```

Observe que o compilador converteu o prefixo de namespace XML de um prefixo global em uma definição de prefixo local.

## <a name="example"></a>Exemplo

O exemplo a seguir importa o prefixo do namespace XML `ns` . Em seguida, ele usa o prefixo do namespace para criar um literal XML e acessar o primeiro nó filho com o nome qualificado `ns:name` .

[!code-vb[VbXMLSamples#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples8.vb#19)]

Esse código exibe o seguinte texto:

`Patrick Hines`

## <a name="see-also"></a>Confira também

- [Literal do Elemento XML](../xml-literals/xml-element-literal.md)
- [Propriedades do eixo XML](../xml-axis/index.md)
- [Nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
- [Operador GetXmlNamespace](../operators/getxmlnamespace-operator.md)
