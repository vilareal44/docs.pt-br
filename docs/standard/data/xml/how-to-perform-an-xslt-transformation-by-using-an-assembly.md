---
title: 'Como: Executar uma transformação XSLT usando um assembly'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 76ee440b-d134-4f8f-8262-b917ad6dcbf6
ms.openlocfilehash: 623f997d1c11bc643ea4605614cac147b6069be5
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287721"
---
# <a name="how-to-perform-an-xslt-transformation-by-using-an-assembly"></a>Como: Executar uma transformação XSLT usando um assembly
O compilador XSLT (xsltc.exe) compila folhas de estilos XSLT e gera um assembly. O assembly pode ser passado diretamente no método <xref:System.Xml.Xsl.XslCompiledTransform.Load%28System.Type%29?displayProperty=nameWithType>.  
  
### <a name="to-copy-the-xml-and-xslt-files-to-your-local-computer"></a>Para copiar os arquivos XML e XSLT para seu computador local  
  
- Copie o arquivo XSLT para seu computador local e nomeie-o Transform.xsl.  
  
    ```xml  
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
      xmlns:msxsl="urn:schemas-microsoft-com:xslt"  
      xmlns:user="urn:my-scripts">  
      <msxsl:script language="C#" implements-prefix="user">  
        <![CDATA[  
      public string discount(string price){  
        char[] trimChars = { '$' };  
        //trim leading $, convert price to type double  
        double discount_value = Convert.ToDouble(price.TrimStart(trimChars));  
        //apply 10% discount and round appropriately  
        discount_value = .9*discount_value;  
        //convert value to decimal and format as currency  
        string discount_price = discount_value.ToString("C");  
        return discount_price;  
      }  
      ]]>  
      </msxsl:script>  
      <xsl:template match="catalog">  
        <html>  
          <head></head>  
          <body>  
            <table border="1">  
              <tr>  
                <th align="left">Title</th>  
                <th align="left">Author</th>  
                <th align="left">Genre</th>  
                <th align="left">Publish Date</th>  
                <th align="left">Price</th>  
              </tr>  
              <xsl:for-each select="book">  
                <tr>  
                  <td>  
                    <xsl:value-of select="title"/>  
                  </td>  
                  <td>  
                    <xsl:value-of select="author"/>  
                  </td>  
                  <td>  
                    <xsl:value-of select="genre"/>  
                  </td>  
                  <td>  
                    <xsl:value-of select="publish_date"/>  
                  </td>  
                  <xsl:choose>  
                    <xsl:when test="genre = 'Fantasy'">  
                      <td>  
                        <xsl:value-of select="user:discount(price)"/>  
                      </td>  
                    </xsl:when>  
                    <xsl:otherwise>  
                      <td>  
                        <xsl:value-of select="price"/>  
                      </td>  
                    </xsl:otherwise>  
                  </xsl:choose>  
                </tr>  
              </xsl:for-each>  
            </table>  
          </body>  
        </html>  
      </xsl:template>  
    </xsl:stylesheet>  
    ```  
  
- Copie o arquivo XML para seu computador local e nomeie-o `books.xml`.  
  
    ```xml  
    <?xml version="1.0"?>  
    <catalog>  
       <book id="bk101">  
          <author>Gambardella, Matthew</author>  
          <title>XML Developer's Guide</title>  
          <genre>Computer</genre>  
          <price>$44.95</price>  
          <publish_date>2000-10-01</publish_date>  
       </book>  
       <book id="bk102">  
          <author>Ralls, Kim</author>  
          <title>Midnight Rain</title>  
          <genre>Fantasy</genre>  
          <price>$5.95</price>  
          <publish_date>2000-12-16</publish_date>  
       </book>  
       <book id="bk103">  
          <author>Corets, Eva</author>  
          <title>Maeve Ascendant</title>  
          <genre>Fantasy</genre>  
          <price>$5.95</price>  
          <publish_date>2000-11-17</publish_date>  
       </book>  
       <book id="bk106">  
          <author>Randall, Cynthia</author>  
          <title>Lover Birds</title>  
          <genre>Romance</genre>  
          <price>$4.95</price>  
          <publish_date>2000-09-02</publish_date>  
       </book>  
       <book id="bk107">  
          <author>Thurman, Paula</author>  
          <title>Splish Splash</title>  
          <genre>Romance</genre>  
          <price>$4.95</price>  
          <publish_date>2000-11-02</publish_date>  
       </book>  
    </catalog>  
    ```  
  
### <a name="to-compile-the-style-sheet-with-the-script-enabled"></a>Para compilar a folha de estilos com o script habilitado.  
  
1. A execução do comando a seguir a partir da linha de comando cria dois assemblies nomeados `Transform.dll` e `Transform_Script1.dll` (Esse é o comportamento padrão. A menos que de outra forma especificado, o nome da classe e do assembly usa como padrão o nome da folha de estilos principal):  
  
    ```console  
    xsltc /settings:script+ Transform.xsl  
    ```
  
    O comando a seguir define explicitamente o nome da classe como Transform:  
  
    ```console  
    xsltc /settings:script+ /class:Transform Transform.xsl  
    ```  
  
### <a name="to-include-the-compiled-assembly-as-a-reference-when-you-compile-your-code"></a>Para incluir o assembly compilado como uma referência ao compilar o código.  
  
1. Você pode incluir um assembly no Visual Studio adicionando uma referência do Gerenciador de Soluções ou da linha de comando.  
  
2. Para a linha de comando com C#, use o seguinte:  
  
    ```console  
    csc myCode.cs /r:system.dll;system.xml.dll;Transform.dll  
    ```  
  
3. Para a linha de comando com Visual Basic, use o seguinte:  
  
    ```console  
    vbc myCode.vb /r:system.dll;system.xml.dll;Transform.dll  
    ```  
  
### <a name="to-use-the-compiled-assembly-in-your-code"></a>Para usar o assembly compilado em seu código.  
  
O exemplo a seguir mostra como executar a transformação XSLT usando a folha de estilos compilada.  
  
[!code-csharp[XslTransform_XSLTC#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XslTransform_XSLTC/CS/XslTransform_XSLTC.cs#1)]
[!code-vb[XslTransform_XSLTC#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslTransform_XSLTC/VB/XslTransform_XSLTC.vb#1)]  
  
Para vincular ao assembly compilado dinamicamente, substitua
  
```csharp  
xslt.Load(typeof(Transform));  
```  
  
por  
  
```csharp
xslt.Load(System.Reflection.Assembly.Load("Transform").GetType("Transform"));  
```
  
no exemplo anterior. Para obter mais informações sobre o método assembly. Load, consulte <xref:System.Reflection.Assembly.Load%2A> .  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.Xsl.XslCompiledTransform>
- [Compilador de XSLT (xsltc.exe)](xslt-compiler-xsltc-exe.md)
- [Transformações XSLT](xslt-transformations.md)
- [Compilando pela linha de comando com csc.exe](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md)
