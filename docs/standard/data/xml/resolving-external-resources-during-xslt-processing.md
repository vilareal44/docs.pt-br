---
title: Resolvendo recursos externos durante processamento XSLT
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 3a59d31c-0ec5-4de6-a2a9-558531c8116e
ms.openlocfilehash: d38aea1a54c93b00ec14c6aac7ed11ceba288f7b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291494"
---
# <a name="resolving-external-resources-during-xslt-processing"></a>Resolvendo recursos externos durante processamento XSLT
Há várias vezes durante uma transformação XSLT quando você precise resolver recursos externos.  
  
## <a name="using-the-xmlresolver-class"></a>Usando a classe de XmlResolver  
 A classe de <xref:System.Xml.XmlResolver> é usada para resolver recursos externos. A tabela a seguir descreve quando <xref:System.Xml.XmlResolver> se torna envolvido durante processamento XSLT.  
  
|Tarefa XSLT|O que o XmlResolver é usado para|  
|---------------|--------------------------------------|  
|Compile a folha de estilos.|Resolver o URI de folha de estilos.<br /><br /> -e-<br /><br /> Resolver referências de URI em todos os elementos de `xsl:import` ou de `xsl:include` .|  
|Executar a folha de estilos.|Resolver o URI de documento de contexto.<br /><br /> -e-<br /><br /> Resolver referências de URI em todas as funções XSLT `document()` .|  
  
 Os métodos de <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> e de <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> incluem as sobrecargas que recebem um objeto de <xref:System.Xml.XmlResolver> como um dos argumentos. Se <xref:System.Xml.XmlResolver> não for especificado, <xref:System.Xml.XmlUrlResolver> padrão sem credenciais é usado.  
  
 A lista a seguir descreve quando você pode querer especificar um objeto de <xref:System.Xml.XmlResolver> :  
  
- Se o processo XSLT precisa acessar um recurso de rede que requer autenticação, você pode usar <xref:System.Xml.XmlResolver> com as credenciais necessárias.  
  
- Se você deseja restringir os recursos que o processo XSLT pode acessar, você pode usar <xref:System.Xml.XmlSecureResolver> com o conjunto de permissões correto. Use a classe de <xref:System.Xml.XmlSecureResolver> se você precisa abrir um recurso que você não controle, ou que é não confiável.  
  
- Se você desejar personalizar o comportamento, você pode implementar sua própria classe de <xref:System.Xml.XmlResolver> e usá-la para resolver recursos.  
  
- Se você quiser garantir que nenhum recurso externo é acessado, você pode especificar `null` para o argumento de <xref:System.Xml.XmlResolver> .  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria uma folha de estilos que é armazenada em um recurso de rede. Um objeto de <xref:System.Xml.XmlUrlResolver> especifica as credenciais necessárias para acessar a folha de estilos.  
  
 [!code-csharp[XslCompiledTransform.Load#11](../../../../samples/snippets/csharp/VS_Snippets_Data/XslCompiledTransform.Load/CS/Xslt_Load_v2.cs#11)]
 [!code-vb[XslCompiledTransform.Load#11](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslCompiledTransform.Load/VB/Xslt_Load_v2.vb#11)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.Xsl.XslCompiledTransform>
- <xref:System.Xml.Xsl.XsltSettings>
- [Transformações XSLT](xslt-transformations.md)
