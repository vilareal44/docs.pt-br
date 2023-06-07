---
title: Literal do Elemento XML
ms.date: 07/20/2015
f1_keywords:
- vb.XmlLiteralElement
helpviewer_keywords:
- XML element literal [Visual Basic]
- element literal [Visual Basic]
- XML literals [Visual Basic], element
ms.assetid: 95039642-7893-48b7-b23f-45a6c55d8f67
ms.openlocfilehash: d6a91de4e279816bafd29f46bb4f5422cbd934ff
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400183"
---
# <a name="xml-element-literal-visual-basic"></a>Literal do elemento XML (Visual Basic)

Um literal que representa um <xref:System.Xml.Linq.XElement> objeto.

## <a name="syntax"></a>Sintaxe

```xml
<name [ attributeList ] />
-or-
<name [ attributeList ] > [ elementContents ] </[ name ]>
```

## <a name="parts"></a>Partes

- `<`

  Obrigatórios. Abre a marca do elemento inicial.

- `name`

  Obrigatórios. Nome do elemento. O formato é um dos seguintes:

  - Texto literal para o nome do elemento, do formulário `[ePrefix:]eName` , em que:

    |Parte|Descrição|
    |---|---|
    |`ePrefix`|Opcional. Prefixo do namespace XML para o elemento. Deve ser um namespace XML global que é definido com uma `Imports` instrução no arquivo ou no nível do projeto, ou um namespace XML local que é definido neste elemento ou em um elemento pai.|
    |`eName`|Obrigatórios. Nome do elemento. O formato é um dos seguintes:<br /><br /> -Texto literal. Consulte [nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md).<br />-Expressão inserida do formulário `<%= eNameExp %>` . O tipo de `eNameExp` deve ser `String` ou um tipo que é implicitamente conversível <xref:System.Xml.Linq.XName> .|

  - Expressão inserida do formulário `<%= nameExp %>` . O tipo de `nameExp` deve ser `String` ou um tipo implicitamente conversível para <xref:System.Xml.Linq.XName> . Uma expressão inserida não é permitida em uma marca de fechamento de um elemento.

- `attributeList`

  Opcional. Lista de atributos declarados no literal.

  `attribute [ attribute ... ]`

  Cada `attribute` uma tem uma das seguintes sintaxes:

  - Atribuição de atributo, do formulário `[aPrefix:]aName=aValue` , em que:

    |Parte|Descrição|
    |---|---|
    |`aPrefix`|Opcional. Prefixo do namespace XML para o atributo. Deve ser um namespace XML global que é definido com uma `Imports` instrução ou um namespace XML local que é definido neste elemento ou um elemento pai.|
    |`aName`|Obrigatórios. Nome do atributo. O formato é um dos seguintes:<br /><br /> -Texto literal. Consulte [nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md).<br />-Expressão inserida do formulário `<%= aNameExp %>` . O tipo de `aNameExp` deve ser `String` ou um tipo que é implicitamente conversível <xref:System.Xml.Linq.XName> .|
    |`aValue`|Opcional. Valor do atributo. O formato é um dos seguintes:<br /><br /> -Texto literal, entre aspas.<br />-Expressão inserida do formulário `<%= aValueExp %>` . Qualquer tipo é permitido.|

  - Expressão inserida do formulário `<%= aExp %>` .

- `/>`

  Opcional. Indica que o elemento é um elemento vazio, sem conteúdo.

- `>`

  Obrigatórios. Termina a marca de elemento inicial ou vazia.

- `elementContents`

  Opcional. Conteúdo do elemento.

  `content [ content ... ]`

  Cada `content` um pode ser um dos seguintes:

  - Texto literal. Todo o espaço em branco no `elementContents` torna-se significativo se houver qualquer texto literal.

  - Expressão inserida do formulário `<%= contentExp %>` .

  - Literal de elemento XML.

  - Literal de comentário XML. Consulte [literal de comentário XML](xml-comment-literal.md).

  - Literal de instrução de processamento XML. Consulte [literal de instrução de processamento XML](xml-processing-instruction-literal.md).

  - Literal XML CDATA. Consulte [literal XML CDATA](xml-cdata-literal.md).

- `</[name]>`

  Opcional. Representa a marca de fechamento do elemento. O `name` parâmetro opcional não é permitido quando é o resultado de uma expressão inserida.

## <a name="return-value"></a>Valor Retornado

Um objeto <xref:System.Xml.Linq.XElement>.

## <a name="remarks"></a>Comentários

Você pode usar a sintaxe literal do elemento XML para criar <xref:System.Xml.Linq.XElement> objetos em seu código.

> [!NOTE]
> Um literal XML pode abranger várias linhas sem usar caracteres de continuação de linha. Esse recurso permite que você copie o conteúdo de um documento XML e cole-o diretamente em um programa Visual Basic.

As expressões inseridas do formulário `<%= exp %>` permitem que você adicione informações dinâmicas a um literal de elemento XML. Para obter mais informações, consulte [expressões inseridas em XML](../../programming-guide/language-features/xml/embedded-expressions-in-xml.md).

O compilador Visual Basic converte o literal do elemento XML em chamadas para o <xref:System.Xml.Linq.XElement.%23ctor%2A> Construtor e, se necessário, o <xref:System.Xml.Linq.XAttribute.%23ctor%2A> Construtor.

## <a name="xml-namespaces"></a>Namespaces XML

Os prefixos de namespace XML são úteis quando você precisa criar literais XML com elementos do mesmo namespace muitas vezes no código. Você pode usar prefixos de namespace XML globais, que você define usando a `Imports` instrução ou prefixos locais, que você define usando a `xmlns:xmlPrefix="xmlNamespace"` sintaxe de atributo. Para obter mais informações, consulte [instrução Imports (namespace XML)](../statements/imports-statement-xml-namespace.md).

De acordo com as regras de escopo para namespaces XML, os prefixos locais têm precedência sobre prefixos globais. No entanto, se um literal XML definir um namespace XML, esse namespace não estará disponível para expressões que aparecem em uma expressão inserida. A expressão inserida pode acessar apenas o namespace XML global.

O compilador Visual Basic converte cada namespace XML global que é usado por um literal XML em uma definição de namespace local no código gerado. Namespaces XML globais que não são usados não aparecem no código gerado.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como criar um elemento XML simples que tem dois elementos vazios aninhados.

[!code-vb[VbXMLSamples#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples9.vb#20)]

O exemplo exibe o texto a seguir. Observe que o literal preserva a estrutura dos elementos vazios.

```xml
<outer>
  <inner1></inner1>
  <inner2 />
</outer>
```

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar expressões inseridas para nomear um elemento e criar atributos.

[!code-vb[VbXMLSamples#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples9.vb#21)]

Esse código exibe o seguinte texto:

```xml
<book isbn="1234" author="My Author" year="1999" title="My Book" />
```

## <a name="example"></a>Exemplo

O exemplo a seguir declara `ns` como um prefixo de namespace XML. Em seguida, ele usa o prefixo do namespace para criar um literal XML e exibe a forma final do elemento.

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

Observe que o compilador converteu o prefixo do namespace XML global em uma definição de prefixo para o namespace XML. O \<ns:middle> elemento redefine o prefixo do namespace XML para o \<ns:inner1> elemento. No entanto, o \<ns:inner2> elemento usa o namespace definido pela `Imports` instrução.

## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement>
- [Nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
- [Literal de Comentário XML](xml-comment-literal.md)
- [Literal CDATA XML](xml-cdata-literal.md)
- [Literais XML](index.md)
- [Criando XML no Visual Basic](../../programming-guide/language-features/xml/creating-xml.md)
- [Expressões inseridas no XML](../../programming-guide/language-features/xml/embedded-expressions-in-xml.md)
- [Instrução Imports (namespace XML)](../statements/imports-statement-xml-namespace.md)
