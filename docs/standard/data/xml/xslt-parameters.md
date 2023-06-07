---
title: Parâmetros XSLT
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: fe60aaa0-ae43-4b1c-9be1-426af66ba757
ms.openlocfilehash: 7651360b375071c48ba0d23b64881ac794e51e86
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84282526"
---
# <a name="xslt-parameters"></a>Parâmetros XSLT
Os parâmetros XSLT são adicionados a <xref:System.Xml.Xsl.XsltArgumentList> usando o método <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> . Um nome qualificado e URI de namespace são associados com o objeto de parâmetro no momento.  
  
### <a name="to-use-an-xslt-parameter"></a>Para usar um parâmetro XSLT  
  
1. Crie um objeto de <xref:System.Xml.Xsl.XsltArgumentList> e adicione o parâmetro usando o método <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> .  
  
2. Chame o parâmetro folha de estilos.  
  
3. Passe o objeto de <xref:System.Xml.Xsl.XsltArgumentList> para o método de <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> .  
  
## <a name="parameter-types"></a>Tipos de parâmetro  
 O objeto de parâmetro deve corresponder a um tipo W3C. A tabela seguinte mostra tipos correspondentes W3C, as classes equivalentes do Microsoft.NET (tipo), e se o tipo W3C é um tipo XPath ou tipo de fonte.  
  
|Tipo W3C|Classe. NET equivalente (tipo)|XPath ou tipo XSLT|  
|--------------|------------------------------------|------------------------|  
|`String`|<xref:System.String?displayProperty=nameWithType>|XPath|  
|`Boolean`|<xref:System.Boolean?displayProperty=nameWithType>|XPath|  
|`Number`|<xref:System.Double?displayProperty=nameWithType>|XPath|  
|`Result Tree Fragment`|<xref:System.Xml.XPath.XPathNavigator?displayProperty=nameWithType>|XSLT|  
|`Node*`|<xref:System.Xml.XPath.XPathNavigator?displayProperty=nameWithType>|XPath|  
|`Node Set`|<xref:System.Xml.XPath.XPathNodeIterator><br /><br /> **XPathNavigator []**|XPath|  
  
 os *This são equivalentes a um nó definida que contém um único nó.  
  
 Se o objeto de parâmetro não é uma das classes anterior, ele é convertido de acordo com as regras a seguir. Os tipos numéricos do Common Language Runtime (CLR) são convertidos a <xref:System.Double>. O tipo <xref:System.DateTime> é convertido em <xref:System.String>. Os tipos <xref:System.Xml.XPath.IXPathNavigable> são convertidos em <xref:System.Xml.XPath.XPathNavigator>. **XPathNavigator[]** é convertido em <xref:System.Xml.XPath.XPathNodeIterator>.  
  
 Todos os outros tipos lançam um erro.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o método de <xref:System.Xml.Xsl.XsltArgumentList.AddParam%2A> para criar um parâmetro para armazenar calculou a data de desconto. A data de desconto é calculada para ser a 20 dias de data pedido.  
  
 [!code-csharp[XSLT_Param#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XSLT_Param/CS/xsltparam.cs#1)]
 [!code-vb[XSLT_Param#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XSLT_Param/VB/xsltparam.vb#1)]  
  
### <a name="input"></a>Entrada  
  
##### <a name="orderxml"></a>order.xml  
 [!code-xml[XSLT_Param#2](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Param/XML/order.xml#2)]  
  
##### <a name="discountxsl"></a>discount.xsl  
 [!code-xml[XSLT_Param#3](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_Param/XML/discount.xsl#3)]  
  
### <a name="output"></a>Saída  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<order>  
  <total>36.9</total>  
     15% discount if paid by: 2/4/2004 12:00:00 AM  
</order>  
```  
  
## <a name="see-also"></a>Veja também

- [Transformações XSLT](xslt-transformations.md)
