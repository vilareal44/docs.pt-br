---
title: A classe XslTransform implementa do processador XSLT
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 88373fe2-4a6b-44f9-8a62-8a3e348e3a46
ms.openlocfilehash: eec5d6588d907e2d12b588ab3bfe743d6d1eaff9
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281603"
---
# <a name="xsltransform-class-implements-the-xslt-processor"></a>A classe XslTransform implementa do processador XSLT

> [!NOTE]
> A classe <xref:System.Xml.Xsl.XslTransform> está obsoleta no .NET Framework 2.0. Você pode executar a linguagem XSL Transformations (XSLT) usando a classe <xref:System.Xml.Xsl.XslCompiledTransform>. Confira [Usar a classe XslCompiledTransform](using-the-xslcompiledtransform-class.md) e [Migrar da classe XslTransform](migrating-from-the-xsltransform-class.md) para saber mais.

A classe de <xref:System.Xml.Xsl.XslTransform> é um processador XSLT que implementa a recomendação de versão 1,0 de transformações XSL (XSLT). O método de <xref:System.Xml.Xsl.XslTransform.Load%2A> localiza e ler as folhas de estilos, e o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> torna o documento de origem determinado. Qualquer armazenamento que implemente a interface de <xref:System.Xml.XPath.IXPathNavigable> pode ser usado como o documento de origem para <xref:System.Xml.Xsl.XslTransform>. O .NET Framework atualmente implementa a interface de <xref:System.Xml.XPath.IXPathNavigable> em <xref:System.Xml.XmlDocument>, em <xref:System.Xml.XmlDataDocument> e em <xref:System.Xml.XPath.XPathDocument>, para que todos estes pode ser usado como o documento de fonte de entrada para uma transformação.

O objeto de <xref:System.Xml.Xsl.XslTransform> no .NET Framework dá suporte somente à especificação XSLT 1,0, definida com a seguir namespace:

```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">
```

A folha de estilos pode ser carregada, usando o método <xref:System.Xml.Xsl.XslTransform.Load%2A> , de uma das seguintes classes:

- XPathNavigator

- XmlReader

- Uma cadeia de caracteres que representa um URL

Há um método diferente de <xref:System.Xml.Xsl.XslTransform.Load%2A> para cada uma das classes acima de entrada. Alguns métodos recolhem uma combinação de uma dessas classes e a classe de <xref:System.Xml.XmlResolver> como argumentos. <xref:System.Xml.XmlResolver> encontrar os recursos referenciados por `<xsl:import>` ou `<xsl:include>` encontrado na folha de estilos. Os seguintes métodos recebem uma cadeia de caracteres, <xref:System.Xml.XmlReader>, ou <xref:System.Xml.XPath.XPathNavigator> como entrada.

```vb
Overloads Public Sub Load(String)
```

```csharp
public void Load(string);
```

```vb
Overloads Public Sub Load(String, XmlResolver)
```

```csharp
public void Load(string, XmlResolver);
```

```vb
Overloads Public Sub Load(XmlReader, XmlResolver, Evidence)
```

```csharp
public void Load(XmlReader, XmlResolver, Evidence);
```

```vb
Overloads Public Sub Load(XPathNavigator, XmlResolver, Evidence)
```

```csharp
public void Load(XPathNavigator, XmlResolver, Evidence);
```

A maioria dos métodos de <xref:System.Xml.Xsl.XslTransform.Load%2A> mostradas acima recebem <xref:System.Xml.XmlResolver> como um parâmetro. <xref:System.Xml.XmlResolver> é usado para carregar a folha de estilos e qualquer folha de estilos referenciadas em XSL: importação e XSL: inclua elementos.

A maioria dos métodos de <xref:System.Xml.Xsl.XslTransform.Load%2A> também têm a evidência como um parâmetro. O parâmetro de evidência é <xref:System.Security.Policy.Evidence> que está associado com a folha de estilos. O nível de segurança de folha de estilo afeta o nível de segurança todos os recursos que subsequentes referência, como o script contém, quaisquer funções de `document()` usa, e todos os objetos de extensão usados por <xref:System.Xml.Xsl.XsltArgumentList>.

Se a folha de estilos é carregada usando um método de <xref:System.Xml.Xsl.XslTransform.Load%2A> que não contenha um parâmetro de URL e nenhuma evidência é fornecida, a evidência de folha de estilos é calculada pelo URL dado com suas site e zona.

Se nenhum URI ou evidência é fornecido, então a evidência definida para a folha de estilos é totalmente confiável. Não carregar folhas de estilos de fontes não confiáveis, ou adicionar objetos não confiáveis de extensão em <xref:System.Xml.Xsl.XsltArgumentList>.

Para obter mais informações sobre níveis de segurança e evidências e como ele afeta o script, consulte [scripting de \<msxsl:script> folha de estilos XSLT usando ](xslt-stylesheet-scripting-using-msxsl-script.md). Para saber mais sobre os níveis e evidência de segurança, e como isso afeta os objetos de extensão, confira [XsltArgumentList para parâmetros de folha de estilos e objetos de extensão](xsltargumentlist-for-style-sheet-parameters-and-extension-objects.md).

Para saber mais sobre níveis e evidência de segurança, e como isso afeta a função `document()`, confira [Resolvendo folhas de estilos XSLT e documentos externos](resolving-external-xslt-style-sheets-and-documents.md).

Uma folha de estilos pode ser fornecida com um número de parâmetros. A folha de estilos também pode chamar funções em objetos de extensão. Os parâmetros e objetos de extensão são fornecidos para a folha de estilos usando a classe de <xref:System.Xml.Xsl.XsltArgumentList> . Para obter mais informações sobre <xref:System.Xml.Xsl.XsltArgumentList>, consulte <xref:System.Xml.Xsl.XsltArgumentList>.

## <a name="recommended-secure-use-of-xsltransform-class"></a>Seguro uso recomendado da classe XslTransform

Os privilégios de segurança de folha de estilos depende evidências fornecida. A tabela a seguir resume o local de folha de estilos e fornece uma explicação do tipo de evidência para dar.

- A folha de estilos XSLT não tem nenhuma referência externo, ou a folha de estilos vem de uma base de código que você confie.

  - Fornecer a evidência do seu conjunto:

    ```vb
    Dim xslt = New XslTransform() xslt.Load(stylesheet, resolver, Me.GetType().Assembly.Evidence)

    XsltTransform xslt = new XslTransform();  xslt.Load(stylesheet, resolver, this.GetType().Assembly.Evidence);
    ```

- A folha de estilos XSLT vem de uma fonte externa. A origem de origem é conhecida e há URI verificável.

  - Crie a evidência usando um URI.

    ```vb
    Dim xslt As New XslTransform() Dim ev As Evidence = XmlSecureResolver.CreateEvidenceForUrl(stylesheetUri) xslt.Load(stylesheet, resolver, evidence)

    XslTransform xslt = new XslTransform(); Evidence ev = XmlSecureResolver.CreateEvidenceForUrl(stylesheetUri); xslt.Load(stylesheet, resolver, evidence);
    ```

- A folha de estilos XSLT vem de uma fonte externa. A origem de origem não é conhecida.

  - Defina a evidência a `null`. Blocos de script não são processados, a função XSLT `document()` não é suportada, e objetos privilegiados de extensão não é permitida.

    Além disso, você pode também definir o parâmetro de `resolver` a `null` que isso garante que `xsl:import` e elementos de `xsl:include` não sejam processadas.

- A folha de estilos XSLT vem de uma fonte externa. A origem de origem não é conhecido, mas você precisar de suporte de script.

  - Evidência de solicitação do chamador.

## <a name="transformation-of-xml-data"></a>Transformação de dados XML

Uma vez que uma folha de estilos é carregada, a transformação chamando um dos métodos de <xref:System.Xml.Xsl.XslTransform.Transform%2A> e fornecendo um documento de fonte de entrada. O método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> é sobrecarregado para fornecer saída diferente de transformação. A transformação pode levar a seguir formata de saída:

- <xref:System.Xml.XmlReader>

- <xref:System.Xml.XmlWriter>

- <xref:System.IO.TextWriter>

- <xref:System.IO.Stream>

- URL de cadeia de caracteres de arquivo

Esse último formato, o URL de cadeia de caracteres, fornece um cenário de uso geral em transformar um documento de entrada localizado em um URL e gravar no documento a URL de saída. Este método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> é um método conveniente para carregar um documento XML de um arquivo, para executar a transformação XSLT, e para escrever a saída para um arquivo. Isso impede que você tem que criar e carregar o documento de fonte de entrada, e grava a um fluxo de arquivos. O exemplo de código a seguir mostra o uso do método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> usando o URL de cadeia de caracteres como entrada e saída:

```vb
Dim xsltransform As XslTransform = New XslTransform()
xsltransform.Load("favorite.xsl")
xsltransform.Transform("MyDocument.Xml", "TransformResult.xml", Nothing)
```

```csharp
XslTransform xsltransform = new XslTransform();
xsltransform.Load("favorite.xsl");
xsltransform.Transform("MyDocument.xml", "TransformResult.xml", null);
```

## <a name="transforming-a-section-of-an-xml-document"></a>Transformando uma seção de um documento XML

As transformações são aplicadas ao documento no dataset. Ou seja se você passar em um nó que não seja o nó de diretório base, isso não impede que o processo de transformação acessar todos os nós do documento carregado. Para transformar um fragmento da árvore de resultado, você deve criar <xref:System.Xml.XmlDocument> que contém apenas o fragmento da árvore de resultado e passe o <xref:System.Xml.XmlDocument> para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> . O exemplo a seguir executa uma transformação em um fragmento da árvore de resultado.

```vb
Dim xslt As New XslTransform()
xslt.Load("print_root.xsl")
Dim doc As New XmlDocument()
doc.Load("library.xml")
' Create a new document containing just the result tree fragment.
Dim testNode As XmlNode = doc.DocumentElement.FirstChild
Dim tmpDoc As New XmlDocument()
tmpDoc.LoadXml(testNode.OuterXml)
' Pass the document containing the result tree fragment
' to the Transform method.
Console.WriteLine(("Passing " + tmpDoc.OuterXml + " to print_root.xsl"))
xslt.Transform(tmpDoc, Nothing, Console.Out, Nothing)
```

```csharp
XslTransform xslt = new XslTransform();
xslt.Load("print_root.xsl");
XmlDocument doc = new XmlDocument();
doc.Load("library.xml");
// Create a new document containing just the result tree fragment.
XmlNode testNode = doc.DocumentElement.FirstChild;
XmlDocument tmpDoc = new XmlDocument();
tmpDoc.LoadXml(testNode.OuterXml);
// Pass the document containing the result tree fragment
// to the Transform method.
Console.WriteLine("Passing " + tmpDoc.OuterXml + " to print_root.xsl");
xslt.Transform(tmpDoc, null, Console.Out, null);
```

O exemplo usa os arquivos library. xml e print_root. xsl como entrada e gera o seguinte para o console:

```console
Passing <book genre="novel" ISBN="1-861001-57-5"><title>Pride And Prejudice</title></book> to print_root.xsl
Root node is book.
```

library.xml

```xml
<library>
  <book genre='novel' ISBN='1-861001-57-5'>
     <title>Pride And Prejudice</title>
  </book>
  <book genre='novel' ISBN='1-81920-21-2'>
     <title>Hook</title>
  </book>
</library>
```

print_root.xsl

```xml
<stylesheet version="1.0" xmlns="http://www.w3.org/1999/XSL/Transform" >
  <output method="text" />
  <template match="/">
     Root node is  <value-of select="local-name(//*[position() = 1])" />
  </template>
</stylesheet>
```

## <a name="migration-of-xslt-from-net-framework-version-10-to-net-framework-version-11"></a>Migração XSLT do .NET Framework versão 1,0 para a versão 1,1 do .NET Framework

A tabela a seguir mostra os métodos obsoletos do .NET Framework versão 1.0 e a nova versão dos métodos do .NET Framework 1.1 para o método <xref:System.Xml.Xsl.XslTransform.Load%2A>. Novos métodos permitem que você limitar as permissões de folha de estilos especificando a evidência.

|Métodos obsoletos de carregamento do .NET Framework versão 1.0|Métodos de carregamento do .NET Framework versão 1.1 de substituição|
|------------------------------------------------------|---------------------------------------------------------|
|Carregamento XPathNavigator conectado (;)<br /><br /> Carregando (entrada de XPathNavigator, resolvedor de XmlResolver);|Carregando (folha de estilos de XPathNavigator, resolvedor de XmlResolver, evidência de evidência);|
|Carregando (folha de estilos de IXPathNavigable);<br /><br /> Carregando (folha de estilos de IXPathNavigable, resolvedor de XmlResolver);|Carregando (folha de estilos de IXPathNavigable, resolvedor de XmlResolver, evidência de evidência);|
|Carregando (folha de estilos de XmlReader);<br /><br /> Carregando (folha de estilos de XmlReader, resolvedor de XmlResolver);|Carregando (folha de estilos de XmlReader, resolvedor de XmlResolver, evidência de evidência);|

A tabela a seguir mostra os métodos obsoletos e novos para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> . Novos métodos recebem um objeto de <xref:System.Xml.XmlResolver> .

|Métodos obsoletos de transformação do .NET Framework versão 1.0|Métodos de transformação da versão 1.1 do .NET Framework de substituição|
|-----------------------------------------------------------|--------------------------------------------------------------|
|XmlReader transformação (entrada de XPathNavigator, os args de XsltArgumentList)|XmlReader transformação (entrada de XPathNavigator, args de XsltArgumentList, o resolvedor de XmlResolver)|
|XmlReader transformação (entrada de IXPathNavigable, os args de XsltArgumentList)|XmlReader transformação (entrada de IXPathNavigable, args de XsltArgumentList, o resolvedor de XmlResolver)|
|O vácuo transformação (entrada de XPathNavigator, args de XsltArgumentList, a saída de XmlWriter)|O vácuo transformação (entrada de XPathNavigator, args de XsltArgumentList, a saída de XmlWriter, o resolvedor de XmlResolver)|
|O vácuo transformação (entrada de IXPathNavigable, args de XsltArgumentList, a saída de XmlWriter)|O vácuo transformação (entrada de IXpathNavigable, args de XsltArgumentList, a saída de XmlWriter, o resolvedor de XmlResolver)|
|O vácuo transformação (entrada de XPathNavigator, args de XsltArgumentList, a saída de Textreader)|O vácuo transformação (entrada de XPathNavigator, args de XsltArgumentList, a saída de Textreader, o resolvedor de XmlResolver)|
|O vácuo transformação (entrada de IXPathNavigable, args de XsltArgumentList, a saída de Textreader)|O vácuo transformação (entrada de IXPathNavigable, args de XsltArgumentList, a saída de Textreader, o resolvedor de XmlResolver)|
|O vácuo transformação (entrada de XPathNavigator, args de XsltArgumentList, a saída de fluxo)|O vácuo transformação (entrada de XPathNavigator, args de XsltArgumentList, a saída de fluxo, o resolvedor de XmlResolver)|
|O vácuo transformação (entrada de IXPathNavigable, args de XsltArgumentList, a saída de fluxo)|O vácuo transformação (entrada de IXPathNavigable, args de XsltArgumentList, a saída de fluxo, o resolvedor de XmlResolver)|
|O vácuo transformação (entrada de cadeia de caracteres, a saída de cadeia de caracteres);|O vácuo transformação (entrada de cadeia de caracteres, a saída de cadeia de caracteres, resolvedor de XmlResolver);|

A propriedade <xref:System.Xml.Xsl.XslTransform.XmlResolver%2A?displayProperty=nameWithType> está obsoleta no .NET Framework versão 1.1. Em vez disso, use as novas sobrecargas <xref:System.Xml.Xsl.XslTransform.Transform%2A> que usam um objeto <xref:System.Xml.XmlResolver>.

## <a name="see-also"></a>Veja também

- <xref:System.Xml.Xsl.XslTransform>
- [Transformações XSLT com a classe XslTransform](xslt-transformations-with-the-xsltransform-class.md)
- [XPathNavigator nas transformações](xpathnavigator-in-transformations.md)
- [XPathNodeIterator nas transformações](xpathnodeiterator-in-transformations.md)
- [XPathDocument inseriu a XslTransform](xpathdocument-input-to-xsltransform.md)
- [XmlDataDocument inseriu a XslTransform](xmldatadocument-input-to-xsltransform.md)
- [XmlDocument inseriu a XslTransform](xmldocument-input-to-xsltransform.md)
