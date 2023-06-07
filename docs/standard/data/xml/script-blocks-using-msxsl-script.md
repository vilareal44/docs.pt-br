---
title: Blocos de script usando msxsl:script
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: fde6f43f-c594-486f-abcb-2211197fae20
ms.openlocfilehash: e65308f097e81d844cb04b1ebd5cbcdd8a3aadad
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291988"
---
# <a name="script-blocks-using-msxslscript"></a>Blocos de script usando msxsl:script
A classe <xref:System.Xml.Xsl.XslCompiledTransform> oferece suporte a scripts inserido usando o elemento `msxsl:script`. Quando a folha de estilos é carregada, todas as funções definidas são compiladas para Microsoft Intermediate Language (MSIL) pelo Code Document Object Model (CodeDOM) e executadas durante o tempo de execução. O assembly gerado no bloco de script inserido é separado do assembly gerado para a folha de estilos.  
  
## <a name="enable-xslt-script"></a>Habilitar scripts XSLT  
 O suporte a scripts inseridos é uma configuração XSLT opcional na classe <xref:System.Xml.Xsl.XslCompiledTransform>. O suporte a script é desabilitado por padrão. Para habilitar o suporte a script, crie um objeto <xref:System.Xml.Xsl.XsltSettings> com a propriedade <xref:System.Xml.Xsl.XsltSettings.EnableScript%2A> definida para `true` e passe o objeto para o método <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A>.  
  
> [!NOTE]
> O script XSLT deverá ser habilitado somente se você precisar de suporte a script e estiver trabalhando em um ambiente totalmente confiável.  
  
## <a name="msxslscript-element-definition"></a>Definição do elemento msxsl:script  
 O elemento `msxsl:script` é uma extensão da Microsoft para a recomendação XSLT 1.0 e tem a seguinte definição:  
  
```xml  
<msxsl:script language = "language-name" implements-prefix = "prefix of user namespace"> </msxsl:script>  
```  
  
 O prefixo `msxsl` é associado ao URI do namespace `urn:schemas-microsoft-com:xslt`. A folha de estilos deve incluir a declaração de namespace `xmlns:msxsl=urn:schemas-microsoft-com:xslt`.  
  
 O atributo `language` é opcional. Seu valor é a linguagem de código do bloco de código inserido. O idioma é mapeado para o compilador CodeDOM apropriado que usa o método <xref:System.CodeDom.Compiler.CodeDomProvider.CreateProvider%2A?displayProperty=nameWithType>. A classe <xref:System.Xml.Xsl.XslCompiledTransform> pode oferecer suporte a qualquer linguagem do Microsoft .NET, supondo que o provedor apropriado esteja instalado no computador e registrado na seção de system.codedom do arquivo machine.config. Se o atributo `language` não for especificado, a linguagem assume JScript como padrão. O nome da linguagem não diferencia maiúsculas de minúsculas; portanto, 'JavaScript' e 'javascript' são equivalentes.  
  
 O atributo `implements-prefix` é obrigatório. Esse atributo é usado para declarar um namespace e associá-lo ao bloco de script. O valor desse atributo é o prefixo que representa o namespace. Este prefixo pode ser definido em qualquer lugar de uma folha de estilos.  
  
> [!NOTE]
> Ao usar o elemento `msxsl:script`, é altamente recomendável que o script, independentemente da linguagem, seja colocado em uma seção CDATA. Como o script pode conter operadores, identificadores ou delimitadores para uma linguagem específica, caso ele não esteja em uma seção CDATA, ele podes ser interpretado incorretamente como XML. O XML a seguir mostra um modelo da seção CDATA em que o código pode ser inserido.  
  
```xml  
<msxsl:script implements-prefix='your-prefix' language='CSharp'>  
<![CDATA[  
// Code block.  
]]>  
</msxsl:script>  
```  
  
## <a name="script-functions"></a>Funções de script  
 As funções podem ser declaradas no elemento `msxsl:script`. Quando uma função é declarada, ela está contida em um bloco de script. As folhas de estilos podem conter vários blocos de script, cada uma funcionando de maneira independente da outra. Isso significa que, se você estiver realizando a execução em um bloco de script, não poderá chamar uma função definida em outro bloco de script, a menos que tenha sido declarado que ela tem o mesmo namespace e a mesma linguagem de script. Como cada bloco de script pode ter sua própria linguagem, e o bloco é analisado de acordo com as regras de gramática do analisador de linguagem, é recomendável utilizar a sintaxe correta para a linguagem em uso. Por exemplo, se você estiver em um bloco de script do Microsoft C#, use a sintaxe de comentário C#.  
  
 Os argumentos e valores de retorno fornecidos para a função podem ser de qualquer tipo. Como os tipos W3C XPath são um subconjunto dos tipos de Common Language Runtime (CLR), a conversão de tipo ocorre nos tipos que não são considerados um tipo XPath. A tabela a seguir mostra os tipos W3C correspondentes e o tipo CLR equivalente.  
  
|Tipo W3C|Tipo CLR|  
|--------------|--------------|  
|`String`|<xref:System.String>|  
|`Boolean`|<xref:System.Boolean>|  
|`Number`|<xref:System.Double>|  
|`Result Tree Fragment`|<xref:System.Xml.XPath.XPathNavigator>|  
|`Node Set`|<xref:System.Xml.XPath.XPathNodeIterator>|  
  
 Os tipos numéricos de CLR são convertidos em <xref:System.Double>. O tipo <xref:System.DateTime> é convertido em <xref:System.String>. Os tipos <xref:System.Xml.XPath.IXPathNavigable> são convertidos em <xref:System.Xml.XPath.XPathNavigator>. **XPathNavigator[]** é convertido em <xref:System.Xml.XPath.XPathNodeIterator>.  
  
 Todos os outros tipos lançam um erro.  
  
### <a name="importing-namespaces-and-assemblies"></a>Importando namespaces e assemblies  
 A classe <xref:System.Xml.Xsl.XslCompiledTransform> predefine um conjunto de assemblies e namespaces com suporte, por padrão, no elemento `msxsl:script`. No entanto, você pode usar as classes e os membros pertencentes a um namespace que não está na lista predefinida importando o assembly e o namespace no bloco `msxsl:script`.  
  
#### <a name="assemblies"></a>Assemblies  
 Os dois assemblies seguintes são referenciados por padrão:  
  
- System.dll  
  
- System.Xml.dll  
  
- Microsoft.VisualBasic.dll (quando a linguagem de script for VB)  
  
 Você pode importar os assemblies adicionais usando o elemento `msxsl:assembly`. Isso inclui o assembly quando a folha de estilos é compilada. O elemento `msxsl:assembly` tem a seguinte definição:  
  
```xml  
<msxsl:script>  
  <msxsl:assembly name="system.assemblyName" />  
  <msxsl:assembly href="path-name" />  
    <![CDATA[  
    // User code  
    ]]>  
</msxsl:script>  
```  
  
 O atributo `name` contém o nome do assembly e o atributo `href` contém o caminho para o assembly. O nome do assembly pode ser um nome completo, como "System.Data, Version=2.0.3600.0, Culture=neutral, PublicKeyToken=b77a5c561934e089", ou um nome curto, como "System.Web".  
  
#### <a name="namespaces"></a>Namespaces  
 Os seguintes namespaces são incluídos por padrão:  
  
- Sistema  
  
- System.Collection  
  
- System.Text  
  
- System.Text.RegularExpressions  
  
- System.Xml  
  
- System.Xml.Xsl  
  
- System.Xml.XPath  
  
- Microsoft.VisualBasic (quando a linguagem de script for VB)  
  
 Você pode adicionar suporte a namespaces adicionais usando o atributo `namespace`. O valor do atributo é o nome do namespace.  
  
```xml  
<msxsl:script>  
  <msxsl:using namespace="system.namespaceName" />  
    <![CDATA[  
    // User code  
    ]]>  
</msxsl:script>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa um script inserido para calcular a circunferência de um círculo considerando o seu raio.  
  
 [!code-csharp[XSLT_Script#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XSLT_Script/CS/xslt_script.cs#1)]
 [!code-vb[XSLT_Script#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XSLT_Script/VB/xslt_script.vb#1)]  
  
#### <a name="numberxml"></a>number.xml  
 [!code-xml[XSLT_Script#2](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Script/XML/number.xml#2)]  
  
#### <a name="calcxsl"></a>calc.xsl  
 [!code-xml[XSLT_Script#3](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Script/XML/calc.xsl#3)]  
  
### <a name="output"></a>Saída  
  
```xml  
<circles xmlns:msxsl="urn:schemas-microsoft-com:xslt" xmlns:user="urn:my-scripts">  
  <circle>  
    <radius>12</radius>  
    <circumference>75.36</circumference>  
  </circle>  
  <circle>  
    <radius>37.5</radius>  
    <circumference>235.5</circumference>  
  </circle>  
</circles>  
```  
  
## <a name="see-also"></a>Veja também

- [Transformações XSLT](xslt-transformations.md)
- [Geração e compilação de código-fonte dinâmico](../../../framework/reflection-and-codedom/dynamic-source-code-generation-and-compilation.md)
