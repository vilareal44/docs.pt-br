---
title: Resolvendo folhas de estilos XSLT e documentos externos
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 920cfe3b-d525-4bb2-abf6-9431651f9cf9
ms.openlocfilehash: 8e7f66d67f2520b47c30307a98ed2f3fb08455df
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291468"
---
# <a name="resolving-external-xslt-style-sheets-and-documents"></a>Resolvendo folhas de estilos XSLT e documentos externos
Há várias vezes durante uma transformação quando você precise resolver recursos externos.  
  
> [!NOTE]
> A classe <xref:System.Xml.Xsl.XslTransform> está obsoleta no .NET Framework 2.0. Você pode executar a linguagem XSL Transformations (XSLT) usando a classe <xref:System.Xml.Xsl.XslCompiledTransform>.  
  
 Há várias vezes durante uma transformação quando você precise resolver recursos externos:  
  
- Durante <xref:System.Xml.Xsl.XslTransform.Load%2A> para localizar uma folha de estilos externa.  
  
- Durante <xref:System.Xml.Xsl.XslTransform.Load%2A> para resolver alguns elementos de `<xsl:include>` ou de `<xsl:import>` localizados na folha de estilos.  
  
- Durante <xref:System.Xml.Xsl.XslTransform.Transform%2A> para resolver algumas funções de `document()` .  
  
## <a name="using-the-xmlresolver-class"></a>Usando a classe de XmlResolver  
 Se a autenticação for necessária para acessar um recurso de rede, use os métodos de <xref:System.Xml.Xsl.XslTransform.Load%2A> que têm um parâmetro de <xref:System.Xml.XmlResolver> para passar o objeto de <xref:System.Xml.XmlResolver> , que tem as propriedades credenciais necessárias definidas.  
  
 Se você tiver <xref:System.Xml.XmlResolver> personalizado que você deseja usar, ou se você precisar especificar credenciais diferentes, a tabela a seguir lista a tarefa necessária, como quando o recurso externo precisar a resolução.  
  
|Processo requer que a resolução|Tarefa necessária|  
|--------------------------------------|-------------------|  
|Durante <xref:System.Xml.Xsl.XslTransform.Load%2A> para localizar a folha de estilos.|Especificar o método sobrecarregado de <xref:System.Xml.Xsl.XslTransform.Load%2A> que aceita, como um parâmetro, <xref:System.Xml.XmlResolver> se a folha de estilos é um recurso que requer credenciais.|  
|Durante <xref:System.Xml.Xsl.XslTransform.Load%2A> para resolver `<xsl:include>` ou `<xsl:import>`.|Especificar o método sobrecarregado de <xref:System.Xml.Xsl.XslTransform.Load%2A> que aceita, como um parâmetro, <xref:System.Xml.XmlResolver>. <xref:System.Xml.XmlResolver> é usado para carregar as folhas de estilos referenciadas pelas declarações de `import` ou de `include` . Se você passar em `null`, os recursos externos não são resolvidos.|  
|Durante uma transformação resolver qualquer `document()` funciona.|Especifique <xref:System.Xml.XmlResolver> durante a transformação usando o método <xref:System.Xml.Xsl.XslTransform.Transform%2A> que recebe um argumento de <xref:System.Xml.XmlResolver>.|  
  
 A função `document()` recupera outros recursos XML de uma folha de estilos, além dos dados XML iniciais fornecidos pelo fluxo de entrada. Já que essa função permite a inclusão de dados XML que podem ser encontrados em outro lugar, <xref:System.Xml.XmlResolver> com um valor de `null` fornecido para o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> impede que a função de `document()` executar. Se você desejar usar a função de `document()` , use o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> que leva <xref:System.Xml.XmlResolver> como um parâmetro, além de ter o conjunto de permissões apropriado.  
  
 Para obter mais informações sobre o método de <xref:System.Xml.Xsl.XslTransform.Load%2A> e o uso de <xref:System.Xml.XmlResolver>, consulte <xref:System.Xml.Xsl.XslTransform.Load%28System.String%2CSystem.Xml.XmlResolver%29?displayProperty=nameWithType>.  
  
 Quando o método de <xref:System.Xml.Xsl.XslTransform.Transform%2A> é chamado, as permissões são calculadas com a evidência fornecida em tempo de carregamento, e esse conjunto de permissões é atribuído ao processo inteiro de transformação. Se a função de `document()` tentar iniciar uma ação que requer permissões não encontradas no dataset, uma exceção é lançada.  
  
## <a name="see-also"></a>Veja também

- [Transformações XSLT com a classe XslTransform](xslt-transformations-with-the-xsltransform-class.md)
- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
- [Saída de um XslTransform](outputs-from-an-xsltransform.md)
- [Transformações XSLT sobre diferentes armazena](xslt-transformations-over-different-stores.md)
- [XsltArgumentList para parâmetros de folha de estilos e objetos de extensão](xsltargumentlist-for-style-sheet-parameters-and-extension-objects.md)
- [Script de folha de estilos XSLT usando\<msxsl:script>](xslt-stylesheet-scripting-using-msxsl-script.md)
- [Suporte à função msxsl:node-set()](support-for-the-msxsl-node-set-function.md)
- [XPathNavigator nas transformações](xpathnavigator-in-transformations.md)
- [XPathNodeIterator nas transformações](xpathnodeiterator-in-transformations.md)
- [XPathDocument inseriu a XslTransform](xpathdocument-input-to-xsltransform.md)
- [XmlDataDocument inseriu a XslTransform](xmldatadocument-input-to-xsltransform.md)
- [XmlDocument inseriu a XslTransform](xmldocument-input-to-xsltransform.md)
