---
title: Script de folha de estilos XSLT usando <msxsl:script>
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 60e2541b-0cea-4b2e-a4fa-85f4c50f1bef
ms.openlocfilehash: aef2471a375469f7cd4dff27084b305ef9394d5e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291962"
---
# <a name="xslt-stylesheet-scripting-using-msxslscript"></a>Script de folha de estilos XSLT usando\<msxsl:script>
A classe <xref:System.Xml.Xsl.XslTransform> dá suporte a scripts inserido usando o elemento `script`.  
  
> [!NOTE]
> A classe <xref:System.Xml.Xsl.XslTransform> está obsoleta no .NET Framework 2.0. Você pode executar a linguagem XSL Transformations (XSLT) usando a classe <xref:System.Xml.Xsl.XslCompiledTransform>. Confira [Usar a classe XslCompiledTransform](using-the-xslcompiledtransform-class.md) e [Migrar da classe XslTransform](migrating-from-the-xsltransform-class.md) para saber mais.  
  
 A classe <xref:System.Xml.Xsl.XslTransform> dá suporte a scripts inserido usando o elemento `script`. Quando a folha de estilos é carregada, todas as funções definidas são compiladas no Microsoft Intermediate Language (MSIL) sendo empacotadas em uma definição de classe e sem perda de desempenho como resultado.  
  
 O elemento `<msxsl:script>` é definido abaixo:  
  
```xml  
<msxsl:script language = "language-name" implements-prefix = "prefix of user namespace"> </msxsl:script>  
```  
  
 onde `msxsl` é um prefixo associado ao namespace `urn:schemas-microsoft-com:xslt`.  
  
 O `language` atributo não é obrigatório, mas, se especificado, seu valor deve ser um dos seguintes: `C#` , `VB` , `JScript` , `JavaScript` , `VisualBasic` ou `CSharp` . Se não for especificado, a linguagem padrão é JScript. O `language-name` não diferencia maiúsculas de minúsculas, portanto “JavaScript” e “Javascript” são equivalentes.  
  
 O atributo `implements-prefix` é obrigatório. Esse atributo é usado para declarar um namespace e associá-lo ao bloco de script. O valor desse atributo é o prefixo que representa o namespace. Este namespace pode ser definido em qualquer lugar em uma folha de estilos.  
  
 Como o elemento `msxsl:script` pertence ao namespace `urn:schemas-microsoft-com:xslt`, a folha de estilos deverá incluir a declaração de namespace `xmlns:msxsl=urn:schemas-microsoft-com:xslt`.  
  
 Se o chamador do script não tem permissão de acesso <xref:System.Security.Permissions.SecurityPermissionFlag>, o script em uma folha de estilos nunca compilará e a chamada a <xref:System.Xml.Xsl.XslTransform.Load%2A> falhará.  
  
 Se o chamador tiver a permissão de `UnmanagedCode`, o script será compilado, mas as operações que são permitidas são dependentes da evidência que é fornecida em tempo de carregamento.  
  
 Se você estiver usando um dos métodos <xref:System.Xml.Xsl.XslTransform.Load%2A> que recebem <xref:System.Xml.XmlReader> ou <xref:System.Xml.XPath.XPathNavigator> para carregar a folha de estilos, você precisará usar a sobrecarga <xref:System.Xml.Xsl.XslTransform.Load%2A> que utiliza um parâmetro <xref:System.Security.Policy.Evidence> como um dos argumentos. Para fornecer a evidência, o chamador deverá ter a permissão <xref:System.Security.Permissions.SecurityPermissionFlag> para fornecer `Evidence` ao assembly de script. Se o chamador não tiver essa permissão, eles poderão definir o parâmetro `Evidence` como `null`. Isso faz a função <xref:System.Xml.Xsl.XslTransform.Load%2A> falhar se encontrar o script. A permissão `ControlEvidence` é considerada uma permissão muito eficiente que deve ser concedida apenas a código altamente confiável.  
  
 Para obter a evidência do assembly, use `this.GetType().Assembly.Evidence`. Para obter a evidência de um Uniform Resource Identifier (URI), use `Evidence e = XmlSecureResolver.CreateEvidenceForUrl(stylesheetURI)`.  
  
 Se você usar os métodos <xref:System.Xml.Xsl.XslTransform.Load%2A> que utilizam um <xref:System.Xml.XmlResolver> mas nenhum `Evidence`, a zona de segurança para o assembly usará como padrão a confiança total. Para saber mais, veja <xref:System.Security.SecurityZone> e [Conjuntos de permissões nomeadas](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/4652tyx7(v=vs.100)).  
  
 As funções podem ser declaradas no elemento `msxsl:script`. A tabela a seguir mostra os namespaces que têm suporte por padrão. Você pode usar as classes fora dos namespaces listados. No entanto, essas classes devem ser totalmente qualificadas.  
  
|Namespaces padrão|Description|  
|------------------------|-----------------|  
|Sistema|Classe do sistema.|  
|System.Collection|Classes de coleção.|  
|System.Text|Classes de texto.|  
|System.Text.RegularExpressions|Classes de expressão regular.|  
|System.Xml|Classes XML principais.|  
|System.Xml.Xsl|Classes XSLT.|  
|System.Xml.XPath|Classes da linguagem XPath.|  
|Microsoft.VisualBasic|Classes para scripts do Microsoft Visual Basic.|  
  
 Quando uma função é declarada, ela está contida em um bloco de script. As folhas de estilos podem conter vários blocos de script, cada uma funcionando de maneira independente da outra. Isso significa que, se você estiver realizando a execução em um bloco de script, não poderá chamar uma função definida em outro bloco de script, a menos que tenha sido declarado que ela tem o mesmo namespace e a mesma linguagem de script. Como cada bloco de script pode estar em sua própria linguagem, e o bloco é analisado de acordo com as regras de gramática do analisador de linguagem, você deverá usar a sintaxe correta para a linguagem em uso. Por exemplo, se você estiver em um bloco de script C#, é um erro usar um nó de comentário XML `<!-- an XML comment -->` no bloco.  
  
 Os argumentos fornecidos e os valores de retorno definidos pelas funções de script devem ser do tipo XPath do W3C (World Wide Web Consortium) ou XSLT. A tabela a seguir mostra os tipos correspondentes de W3C, as classes equivalentes do .NET Framework (tipo) e se o tipo do W3C é XPath ou XSLT.  
  
|Tipo|Classe equivalente do .NET Framework (tipo)|Tipo XPath ou XSLT|  
|----------|----------------------------------------------|-----------------------------|  
|String|System.String|XPath|  
|Boolean|System.Boolean|XPath|  
|Número|System.Double|XPath|  
|Fragmento da árvore de resultado|System.Xml.XPath.XPathNavigator|XSLT|  
|Node Set|System.Xml.XPath.XPathNodeIterator|XPath|  
  
 Se a função de script utilizar um dos seguintes tipos numéricos: Int16, UInt16, Int32, UInt32, Int64, UInt64, Single ou Decimal, eles serão forçados para Double, que mapeia para o número do tipo XPath do W3C. Todos os outros tipos são forçados para uma cadeia de caracteres chamando o método `ToString`.  
  
 Se a função de script utilizar um tipo diferente dos mencionados acima, ou se a função não compilar quando a folha de estilos for carregada no objeto <xref:System.Xml.Xsl.XslTransform>, uma exceção será gerada.  
  
 Ao usar o elemento`msxsl:script`, é altamente recomendável que o script, independentemente da linguagem, seja colocado em uma seção CDATA. Por exemplo, o XML a seguir mostra o modelo de seção CDATA onde o código é colocado.  
  
```xml  
<msxsl:script implements-prefix='yourprefix' language='CSharp'>  
    <![CDATA[  
    ... your code here ...  
    ]]>  
</msxsl:script>  
```  
  
 É altamente recomendável que todo o conteúdo de script seja colocado em uma seção CDATA, porque os operadores, os identificadores ou os delimitadores para uma determinada linguagem têm o potencial de serem mal interpretados como XML. O exemplo a seguir mostra o uso do operador lógico AND no script.  
  
```xml  
<msxsl:script implements-prefix='yourprefix' language='CSharp'>  
    public string book(string abc, string xyz)  
    {  
        if ((abc == bar) && (abc == xyz)) return bar + xyz;  
        else return null;  
    }  
</msxsl:script>  
```  
  
 Isso gera uma exceção porque o E comercial não escapa. O documento é carregado como XML e nenhum tratamento especial é aplicado ao texto entre as marcas de elemento de `msxsl:script`.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa um script inserido para calcular a circunferência de um círculo considerando o seu raio.  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
Imports System.Xml.XPath  
Imports System.Xml.Xsl  
  
Public Class Sample  
  
   Private Const filename As String = "number.xml"  
   Private Const stylesheet As String = "calc.xsl"  
  
   Public Shared Sub Main()  
  
    'Create the XslTransform and load the style sheet.  
    Dim xslt As XslTransform = New XslTransform  
    xslt.Load(stylesheet)  
  
    'Load the XML data file.  
    Dim doc As XPathDocument = New XPathDocument(filename)  
  
    'Create an XmlTextWriter to output to the console.
    Dim writer As XmlTextWriter = New XmlTextWriter(Console.Out)  
    writer.Formatting = Formatting.Indented  
  
    'Transform the file.  
    xslt.Transform(doc, Nothing, writer, Nothing)  
    writer.Close()  
  End Sub
End Class  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
using System.Xml.XPath;  
using System.Xml.Xsl;  
  
public class Sample  
{  
   private const String filename = "number.xml";  
   private const String stylesheet = "calc.xsl";  
  
   public static void Main()  
   {  
    //Create the XslTransform and load the style sheet.  
    XslTransform xslt = new XslTransform();  
    xslt.Load(stylesheet);  
  
    //Load the XML data file.  
    XPathDocument doc = new XPathDocument(filename);  
  
    //Create an XmlTextWriter to output to the console.
    XmlTextWriter writer = new XmlTextWriter(Console.Out);  
    writer.Formatting = Formatting.Indented;  
  
    //Transform the file.  
    xslt.Transform(doc, null, writer, null);  
    writer.Close();  
  }  
}  
```  
  
## <a name="input"></a>Entrada  
 number.xml  
  
```xml  
<?xml version='1.0'?>  
<data>  
  <circle>  
    <radius>12</radius>  
  </circle>  
  <circle>  
    <radius>37.5</radius>  
  </circle>  
</data>  
```  
  
 calc.xsl  
  
```xml  
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
    xmlns:msxsl="urn:schemas-microsoft-com:xslt"  
    xmlns:user="urn:my-scripts">  
  
  <msxsl:script language="C#" implements-prefix="user">  
     <![CDATA[  
     public double circumference(double radius)  
     {  
       double pi = 3.14;  
       double circ = pi*radius*2;  
       return circ;  
     }  
      ]]>  
   </msxsl:script>  
  
  <xsl:template match="data">
  <circles>  
  
  <xsl:for-each select="circle">  
    <circle>  
    <xsl:copy-of select="node()"/>  
       <circumference>  
          <xsl:value-of select="user:circumference(radius)"/>
       </circumference>  
    </circle>  
  </xsl:for-each>  
  </circles>  
  </xsl:template>  
</xsl:stylesheet>  
```  
  
## <a name="output"></a>Saída  
  
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

- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
